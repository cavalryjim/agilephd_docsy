---
title: Technology Stack
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

\label{cha:technology_stack}

A technology stack is the combination of all the products and programming languages used to create
an application. For many applications, the 'stack' used in its development might include:

* Database management system (DBMS)
* Language(s) for backend logic
* Language(s) for frontend logic or presentation
* Web server
* Operating system

## Full Stack Development

You may have also heard the term 'full-stack developer'.  A full-stack developer is knowledgable in all aspects of software development from concept to a finished product.  The key here is 'knowledgable'. It is acceptable that you might not know the answer to all technical questions but you should be able to find answers in a timely manner.

The opposite of a full-stack developer would be someone with a very specialized and limited skillset.  A database administrator (DBA) will have a very deep understanding of his/her particular database management system (DBMS) along with the concept of table spaces, triggers, SQL, etc. but probably has a minimal understanding of UI/UX, responsive frontends, or javascript.

Over the years, the technology stack a developer was required to know has gotten more complex.
![Stacks change.](images/stacks-change.jpg)

Other important skills for a full-stack developer to know:

* Server, network, and hosting environment
* Relational and non-relational databases
* How to interact with APIs and the external world
* User interface and user experience
* Quality assurance
* Security concerns throughout the program
* Understanding customer and business needs
* Version control


## Demystifying Stacks

Geek speak, or technical jargon, is the terms, phrases, and expressions that the members of the technology community use in their, or if you identify as a techie, 'our', communication.

To the outsider or someone new to community, this jargon can be confusing, intimidating, and possibly a barrier.  I've found comparing technology stacks to a bento box helps to remove some of the mystery of our field.  


\begin{aside}
\label{aside:Bento}
\heading{Bento}
![Bento Box](images/bento_box.png)

\noindent Bento is a popular box meal common in Japanese cuisine. It brings together rice or noodles, fish or meat, with pickled and cooked vegetables, in a yummy box.

You need a balanced mix of things. It’s a puzzle - putting everything together in the box.
“Ekiben“ - content which is arranged in the most efficient, graceful manner. The bento is presented in a simple, beautiful, balanced way. Nothing lacking. Nothing superfluous. Not decorated, but wonderfully designed.

\end{aside}

Likewise, we can think of the technology stack as a bento consisting of our protein, starch, veggies, and lagniappe[^lagniappe_footnote].  Our technology bento will be used to classify the
technology as storage, infrastructure, logic, and style/structure.

![Bento Worksheet](images/bento_worksheet.png)

## What I use for development

The Golf Channel has a segment called "What's in the bag?" where they analyze the clubs used by a particular professional golfer.  During the course of this book, I'm going to give you my "What's in the bag?" for developing a software product.  Like pro golfers, my selections change as the technology changes. Here is what I'm currently using for development...to include writing this book!

### My Machine
Most developers I know working at startups or active in the open source community use Mac computers [^usage_footnote]. Those not using a Mac tend to select some flavor of Linux (with Ubuntu being a personal favorite). I was a long term ThinkPad owner (dual booting between Windows & Ubuntu) but made the switch to a MacBook in 2013 and haven't looked back.  When working in software development, you will eventually want to create a mobile app.  The only way to create and deploy an app for the iOS is by using a Mac.  With my MacBook Pro, I can do web development, android development, and ios development.

### Text Editor
Yes, a text editor is exactly what it sounds like.  It edits text.  In coding, the term 'editor' has a slightly different meaning.  Used in software development, it is an application used for editing your code files.  Most web development languages are interpreted, not compiled, so there isn't a need for a robust integrated development environment (IDE).  For the past 5 years, I have been using TextMate as my coding editor.  

You might be wondering why would you need a special code editor, rather than using something like Word or Notepad.

The main reason is that code needs to be plain text, and the problem with programs like Word is that they don't actually produce plain text, they produce rich text (with fonts and formatting) along with other meta data created as part of the file.

Another reason is that code editors are specialized for editing code, so they can provide helpful features like highlighting code with color according to its meaning, or automatically closing tags for you.

I've found most developers become very passionate about their particular editor. Soon, you'll come to think of your trusty code editor as one of your favorite tools.

![TextMate](images/textmate.png)

#### VS Code (recommended)
[Visual Studio Code](https://code.visualstudio.com/) is a free editor from Microsoft.  It is
the most popular code editor in the software development community and my current editor of choice.  Occasionally, I find VS Code to be  little too heavy for my needs.  If you are not already attached to an editor, I would recommend VS Code.

Here are some survey results from a 2021 survey of 82,227 software developers on from [StackOverflow](https://insights.stackoverflow.com/survey/2021#most-popular-technologies-new-collab-tools)

![CodeEditorPopularity](images/text_editor_2021.png)

#### Other Code Editors
Over the years, I have used other code editors such as [Atom](https://atom.io/), VIM, and TextMate.  When using the previous code ediors, I probably recommened them and could tell you why I like them.  If you do this stuff long enough, you will likely use several different code editors throughout your career.  My advice is to find something that is free, popular amonst your colleagues, and available for Windows, MacOS, and Linux.  

#### Eclipse IDE
[Eclipse](http://www.eclipse.org/home/index.php) is an integrated development environment (IDE) which is much more than a code editor.  IDEs are typically associated with compiled languages (such as Java) but can still be used with other languages.  I have used Eclipse in the past but it is very heavy in terms of application size.

\begin{aside}
\label{aside:polytex_markdown}
\heading{Who cares what I think?}

\noindent Who is this guy and why should you care what I think about development?  If you are one of my students, you are a captive audience and your grade is an incentive to care.  Otherwise, here is some more information about me.  I haven't always been an academic and these are the cliff notes.  I've been writing code since the mid-1980s.  As a kid, my parents bought me a Commodore Vic-20 and I would write games for me and my friends.  In college, I majored in math while taking programming classes in Fortran, Pascal, COBOL, and C.  There was a 4ish year hiatus from coding because I used the army to help pay for college (whole different story for a completely different book) but quickly returned to software development after my time in the military. While working on an MS in Computer Science, I paid the bills by developing software for the healthcare industry.  Not knowing when to quit, I completed the MS and picked up a PhD program in Information Systems.  

Somewhere in graduate school things got a bit more interesting.  I started working with some fellow researchers on social networking algorithms and knowledge networks.  Long story made short, we found success in a startup venture developing an application to track the knowledge networks of organizations.  We had a good exit, I did some more army stuff, worked as an enterprise architect for a bit, and then returned to academics while still partaking in the occasional side venture.

[LinkedIn](https://www.linkedin.com/in/jdavisphd/)
\end{aside}

[^lagniappe_footnote]: The word comes from the Louisiana French referring to the extra items.
[^usage_footnote]: This is completely my observation with no quantitative support.
