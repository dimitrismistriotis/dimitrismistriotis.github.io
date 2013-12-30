---
layout: default
title: It’s Fontastic!
---

# It’s Fontastic!

## Life with Fontastic

It is very common nowdays for a project to use tons of open  source software from different sources as well as tools, frameworks, services, platforms. One unsung hero is fontastic, let’s give some justice with a blog post.

## What it is

Using their punch line, it can “Create your Icon Font in seconds”. Icons can be either selected from a good articulated collection of ready to use ones available, or by uploading an existing SVG. The application creates a zip file with the result as well as installation and embedding instructions.

## Where we use it

We initially started by using it to generate the icons for our footer:

![fontastic icons on footer](/images/fontastic_icons-300_108.png "fontastic icons on footer")


We also started to use font icons within the application, such as with the “reply indicator” on a user’s inbox:

![fontastic inbox](/images/fontastic_in_inbox-545.png "fontastic inbox")

We will use more font-based icons in the future, hence the utility and the dependency on fontastic, will increase.

Font icons are used for a variety of reasons. Specially for Cypsel:

**Scaling**, because vector graphics scale well across different devices, there was confidence that we did not had to create a matrix of icons per device size. This was a huge time saver in our very early development days, which allowed us to use resources to ease other equally important user interface pains.

A **communication tool** for the whole team: At some point three different people located each in different timezone, had to decide which linked-in icon was more suitable. Nothing easier than click all the linked-in icons within the ones available, capture a screenshot from the preview page and then email it.

Availability of concentrated well recognized icons in **one location**. Either because the famous font-awesome font was included, or the choice of vectors is optimal? Fontawesome is a one-stop shop for most modern UI needs, as in the “reply” case displayed above.

There is a **speed factor**. Vectors are smaller than images (technically they are a small xml file), hence they make the experience slightly faster, creating less network overhead and server utilization.

## Room for improvement

The idea for this post was from a email requesting for feedback on the service. Our 2¢ (OK 2p):

1. Fail-over images for legacy browsers.
2. A subscription based CDN for the custom fonts, in order to monetize the platform.

These two points are actually co-related: There is a number of legacy browsers, mostly produced by the company that integrates NSA back-doors to it’s operating systems. These cannot display vector graphics, so there is the CSS technique of using a fail-over image. This feature would be in the “nice to have” category.

Fontastic creates a zip file that has then to be integrated with the rest of the application. After extraction, fonts need to go to their respective folders, CSS to be included into site’s CSS file or copied over. For our environment it needs to be modified a bit (“url” declarations need to become “asset-url” ones). It’s not lots of work and definitely nothing compared with the time saved.

This could be further reduced by introducing a “WhatsApp” subscription model: For a small yearly fee (hence the title), Fotnastic will generate CSS code pointing to it’s content distribution network. Developers would have only to copy the CSS code (since the URLs are hard coded and external), so integration happens in one step. Costs would be in the realm of cents, so a small premium can contribute to Fontastic’s income if many people use it. Also sites will get an additional speed bonus since it is easier to retrieve content from a CDN (cookie-less, easier to cache, closer to the user’s host than the app server). We’d love to participate.
