---
layout: post
title: Prevent 404s for missing uploads in your rails app
---
Every software developer has to deal with bugs in their apps and reproducing
those. Some of these cases require a production database clone.
In that case you'd probably just pull the most recent database backup from
production and use it on your local machine.
What is missing in those backups though, are the images uploaded by your users.
This can be very annoying especially if you work on stuff like a media center
which i did recently.
To prevent those 404 errors from popping up in your browser's developer console, while at
the same time not needing to store gigabytes from user uploaded content
in your locale filesystem, a rack middleware can help you.

{% gist Crunch09/c6dd7928c8ef64a30148 uploads_fallback.rb %}

As you can see in the gist above, we define a middleware called *UploadsFallback*
and check in *Line 8* if the current request is requesting a missing uploaded
image. If that is the case we'll return a fallback image to the client instead.

Lastly we need to tell Rails to load our *UploadsFallback* middleware. As we
only want this to happen in **development** we add the following line to the
*/config/environments/development.rb* file.

{% gist Crunch09/c6dd7928c8ef64a30148 development.rb %}

Restart your app and 404s for missing uploads are gone.
