---
layout: default
title: The path to SJCP 310-065 exam - Part 2.
permalink: /posts/the-path-to-sjcp-exam-310-065-part2
---

# The path to sjcp exam 310-065 - Part 2

It would be benefitial to examine a little more thoroughly the methodology of finding an answer to a question "from scratch" and turn that into an experience that will provide the appropriate knowledge to pass the exam. 

Apart from reading and understanding and adopting a theoretical approach, the size of the items covered demands writing a big number of small programs so that the compiler\'s behaviour will get "wired" to your brain. Allow an extract from the SJCP Certificate book mentioned in the <a href="/posts/the-path-to-sjcp-exam-310-065" title="Part 1">previous post</a>:

<img alt="" src="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/sjcp_book_extract-write_small_programs.png" title="book extract" class="aligncenter" width="400"/>

or alternatively the modern stack overflow question: <a href="http://stackoverflow.com/questions/3021998/resources-and-techniques-methods-for-scjp-preparation" title="Resources and techniques/methods for SCJP preparation?" target="_blank">Resources and techniques/methods for SCJP preparation?</a>.

Intentionally chosen, is the subject of <a href="http://en.wikipedia.org/wiki/Label_(computer_science)" title="labels" target="_blank">labels</a> in java (<a href="http://www.janeg.ca/scjp/flow/labels.html">http://www.janeg.ca/scjp/flow/labels.html</a>) will be discussed. It is important before that to consider the "why"s of this.

## Why Labels

There is a number of questions in the SJCP examination concerning labels. Before that, there is some QnA about the topic:

Q. Why study labels?

A. Because they are part of the exam.

Q. But why really? Is there any other more arcane/mystic/deeper reason?

A. It does not matter.

Q. But please, is there any answer?

A. OK, for a number of different reasons, such as: labels are used primarily in the implementation of <a href="http://en.wikipedia.org/wiki/Finite_state_machine" title="wikipedia article">finite state machines</a>. They can also be found in some old implementations of algorithms that got inherited in java from other implementations in other languages, such as C, or just to maintain consistency with the mathematical paper that describes the algorithm. Additionally they give a better understanding of how things work in assembly level.
All of the above does not mean that people usually encounter labels in their real life while working with the language (most people don\'t know their existance, specially in Java). This fact may rationally lead to the following question...

Q. So since their use is limited, I can skip them or ignore them?

A. Not at all. There is no quota or percentage ratio on the questions areas that pop up in the exams, hence this post.

## Now let's answer one question...

Which are the values stored in variables <b>i</b> and <b>j</b> after running the following piece of code:

<pre lang="java">outer:
  for (i=0; i &lt; 10; i++) {
    for (j=10; j > 0; j--) {
       if (j==5) {
         break outer;
       }
    }
  }</pre>

Additional question, answer the same for the following piece of code:

<pre lang="java">outer:
  for (i=0; i &lt; 10; i++) {
    for (j=10; j > 0; j--) {
       if (j==5) {
         continue outer;
       }
    }
  }</pre>

Before getting down to the details, it is assumed that both variables are "something" (possibly a primitive, such as an int), that can store values between 0 and 10. On top of this if there ever was some code before or some code after those lines,as well as for the whole application around it, is valid and also can compile and that shall be the law(Too much time is spend on this, for a good reason). Which means that when we see "extracts" of code such as this, we don\'t look for syntax errors or so called "gotchas".

This also means that now, that we will write a small application which will have a 
<pre lang="java">public static void you_know_what(...)</pre>
the i will be initialised in this way:
<pre lang="java">int i=0;</pre>

before we print it, and not a:

<pre lang="java">int i;</pre>

which throws a compilation error, something you definetely know, else you would not be reading this blog post. (If you are, you know that you will first run one or two mini-programs and come back).

One way or another our java apprentice will end up with two programs which will look somehow like this:

<pre lang="java">class LoopCase1 {
  public static void main(String[] args) {
    int i=0;
    int j=0;
    outer:
    for (i=0; i &lt; 10; i++) {
      for (j=10; j > 0; j--) {
        if (j==5) {
        break outer;
        }
      }
    }
    System.out.println("i = " + i + " j = " + j);
  }
}
</pre>

Where the call to System.out.println(...) goes <u>immediately</u> after the code under examination. A mini-app with the source code above, can be downloaded here: <a href="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/code_and_samples/LoopCase1.java">LoopCase1.java</a>.


<pre lang="java">class LoopCase2 {
  public static void main(String[] args) {
    int i=0;
    int j=0;
    outer:
    for (i=0; i &lt; 10; i++) {
      for (j=10; j > 0; j--) {
        if (j==5) {
          continue outer;
        }
      }
    }
    System.out.println("i = " + i + " j = " + j);
  }
}</pre>

Which can be found here: <a href="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/code_and_samples/LoopCase2.java">LoopCase2.java</a>.

Now we are ready to compile and run.

First - "break" case:
<img alt="" src="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/screenshot-LoopCase1.jpg" title="LoopCase1" class="aligncenter" width="725" height="213" />

Second - "continue" case

<img alt="" src="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/screenshot-LoopCase2.jpg" title="LoopCase2" class="aligncenter" width="725" height="213" />

## Summary

<p>Apparently this seems to be the depth and the approaching mindset needed for the exam. It is a matter of "switching" from usual forms and ways of learning as well as practice.</p>
<p>Good Luck.</p>
