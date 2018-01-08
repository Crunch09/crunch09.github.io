---
layout: post
title: Hacktoberfest 2017
---
[Hacktoberfest](https://hacktoberfest.digitalocean.com/) is a month long event
(in October as hinted by the name) organised by [DigitalOcean](https://www.digitalocean.com/) and [Github](https://github.com/) to encourage people to contribute to open source.
Basically they ask to create at least 4 pull requests to public repositories
within this one month to earn yourself a free t-shirt and do good for the community.

The 2017 edition has been the third Hacktoberfest that I participated in. In
addition to the official rules I added some personal goals before the event started:
- Contribute to projects which I never contributed to previously
- Use four different languages (at least two of them should be unfamiliar to me)
- Obviously this is not my call but I wanted to try as hard as I can to make sure that the PR will be accepted and merged by the maintainers of each project

## Starting in Ruby

The plan for the first PR was to contribute in a language that I'm already very familiar with: Ruby. Via [Github's explore site](https://github.com/explore) I looked for projects which would be interesting to contribute to: They should have at least 100 stars, actively maintained and needed to have a fair amount of open issues.
Because the project should be new to me I couldn't just add a feature to an unknown project.

After some search I decided to try to contribute to the [changelog generator](https://github.com/skywinder/github-changelog-generator).
It seemed like a neat project and had an open issue which seemed important to a bunch of people to be fixed:
[skywinder/github-changelog-generator#555](https://github.com/skywinder/github-changelog-generator/issues/555).

It took me a while to become familiar enough with the codebase to figure out what
was going on but eventually I was able to come up with a patch which was accepted
shortly after: [skywinder/github-changelog-generator#566](https://github.com/skywinder/github-changelog-generator/pull/566).

![Hacktoberfest T-Shirt 2015]({{ "/assets/hacktoberfest_2015.jpg" | absolute_url }})

## Off we go(lang)!

Now it was time to create a PR in a language that I haven't used so far.
I chose Go because I've been interested in that for some time.

Given I had very, very limited knowledge of Go I knew that I first had to learn some basic skills before being able to contribute some code to an established project.
Googling for tutorials I ended up using the [tour of Go](https://tour.golang.org/welcome/1) on the official go website.
It took me 2 or 3 evenings to work through it but at the end I felt confident enough to look for an issue on a go project that I could tackle.

Taking the same approach of finding a repo to contribute to as in my first PR I ended up with [toxiproxy](https://github.com/shopify/toxiproxy), which is a project from [Shopify](https://shopify.com) that helps in testing the reliability of distributed systems by disabling dependencies.

The issue I tried to tackle was about giving the CLI a better error message if an
invalid URL was provided: [Shopify/toxiproxy#174](https://github.com/Shopify/toxiproxy/issues/174).

[Jake](https://github.com/jpittis), one of the maintainers of toxiproxy, provided some
really good feedback to a Go noob like me
and at the end also [this pull request](https://github.com/Shopify/toxiproxy/pull/191) got merged and I was 2 for 2! 💪

![Hacktoberfest T-Shirt 2016]({{ "/assets/hacktoberfest_2016.jpg" | absolute_url }})

## All 🆕 in JavaScript

Having already submitted PRs in Ruby and Go it was time to use a third language.
After my endeavour into Go I wanted to use a language again that I felt somewhat comfortable.

I chose JavaScript, because I knew it already but was nowhere near as confident in it as in Ruby, especially in regards to it's newish features like ES6 and the entire npm ecosystem.

This time I didn't look for projects via the Github explore site, instead I tried
to contribute to a project that I saw a couple of times on my twitter feed: [probot](https://github.com/probot).
It's an automation tool that helps maintainers on Github with certain tasks like
asking issue reporters for more information on their reports.

I looked through the recently update repositories on the probot organisation and
I found a suitable open issue in the [stale](https://github.com/probot/stale) repository:
[probot/stale#52](https://github.com/probot/stale/issues/52).

This project didn't have tests yet but I felt unsure about my JavaScript skills
from the good old jQuery days so I looked for other probot plugins within the
same organisation that included tests and tried to replicate their tests.

This process taught me about modern JavaScript and tools like [jest](https://github.com/facebook/jest)
which I never heard of before but now used for testing my JS code. I was able to
create [a pull request](https://github.com/probot/stale/pull/70) and the maintainer
was really happy with it so I quickly got my
third contribution for the Hacktoberfest goal merged! 🙌

![Probot pull request feedback]({{ "/assets/probot_pr_feedback.png" | absolute_url }})


## Compromises

For the fourth pull request I wanted to contribute in an unknown language again. While I was still
debating whether I should go with Rust or Elixir I realised that I had almost
reached the end of Hacktoberfest and because I was also expecting guests for a couple
of days I realised that I wouldn't be able to learn enough in a new language before the end of the month to be able to contribute. I came to the conclusion that I should focus on a PR in familiar territory (aka "Ruby") instead.

With that in my mind I saw in one of our [Deliveroo](https://deliveroo.engineering) slack channels that someone
had reported an issue on our rails bootstrap project [roo_on_rails](https://github.com/deliveroo/roo_on_rails).

It's open-source so it was the perfect opportunity to help my coworkers while also achieving my Hacktoberfest goal: win-win! 🎉
It took me only some investigating into the `ActiveSupport::Logger` class from Rails
and then I could submit my final pull request:
[deliveroo/roo_on_rails#77](https://github.com/deliveroo/roo_on_rails/pull/77).

## Summary

Overall even though I didn't quite achieve my initial personal goal I am still really happy about this years Hacktoberfest. I got to know new projects, new languages and each maintainer seemed really
happy about my contribution.

A few weeks ago I received the new shirt for achieving this goal which is always
a really nice reward!


![Hacktoberfest T-Shirt 2017]({{ "/assets/hacktoberfest_2017.jpg" | absolute_url }})

💖 to Github, DigitalOcean and all projects I contributed to this year!
 I am already looking forward to the Hacktoberfest 2018 edition!
