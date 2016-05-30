---
layout: post
title: How to learn Ruby - Tales in Rubyland part 1.
permalink: /posts/tales_in_rubyland_part1
---

Following the concept of learning a new programming language whenever I can as well as being curious about what changed in Ruby since I used it last time in 2003 as well as meeting <a href="http://twitter.com/#!/andypearson">Andy</a>, I decided to go for it.

## Preparation

Because <a href="http://www.codinghorror.com/blog/2005/04/you-can-write-fortran-in-any-language.html" target="_blank">you can write Fortran in any language</a>, I first wanted to begin with engaging with the language's mentality, it's design. Because Ruby is being compared and sometimes competing with Python, I started with checking the web front and try to see how rails is designed VS django. A friend supplied me with the following old, but still great video: <a href="http://video.google.com/videoplay?docid=2939556954580527226">Snakes and Rubies (full)</a>

<object id="VideoPlayback" style="width: 400px; height: 326px;" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" width="100" height="100" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="src" value="http://video.google.com/googleplayer.swf?docid=2939556954580527226&amp;hl=en&amp;fs=true" /><param name="allowfullscreen" value="true" /><embed id="VideoPlayback" style="width: 400px; height: 326px;" type="application/x-shockwave-flash" width="100" height="100" src="http://video.google.com/googleplayer.swf?docid=2939556954580527226&amp;hl=en&amp;fs=true" allowfullscreen="true"></embed></object>

Also this <a href="http://www.wikivs.com/wiki/Python_vs_Ruby">vswiki</a> entry somehow popped up.

## First Steps

Thinking with my "old" brain, my first inclination was to buy a book, which I did, but while waiting for the book to arrive on my doorstep, I started realising that the whole ecosystem provided new innovative ways of learning.

### Ruby Games

Say hi to <a href="railsforzombies.org" target="_blank">Rails for Zombies</a>. Rails for zombies is an interactive course where you learn how to program on Rails, by developing a twitter-like application for zombies :-). The game is arranged in levels and some additional goodies are available upon completion. It took about two days to finish it, by using only a modern browser. In order to support them the development team encourages the newcomer to buy a rails for zombies <a href="http://www.rubyrags.com/products/12" target="_blank">t-shirt</a>. Would definitely buy something like a mug or stationary.

### The book

In the mean time the book arrived: <a href="http://headfirstlabs.com/books/hfrails/" target="_blank">Head First Rails</a> by being a long-term fan of the series. Armed though with the knowledge of having finished the game, the exercises got finished fast. What was tough was to do them o Rails3 which is not there by default nor in ubuntu or in os-x.
In order to do so:
* Ubuntu: <a href="http://ascarter.net/2010/05/10/rails-development-on-ubuntu-10.04.html" target="_blank">Rails Development on Ubuntu 10.04</a>
* <a href="http://ascarter.net/2010/05/10/rails-development-on-ubuntu-10.04.html" target="_blank"></a>and OS-X: <a href="http://www.bawdo.com/posts/42" target="_blank">ruby 1.9 and rails 3 on mac osx 10.5</a>

Having to fix something broken apart from the initial frustration is an experience that exposes the internals of the system under repair. This time it got used as an introduction to gems, the Rails command line and other details and idiosyncrasies of the community and the languages approach.

## Going Deeper

### Koans

After the first experiences the opportunity to dwell more into programming in the "Ruby Way" came with exposure to the <a href="http://rubykoans.com/" target="_blank">Koans</a>, quoting:

> The Koans walk you along the path to enlightenment in order to learn Ruby. The goal is to learn the Ruby language, syntax, structure, and some common functions and libraries. We also teach you culture. Testing is not just something we pay lip service to, but something we live. It is essential in your quest to learn and do great things in the language.

Koans are essentially a list of tests that are designed to fail. The practitioner has to fill the gaps or write small chunks of code so that the tests will "pass". Evaluation is being achieved by a simple:
<pre lang="ruby"> rake</pre>

while at the same time the 263 tests are a tour de force of all language's features, conventions etc. At the end of the "challenge" you are being rewarded by a text adventure like screen (spoiler alert):

<p style="text-align: center;"><img class="aligncenter" title="Koans end screen" src="http://dl.dropbox.com/u/1995706/cdn/blog/koans_end.png" alt="" width="376" height="329" /></p>

The experience and the layout of the tests was great. I only found the last one being a little bit more difficult than the usual case, which made me <a href="www.johnlmiller.com/archives/2009/04/14/exploring-the-ruby-koans-building-an-object-proxy/" target="_blank">cheat a bit</a> in order to solve it. The most surprising fact is that whenever I did not know which method to use, the most reasonable english word, did the trick.

### Other Resources

Many and different and common. It seems that ruby programmers know them "by default". I spent much time watching the <a href="http://railscasts.com/" target="_blank">rail casts</a>, listening to the <a href="http://5by5.tv/rubyshow" target="_blank">Ruby show</a>. On the cheat sheet front, dzone's <a href="http://refcardz.dzone.com/refcardz/essential-ruby" target="_blank">reference card</a> was great as well as envy labs rails3 <a href="http://blog.envylabs.com/2010/12/rails-3-cheat-sheets/" target="_blank">cheat sheet</a>, which makes most of the cheat sheets I've printed to look oh-so poor.

## Conclusion

Great mentality, great language (I knew it would happen when I decided to write my dissertation on Ruby back in 2003, but would never expect it to be so big... never). Also great community. People are eager to accomodate the newcomer and teach the various ruby-isms which make things so great but overwhelmingly different.

Also having many issues solved because of the languages design and frameworks on top of it, code maintainers are investing huge amounts of time on code readability and maintainability.

Finally although Ruby is far more than that, most attention is on it's web framework, Rails. So it is natural for most people to be a "web" language or a "php-killer". I do not know if this limits languages reachability or if it is irrelevant.

## Further steps

I suppose I will use it for a mini personal project that I can implement in any language that I want. Also I will do a Rails project whenever I have the chance. In part 2 the journey continues with Ruby's scripting capabilities: <a title="Tales in Rubyland part 2 – “You can write elegant shell scripts and unit test them too”" href="/posts/tales_in_rubyland_part2">Tales in Rubyland part 2</a>.
