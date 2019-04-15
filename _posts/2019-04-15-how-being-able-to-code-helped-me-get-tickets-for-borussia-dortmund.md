---
layout: post
title: How being able to code helped me get tickets for Borussia Dortmund
---

17.Dec 2018: Champions league draw for the round of 16 and my favourite team,
Borussia Dortmund, is in the pot of the group winners. As I'm living in London
at the moment I rarely see them live so I'm hoping for an english team to be
drawn as our opponent. And it works out: Dortmund will face Tottenham and the
first leg will be played in their temporary home, Wembley Stadium.

16.Jan 2019: Borussia Dortmund starts their pre-sale for away tickets at
Tottenham. UEFA rules dictate that the visitors are allocated 5% of the total
capacity which is about 4,500 tickets at Wembley stadium.
Not all of those tickets get into the pre-sale though. Borussia Dortmund
allocates a non-specified number of tickets to away season ticket holders,
fanclubs, sponsors and VIPs.

The pre-sale of tickets always works in the same way: At exactly 8.30am german
time on the specified day you have to call the ticket hotline, talk to an IVR,
enter your credentials like membership number and hopefully purchase tickets to
the game.

Because of Dortmund's massive fan base (we've got the largest average home
attendance in all of Europe for a few years now) you have to be on the line
right on time.
And by right on time I mean that if you're a single second too late the hotline
is overloaded and you miss out on tickets.

The pre-sale started at 7.30am and as always I tried to time my call to be
exactly on the line at 07:30:00am. It didn't work. I called the hotline 101
times that morning only to hear that the game has been sold out.

😞 What now?

Buying overprized tickets on stubhub or ebay was not an option for me. As was
buying a ticket from Tottenham outside of the away sector. I already did this
for our away game at Atletico Madrid, a few months earlier. While it was great
to be in the stadium at all, it was hard to see my friends in the away
sector and not being able to support the team with them.

Then I remembered that sometimes a few tickets suddenly become available on the
hotline again. The reason is not entirely clear to me but I imagine some
supporters don't have enough money on their bank account or fanclubs need less
tickets than previously allocated.
Whatever the reason is, when tickets do become available again it's not
announced anywhere. You just have to keep calling and hope that you catch the
right moment.

I knew that I wouldn't be able to call multiple times per day while being at
work but I can code so I was wondering if I could automate this.

From a project at my current job I knew a bit about Twilio and I was wondering
if I would be able to replicate the call procedure.

The sequence of steps when calling the hotline is always the same.
*(Translated and shortened for the sake of this blog post, the
actual announcements are in german and add a bit more information):*

> IVR: Welcome to the Borussia Dortmund ticket hotline. Do you know this
service already?

> Me: Yes!

> IVR: Do you have a membership at Borussia Dortmund?

> Me: Yes!

> IVR: Please start by entering 0 on your keyboard <beep>

> Me: \*Enters 0\*

> IVR: Please enter your membership number <beep>

> Me: \*Enters membership number\*

> IVR: Please enter your zip code <beep>

> Me: \*Enters zip code\*

> IVR: Thanks. I can offer you tickets for the following games. When I said the game of your choice please say "STOP" loud and clearly.

> IVR:     \*Announces games\*


From this procedure it was clear to me what I needed to be able to automate:
- Transmit speech at specified point during phone call
- Transmit keyboard input at specified point during phone call
- Record announced games
- Transcribe recording to find out if Tottenham tickets are available

I wanted to create the project in Go but as Twilio doesn't have an SDK for Go
I needed to send HTTP requests to Twilio's API endpoints instead. Twilio created a set of instructions called [TwiML](https://www.twilio.com/docs/voice/twiml) which allows you to specify commands which should be executed on certain
events like making a phone call or receiving an SMS.

A phone call is very easy to initiate, just use the TwiML verb
[TWIML's <Call\> verb](https://www.twilio.com/docs/voice/twiml/call) with
a previously bought number. As the number of the hotline is a restricted number
I needed to make sure that I allow calls to the hotline in my Twilio settings.

Once that was done it was important to figure out when to send the specific
commands. The way to find this out was to just call the ticket hotline and
write done the times on when to say what.

After this I could use
[TWIML's <Pause\> verb](https://www.twilio.com/docs/voice/twiml/pause) to
simulate a wait time between executing my actual commands.

As described in the transcript above a caller has to say
things and also enter credentials on the keyboard. For these
tasks I used the TwiML's verb
[<Say\>](https://www.twilio.com/docs/voice/twiml/say),which fortunately also
allows to specify non-english language, and the TwiML verb
[<Play\>](https://www.twilio.com/docs/voice/twiml/play) to transmit digits.

Once I finally arrived at the game announcement stage I needed to start
recording what was being said so I used TwiML's
[<Record\>](https://www.twilio.com/docs/voice/twiml/record) for that. It is
important to follow legal requirements when doing this but luckily I was just
recording a bot :)

Given all that the final TwiML looks something like that:

{% gist Crunch09/3fdcb2643a855e316764fb84359bee62 twiml_bvb.xml %}

At the specified callback route I received the URL to my recording. I needed to do some research to find a service that could transcribe a recording in german.
While Twilio offers transcribing of recordings themselves it's only available in
english. My next idea was AWS but at the time of my project their service
[Amazon Transcribe](https://aws.amazon.com/transcribe/) also didn't support
German.
So I went to Google Cloud's offering and luckily their [Google speech](https://cloud.google.com/speech-to-text/) service offered transcription support for German.

There are multiple ways to submit a recording to Google but the easiest way
seemed to be to download it from Twilio and submit it as base64 - encoded string to Twilio's API. An [episode](https://www.youtube.com/watch?v=uIA1s3Rhpv8) of the Go youtube channel [Justforfunc](https://www.youtube.com/channel/UC_BzFbxG2za3bp5NRRRXJSw) really helped me to figure out
the correct encoding options so Google could understand the bytes correctly.

{% gist Crunch09/3fdcb2643a855e316764fb84359bee62 transcribe_recording.go %}

When the string `Tottenham` is included in the transcript I send myself a text
to let me know to call and buy them. Also whenever no games at all were included
in my transcript I send myself a text to update the length of the pauses as the
text of the IVR had changed then and disrupted the execution of my commands.
This was a bit annoying but luckily only happened a couple of times.

As I was already using Google's Voice API I decided to also host the code on
Google Cloud. This was also a good learning exercise as I previously only used
AWS. I used [Google App engine](https://cloud.google.com/appengine/docs/go/) and
it worked quite well.

One of the important things I had to set up was a cron schedule that issues a
call regularly.

{% gist Crunch09/3fdcb2643a855e316764fb84359bee62 cron.yaml %}

As I quickly learned the hotline is closed at night so I didn't have to run my code then.

1.Feb 2019: My bot was running fine for about 10 days now and finally it
happened around 1pm, I got an SMS confirming that tickets were available again!

I couldn't really believe it until I called myself but tickets were
actually available and I bought two of them in the Dortmund sector! Needless to
say that I was very happy that day :)
To make all those calls and then transcribe them cost me around £30 in addition
to the tickets for the game but it was all so worth it.

![Best SMS ever]({{ "/assets/sms.jpg" | absolute_url }})

Quickly I informed two of my friends who were also still looking for tickets.
While one of them got my message quickly and was also able to buy tickets, the
other one only called a half hour later when tickets were already sold out
again.

![Tottenhame Ticket]({{ "/assets/tottenham_ticket.jpg" | absolute_url }})


13.Feb 2019: Finally the day has come, Dortmund is in town! Of course I took
the day off and after some pre-game drinks and a march through Wembley we
finally entered the away sector.

After a good performance in the first half Dortmund ended up losing 3:0 (and
three weeks later would be kicked out of the Champions League after also losing
the second leg at home) but it was still a fun night as I can't see my team live
that much anymore.

![Away sector]({{ "/assets/bvb_fans_tottenham.jpg" | absolute_url }})


## What's next

As of this moment Dortmund is still fighting for the german championship being
second in place with just one point behind league leaders Bayern Munich and
again I couldn't get tickets for the last two away games of the season so I
reactivated my bot and hope that I'll be successful this time as well!
