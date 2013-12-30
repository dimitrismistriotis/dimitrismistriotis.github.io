---
layout: default
title: Substraction and the art of code maintenance
---

# A “sivering” moment – Whatever scares you…

This is a tribute to derek sivers

Preface: I used to be a fan of Derek Siver’s blog, [sivers.org/blog](http://sivers.org/blog)

where two of his motos clearly registered to me:

I have some easy rule-of-thumbs to follow [sivers.org/scares-excites-do-it](http://sivers.org/scares-excites-do-it):

> 1. whatever excites you, go do it
2. whatever scares you, go do it
3. every time you’re making a choice, one choice is the safe/comfortable choice – and one choice is the risky/uncomfortable choice. the risky/uncomfortable choice is the one that will teach you the most and make you grow the most, so that’s the one you should choose.

Another one that really got into me was the advice towards him on learning how to sing:

> ... When I was 14 years old, taking guitar lessons from Tom Pecora, he gave me that this-is-important-so-listen-well look, and told me something that stuck with me for life:
“You need to learn to sing. Because if you don’t, you’re always going to be at the mercy of some a$$h0le singer.” ...

The case

Both of the above came into use when we had to redesign large parts of Cypsel before our initial release. We had decided to drop usernames as a way to address users and use people’s first names instead. This had the consequence that a user registering had to supply his/her first name as part of the registration process.

After a long refactoring session everything was OK with one exception, the registration form looked like this:

![Sign in, envelopes only](/images/sign_in_before-300_219.png "Sign in, envelopes only")

At that point we would prefer instead of having an envelope decorating the name input boxes, to use a small person “avatar” instead. Conveniently at that point our (nothing to do with being an XXX-hole) singer designer happened to be on holiday and was generally reluctant to provide the time needed to provide it. Unfortunately for us, the “envelopes” provided were having custom spacing from their edges, which meant that I could not “just” download/buy an icon and place it there.

So one day I woke up with a feeling similar to those old cartoons where the main character has a small devil/angel figure speaking and saying constantly: “draw, draw, draw”. Then decided that I would not stop until I had my little icon done.

First I needed an editor for SVG files (which is the format of the icons used). I knew that the original ones were exports from a tool like photoshop or illustrator, but SVGs are XMLs, which means for our case “formatted text”. I had read some tutorials on their structure on the [Mozilla Developer Network](https://developer.mozilla.org/en-US/), but an actual editor was necessary, something in the lines of… Microsoft paint for SVG.

Rescued by google, not only such an editor exists, but it is also easy to use and web-based. [Svn-edit](https://code.google.com/p/svg-edit/), “A complete vector graphics editor in the browser (in JavaScript)”,  can be used straight from this link: [http://svg-edit.googlecode.com/svn/branches/2.6/editor/svg-editor.html](http://svg-edit.googlecode.com/svn/branches/2.6/editor/svg-editor.html).

Additionally it allows to upload an existing image, which just rocks. So now the “problem” was deduced into imitating existing work and not drawing something from scratch, “All drawing is re-drawing”. So first step is to upload the original envelope image:

![Original envelope edit screenshot.](/images/svg_edit_screenshot_envelope_only.png "Original envelope edit screenshot.")

After some time spend and remembering things from other editors, such as GIMP or Seashore, I started experimenting with the “path tool”:

![svn_editor_path_tool](/images/svn_editor_path_tool.png "svn_editor_path_tool")

The aim was to produce an arc that would fit within the original and also be at about 50% of it’s height. After an embarrassing amount of time and with an extra circle on top, the result was something like the following:

![Envelope and Avatar](/images/svg_edit_envelope_and_avatar.png "Envelope and Avatar")

So although I was looking one of the most ugly drawings, the difficult part was over: After entering the “scared” and “singing” zone, there was “only” the colouring left as well as remove the original. After 15 minutes of  trying and at the same time going nowhere, there was an “A-ha”/eureka moment: Since SVGs are texts, maybe I should continue in a text editor. This was crucial.

To the text editor

Once in the “eyes” of pure text, the last obstacle became obvious: The vector lines are inside the envelope are inside an element that declares the opacity to 0.5, so had to reposition the additional elements inside it. After that it was easy to do the rest, like copy and paste the color values or remove the elements from the folder. A before and after screenshot in the text editor is the following (initial on the left, final on the right):

![svg_edit_screnshot_side_by_side](/images/svg_edit_screnshot_side_by_side-300_137.png "svg_edit_screnshot_side_by_side")

The new vector file had just to be integrated to the application, with the result as it shows:

![sign_in_after](/images/sign_in_after.png "sign_in_after")

Check (and sign up) here: [http://www.cypsel.com/users/sign_up?customer=1](http://www.cypsel.com/users/sign_up?customer=1)

At the end the whole exercise fell like an achievement, first endeavour in the zone “of what scares you” while at the same time “singing for my own band” – kind of. And this is also one of the nice things of working for a startup: at some point everyone has to get out of their comfort zone and expand their capabilities.
