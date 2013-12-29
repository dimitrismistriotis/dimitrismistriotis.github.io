---
layout: default
title: The path to SJCP 310-065 exam.
permalink: /posts/the-path-to-sjcp-exam-310-065
---

# The path to sjcp exam 310-065

I intended to write this post some days later, but many colleagues of mine ask me for exam advice so here it is.

## Find out exam objectives

Check the examination objectives before starting to study. I studied lots on the concept of serialization, just to find out that they were removed from the exam :-(.

## Preparation - Studying

<h3>Books</h3>

<span style="font-size: 14px; line-height: 23px; font-family: Georgia, 'Bitstream Charter', serif; font-weight: normal;">Most people start with <a title="SCJP Sun Certified Programmer for Java 6 Study Guide (CX-310-065): Exam 310-065" href="http://www.amazon.co.uk/Certified-Programmer-Study-Guide-CX-310-065/dp/0071591060/" target="_blank">SCJP Sun Certified Programmer for Java 6 Study Guide (CX-310-065): Exam 310-065</a> of Katherine Sierra and Bert Bates (pictured below image from amazon.co.uk), which is most of the times enough concerning the concepts that have to be mastered. <em>(note: the book is titled "Sun certified...", while the exam is named "Oracle SJCP 310-065" exam after the acquisition of Sun by Orale)</em></span>

<img class="aligncenter" src="http://ecx.images-amazon.com/images/I/51%2BT%2Bi0tAtL._BO2,204,203,200_PIsitb-sticker-arrow-click,TopRight,35,-76_AA300_SH20_OU02_.jpg" alt="" width="300" height="300" />

The difference with other exams is that in this one the examined is asked to act as a "human compiler" this has two implications:

* The level of comprehension must be deep and
* There is (unfortunately) the need for exam-specific preparation

Therefore there is the need to write lots of code, in exam specific topics. This means many programs which at most will reach 30 lines of code. An example (from exercise 5.3 of the book above):

<pre lang="java">class Propagate {
  public static void main(String[] args) {
      try {
          System.out.println(reverse("1234"));
          System.out.println(reverse(""));
      } catch (Exception exp) {
          System.out.println("Caught it");
      } finally {
          System.out.println("Finally ");
      }
  }

  static String reverse(String input) throws Exception {
      if (input.length() == 0) {
          throw new Exception("Zero input size");
      }

      // using code provided
      String reverseStr = "";

      for (int i = input.length() - 1; i &gt;= 0; --i) {
          reverseStr += input.charAt(i);
      }

      return reverseStr;
  }

}</pre>

This exercise will train the brain to think accordingly.In similar fashion solve many (as many as possible) exam related questions, this is the reason that in case of failure, I would buy this: <a href="http://www.amazon.co.uk/Java-Programmer-Practice-Exams-310-065/dp/0072260882/" target="_blank">OCP Java SE 6 Programmer Practice Exams (Exam 310-065): Exam 310-055 (Certification Press)</a>.

<h3>On-line material</h3>

I was happy to discover some on-line resources such as those proposed by a good colleague, <a title="G. Ishak linked in profile" href="http://uk.linkedin.com/pub/george-ishak/14/302/967" target="_blank">George Ishak</a>:

* <a href="http://enigma.vm.bytemark.co.uk/webstart.html" target="_blank">Java.Inquisition</a> (update: broken link),
* <a href="http://www.exforsys.com/certification/scjp.html" target="_blank">SCJP Tutorials</a> (update: broken link).
* <a href="http://www.javabeat.net/products/cert/scjp-1-5.php" target="_blank">Java Beat</a> (note: might be a bit outdated).

## Exam Strategems

<h3>Train on how to answer questions</h3>

I had a great tutorial here from my exam coach, I do not want to reproduce material created by him. I will discuss an example, similar which can be found in the Sierra-Bates book mentioned before.

In the following code:

<pre lang="java">1 class Reference {
2    Reference r;
3 }
4
5 class GCQuestion {
6   public static void main(String[] args) {
7     Reference r1 = new Reference();
8     Reference r2 = new Reference();
9     Reference r3 = new Reference();
10    r1.r = r2;
11    r2.r = r1;
12    r3.r = null;
13    // Checkpoint 1
14    r1 = null;
15    r2 = null;
16    // Checkpoint 2
17   }
18 }</pre>

How many objects are eligible for garbage collection on line 16 (checkpoint 2)? Before you answer concentrate on <strong>how</strong> you reached to a number. Did you just read the code? Or did you do something like draw down a stack and a heap? And last but not least, what would be the case if line 15 was something like:

<pre lang="java">r2 = null; r3 = new Reference(); r3.r = r1;</pre>

The safest method is to draw a diagram like the following:<img class="alignleft" title="stack heap diagram" src="https://dl.dropboxusercontent.com/u/1995706/cdn/blog/javaGCexample.png" alt="" style="background-color: white" />

After that it is obvious: line 14 - red "X" - r1 points nowhere, line 15 - purple - same to r2. Therefore we have an "island" of isolation, it does not matter that one object references the other, two can be "garbage collected". Period. Next question. Try now the alternative scenario. Is it easier now?

<h3>Question time management</h3>

Generally speaking the exam is at about 3 hours which gives approximately 3 minutes per question. So train to answer exam-like questions in an average of 3 minutes. Some questions will be answered immediately, in about a minute that will give "extra" two minutes for a more tedious one.

<h3>Find a suitable exam centre</h3>

Having been to totally three exam centres up to now (scoring 3 out of 5 :-)), I can say that the choice of one matters, but not that much.

In London, I have tried two. The first one was located away from central London, looked like a family business. It might have the capability to offer only water and a tiny locker to squeeze my stuff, but the exam room was atomic and there were no distractions.

The second one was down-town London, very professional, allowed me to start 30 minutes earlier, but because of the location and the arrangement, the room was crammed. Also I got some times interrupted for split seconds, which broke concentration. Total there was a 5 minute penalty.

Again nothing of ultimate importance, but for some people it might matter.

## Final Notes

Keep in mind that the examination results are being printed immediately. You can request additional copies, all of them have to be stamped by the people exam centre stuff, usually the receptionist. You can use them until the Oracle's associates web page gets updated. You will receive an email from them along with the right to use Oracle's logo on your card is granted (in case you've passed). Don't forget them! A transcript looks like <a href="http://dl.dropbox.com/u/1995706/cv/supporting_documents/JavaSE6-ProgrammerCertifiedProfessionalExamScoreReport.pdf" target="_blank">th﻿is</a>.

The "official" printed degree arrives from post some weeks later.

## Acknowledgements

Thanks to: <a title="G. Ishak linked in profile" href="http://uk.linkedin.com/pub/george-ishak/14/302/967" target="_blank">George Ishak</a> (colleague) for answering questions and general helping with concepts difficult to grasp, already a SJCP, Brian Earl - Oracle University for corporate coaching.
