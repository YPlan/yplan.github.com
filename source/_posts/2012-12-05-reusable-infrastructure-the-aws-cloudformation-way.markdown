---
layout: post
title: "Configure your cloud the AWS CloudFormation way"
date: 2012-12-05 18:43
comments: true
categories: [Infrastructure, Cloud]
author: Julius Seporaitis
---

Hello everyone, let me introduce myself and tell you what I am doing at
YPlan. My name is Julius, I am Software Engineer, who is not afraid to tinker with AWS
from time to time. At YPlan I am responsible for our backend systems,
parts of developer tool chain as well as cloud infrastructure.

In this post I would like to talk a little about how we came up with
using [AWS CloudFormation](http://aws.amazon.com/cloudformation/) to
manage our infrastructure and deployments and did not jump into more
conventional tools like [Puppet](http://www.puppetlabs.com/),
[Chef](http://www.opscode.com/), [CFEngine](http://cfengine.com/) or
services like [RightScale](http://www.rightscale.com/) or
[Scalr](http://www.scalr.com/).

<!-- more -->

It all began mid-summer when the team was in place and started rocking
some code. After a couple of weeks we got bits of our server side
working and the time came to think about our infrastructure. We wanted
to bootstrap it _quickly_, have it working and deploy the code without
bothering too much about it. We came up with what I would call a
straightforward cloud architecture, found in all "Web application best
practices" books, and the search for the right tools to manage
infrastructure began.

My previous experience involved some Puppet-eering and I was
considering using it, but there were some but's. Puppet is a very
powerful configuration management tool and while I had some experience
with it, I wouldn't call myself a guru enough to completely automate
configuration management. Also, whoever tried to implement centralized
management of agents using Puppet master, should agree that it is
difficult to do it right. And we wanted to manage our infrastructure
from one authority and bootstrap it quickly. So I was very hesitant to
choose Puppet for the task. Therefore the search continued.

I turned down Chef too, with which I played for my own fun &
profit some long time ago and CFEngine of which I heard that it might
be the best of its kind tool out there (AFAIK Facebook uses it), but
very complex. My assumption was that they would have the same problem
that Puppet had - hard to do it right. On top of that, I would have to
learn things completely from scratch. That did not rhyme with
"quickly".

With conventional tools turned down I had to give a look into what
services are out there. And there are few, one of them is
RightScale. I researched a little about it and there were some
indications that it might actually be the right tool. Documentation
and whitepapers said it has a Puppet integration. I speak some
Puppet. If RightScale could take care of managing the master and the
agents I could probably orchestrate everything in a blink of an eye. I
signed up.

And quite frankly I was a little bit disappointed. Not because
RightScale would be bad, but as in the Puppet case - it was powerful
tool with, what seemed to my first time user eyes, a bit too complex
UI and too much features for the start. I clicked around, read the
docs, I tried a tutorial, but the results were not very inspiring in
terms of what has been done in couple of hours. I did not want to read
a lot of docs just to understand how the thing is working or what is
where. It would just take too much time.

Later I checked out Scalr too, but turned it down for the same reasons -
good, but too much to boot myself quickly.

I must admit, that at this point I felt a little bit desperate - I had
turned down some of the most known tools and next in list was AWS
CloudFormation of which I never heard of and not much hype or praising
was happening around it. Just a service homepage on AWS website with a
description and user guide. Not very inspiring at first sight, but I
had to give it a look. And so I dived into documentation.

First glance - it seems odd to define configuration using JSON, but
they had a reasonable explanation for that, so I
continued. Documentation was written in a very clear and concise way:
"X means this, Y means that" - pretty straightforward to grasp what
will happen. Reading about _cloud init_ scripts I noticed that they
have clear concepts that I have already seen in Puppet scripts -
packages, services, commands, files - and they work in much of the
same way.

After a while, I was convinced that CloudFormation was just the right
tool:

- it looked like a (kind of) subset of a Puppet manifest, although
more strict - yay, I speak Puppet;
- it takes care of materializing whatever I write - no need to bother
with that - helps;
- most important, the documentation explains how to make things happen
not how to comprehend the tool itself;
- as for JSON format - it turned out quite useful when comparing
  configuration changes;

The equation: something I know _minus_ bothering with orchestrating
things from one place _plus_ documentation that guides you to get
things done. Bingo! Our cloud infrastructure was up and running in
less than a day and we did not bother with it until we needed new
packages installed or performance tweaks and that did not happen a lot
until recently.

To this day I do not quite understand why there is little hype around
CloudFormation, although I heard with one ear that there are companies
that use it. It is a great small AWS service that just lets you get
things done quickly. And since mid-November it supports what is called
"custom resources", so it is possible to define almost every bit of
infrastructure using CloudFormation JSON scripts - 3rd party resources
and your own services. Not to mention the integration with all other
AWS services and an extension point via SNS and SQS that was always
there. Oh, and you pay only for the infrastructure you provision,
CloudFormation itself is free!

To the defense of tools and services that I turned down, I must say
that they are great tools and they serve their purpose well. However,
at the time the goal was to have something running in less than a day
and don't bother with it anymore until there is a need. And those
tools were just too powerful and complex to grasp for the quickness of
the result. CloudFormation has it's own drawbacks too, so we are aware
that at some point in the future we might need to switch to something
more powerful.

But I think and hope this story can be an example of how it is
possible to move quickly, fail by actually testing things, polish the
requirements and find answers in unexpected places.

Please let me know what you think and feel free to share your ideas in the
comments section below.
