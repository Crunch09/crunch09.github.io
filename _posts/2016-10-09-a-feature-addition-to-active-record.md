---
layout: post
title: A feature addition to ActiveRecord - left_joins
---
This post describes the journey of how i made a feature addition to ActiveRecord.
It took two years after the creation of the initial PullRequest before it was
finally ready to merge so it seems only appropriate that i write this blog post
a year after it got merged :laughing:

##### The problem

While working on [Tabulatr](https://github.com/metaminded/tabulatr2) i noticed
that there were two options for creating a *left_outer_joins* in ActiveRecord:

- joins
- includes

{{site.url}}

Both options have some downside: `joins` is creating `inner` joins by default
and the user has to write pure SQL if one wants to create an `outer` joins instead.
`includes` on the other hand creates an `outer` joins by default but you can't
override the selected columns because Rails will use it's custom selections anyway.

##### The fix
