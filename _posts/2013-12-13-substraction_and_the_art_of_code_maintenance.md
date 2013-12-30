---
layout: default
title: Substraction and the art of code maintenance
---

# Substraction and the art of code maintenance

Many times in the past I was unlucky enough to work on consultancy projects where the rule of thumb was “only add never remove”. Had to encounter huge code bases, sometimes in different programming styles that made the most simple thing take forever, since navigating within tens or even hundreds of files was necessary in order to comprehend where was what. Any effort to reduce the code-base was landing into the moronic responses “…what value will it add?”, “… clients have not paid for this …” and my favorite “There is time to do it now”, as it there will be any in the future. Additionally the senior software engineers (of sorts) happened not to be significantly competent gurus or good programmers, but merely people that had accepted the situation, “knew” the architecture having lived there forever, as well as the bugs of the external framework introduced five years before, that nobody updated to it’s next stable version. Their life was managing “Code Debt“, and hiding it from the client(s) in a way that would make the inventors of “Greek Statistics” proud that their methodology has been applied to an another disciple.

Needless to say, when the decision about managing Cypsel’s technical workings, the Zen mantra prevailed: “Always subtract what is not necessary”, soon life forced us to back up our initial decision or regress. Let’s check with a picture and then back it up with a thousand words:

![Code Frequency 3rd October 2013.](/images/code_frequency-03oct20131-545_302.png "Code Frequency 3rd October 2013.")

## Technical details – The story

At the end of August 2013, the 3rd version of the UI framework we use, bootstrap, was released: [http://blog.getbootstrap.com/2013/08/19/bootstrap-3-released/s](http://blog.getbootstrap.com/2013/08/19/bootstrap-3-released/s)

Cypsel, utilizing “responsive design”, needed many of the features that were in version 3. Because we did not had them available in the previous version, we had devised a number of hacks and custom workarounds. There was also a bit of duplication, since we pushed for an initial release, that somehow had to get fixed “soon”. The only missing piece of the puzzle was a a bootstrap3 version of “flatUI“, which we used on top. In less than a month, designmodo released it, free of charge, backing up their promise.

This explains the two red “valleys”, which count deletions on our diagram. Along with doing market research, talking to users, adding features, fixing bugs and all the startup “stuff”, silently all our user interfacing code-base was migrated to Bootstrap version 3. While doing so, custom hacks were removed, so the UI code, mostly HTML, started becoming thinner. This in turn made it easier to expose some CSS class duplication and refactor also the CSS declarations. Having a cleaner and smaller surface, made it easier to spot some bugs that had somehow manage to hide before. In parallel we had to add some features.

After this exercise was over and a couple of weeks later, the total project size is roughly the same (in the diagram the additions, colored green, take roughly the same space as the red colored deletions), after having added a number of additional features to the application.

## Added benefits

We hope that at some point in the near future, additional developers will be added to our team. The mental effort needed to engage and understand the project just got a small reduction. Only reading bootstrap’s and FlatUI’s documentation should be enough, without having to explain another “story” of customizations. Having a higher quality (or less chances to hide bad output) is an incentive to perform better. At the end of the day “Less is more” will prevail.