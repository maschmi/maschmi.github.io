---
layout: post
title: "Threat Modeling"
date: 2026-06-05 15:00:00 +0200
image: assets/threat-modeling/elem.png
description:
categories:
- security
- threat modeling
- methodology
tags:
- security
- threat modeling
- methodology
---
## Preface

As always, when I write about a topic, I classify myself as a beginner, I will tell why I write it anyway. 
However, if you want to dive right in, click [here](#start) and take everything that I write as an invitation to correct me or discuss with me.

I first came into contact with a hands-on Threat Modeling exercise during [Lisi Hocke's](https://www.linkedin.com/in/lisihocke/) workshop at SoCraTes 2024. 
She gave a packed workshop on "Secure Development Lifecycle Applied - How to Make Things a Bit More Secure than Yesterday Every Day". 
It was one of the most dense and well-structured workshops I ever visited, and it also had a part on Threat Modeling. 
This part peaked my interest in a way I started to read up, incorporating not only the question of "how will it break" but also "what are threats to it" into technical planning and refining stories with the team I was working in.

During the last year I never lost sight of the Threat Modeling methodology.
I also wanted to introduce it at work for features and systems. 
As it goes, things take time. 
At OSCO 2025 I then was able to attend a session by [Clemens Hübner](https://www.linkedin.com/in/clemens-huebner/) on Cumulus, a card game to threat model cloud environments. 
I ended up with a set of those cards and an order for Cornucopia (web edition and mobile edition) and also for Elevation of Privilege.
More on those card games later. 
Then came B-Sides Munich with a four-hour workshop on Threat Modeling by [Juliane Reimann](https://www.linkedin.com/in/juliane-reimann/) and [Moritz Krause](https://www.linkedin.com/in/moritz-krause-fcs/).

Finally, I felt ready to propose such a session at work. 
We did it with a new feature we were developing and were able to find some threats and hopefully mitigate them.
Even better, we could go back to the customer and ask them how risky they are seeing the one or other threat.
Making them aware and being abte to come to an informed decision.
I was quite happy. 
Even better, for other reasons, I somehow started a security focus group at the company.
We did some exercise sessions here as well.

After that, I've managed to prioritize to read Adam Shostack's book on Threat Modeling. 
It helped a lot in filling in gaps and foster understanding.

This is where I come from. 
But why write this article? 
At the SWEC 2026, I spontaneously packed some of the card games and thought it fun to give a session with those games. 
I did come up with an inconsistent data flow diagram.
I was surprised by the number of people expressed interest and then also showing up. 
In between sessions I tried to come up with some slides to give at least a short intro to the topic. 
I think it was good enough for that day.
But frankly, I really need to structure myself to be prepared for such situations.
Hence, this article.

## <a name="start"></a> What is Threat Modeling

> A threat model is a structured representation of all the information that affects the security of an application. In essence, it is a view of the application and its environment through the lens of security.
> 
> – [OWASP cummunity page](https://owasp.org/www-community/Threat_Modeling)

and of course, Threat Modeling is the process of getting a threat model.
It is a structured way of thinking about the things which can go wrong into a system.
Basically, there are four questions to be asked:

1. What are we working on?
2. What can go wrong?
3. What are we going to do about it?
4. Did we do a good enough job?

this is [Shostack's Four Question Frame for Threat Modeling](https://github.com/adamshostack/4QuestionFrame).

For me, those questions are not only applicable to security threats. What can go wrong also can be "What happens if this part of a system fails to function?", 
personally, I find it similar to the question "How will it break?". 
Not the same, but similar.

Why different? Because in Threat Modeling the threat takes the center stage. But how do we find threats?

## Finding Threats

There are different ways on how to tackle this.
It depends on what you focus, which documentation you have or are willing to create, on your scope, and also on your time.

### On what to focus

You can do it by focusing

* on assets
* on attackers
* on software

Focusing on assets I find somewhat a bit arbitrary and often hard to defend in my daily work. 
For me focusing on assets very fast includes physical security and similar things. 
Also, personally, I find it easier to look at processes or data flow to find threats.

Focusing on attackers is often phrased as "think like an attacker". 
Honestly, I can try. 
But when I learned one thing from the things I learned and trained about penetration testing, this is really hard to do. 
There are so many vectors and ways I do not know of.
Also, I would never be sure how determined an attacker could be and through which lengths they go.
Should I really think about threatening physical violence to get access to systems? 
Should I let my imagination run havoc?
Where do I stop, where do I begin?

Focusing on software is my favourite. 
Maybe not surprisingly as I am a software engineer. 
For me, looking at data flow diagrams, system diagrams, and similar just helps me to structure my thoughts and helps me see connections and parts which might be threatened.

### Useful documentation

Of course, if you want to go by asset, an asset inventory is very helpful. But what about when you focus on software?

For me things like

* deployment/system diagrams with data flow
* [data flow diagrams](https://en.wikipedia.org/wiki/Data-flow_diagram)
* state diagrams

help a lot. Data flow in general helps to understand who talks with what and where the output is used. 
State diagrams help in finding transitions where something can go wrong, e.g. a data set visibility changes when its state changes.

A thing I often forget, even though I know it helps a lot: Drawing in **trust boundaries**. Those are the places a lot of things can go wrong. 
In his book Adam Shostack describes a technique called "Look into seams" where the boundaries between systems get special attention. 
For me, trust boundaries are seams as well. 
Maybe not between systems but between zones of trust. 
For example, is the user input validated and sanitized? 
Does the next zone assume it is or checks it? 
Can it go wrong?

But what to do when there are no such diagrams?
Create them.
They do not have to be perfect from the beginning.
They might be coarse or even wrong at some places.
One benefit of a Threat Modeling session is validating and documenting assumptions about a system.
At first the diagrams function as a way to get the discussion going.
Refine and update them during the discussion.
Afterward you will have found threats and updated documentation.

On a personal note, I love having an excuse to draw up such diagrams and talk them over in a collaborative session.
Talking them over also usually helps me in getting the level of details right for a given scope – this holds true for all the diagrams I create.

### Example

Here you can see a data flow diagram which might be helpful in Threat Modeling.

![A data flow diagaram. In the upper left as User can upload a file, wich is then processes by File Processing. File Processing confirms the type and size and informs Business Logic. Business Logic the allows the upload. File Processing then uploads the file and retrieves a read access URL which it returns to the User together with a confirmation. The User can use this URL to download the file from File Storage. Also there are trust boundaries drawn in. File Processing and Business Logic live in the same trust zone, whereas User and File Storage live in their own ones. ](../assets/threat-modeling/dfd.png)

It includes trust-boundaries, forming trust zones.
The File Processing and the Business Logic trust each other. 
While the User and the File Storage only trust themselves.
Just by looking at the trust boundaries, I put threats to the interactions crossing them.
Do you already see some too?

### The scope

The scope of a Threat Modeling session can vary greatly. 
It depends on the time you want to invest, on the scale of the system you are modeling and also will change if the system already has some threat models.

But where to start if you do not have a threat model? 
Personally, I would start with some general things like authentication and authorization and then go feature by feature. 
I would try to rank the features by risk.
For example, is there a payment feature? 
Or a feature that will break the whole business process when it is attacked? 
Or a feature that holds PII?

By doing shared functionality first, I would scope them out when going feature by feature.
No need to threat model your authentication all the time when it uses the same shared functionality.
The threats will be the same. 
It might be different for authorization, depending on what you use and how different your features or system need to implement them.

In my opinion, the good thing is, when doing Threat Modeling regularly, you can limit your scope and have shorter sessions. 
Imagine doing it for each user story or feature during refinement and planning.
It might take you a couple of minutes, you can document the results and directly implement mitigations or talk to the customer. 
Shift left also helps with security topics.

### Who should be in the group?

Well, a security expert, practitioner, or security-aware person will help.
Also, persons knowing the system or assets to be modeled are important. 
Further, subject-matter experts, UX experts and requirement engineers and business analysts will certainly provide unique view-points and experiences.

My suggestion is to have a diverse group.
Especially the threat finding phase will benefit a lot from different viewpoints and schools of thought.
And who knows, maybe your UX expert will come up with good mitigations of some threats to password recovery? 
Making it easier for the user and making it more secure.
They may even help you to navigate the fine line between security and usability. 
Good security must be usable.
But I digress. 
Keep a diverse group and an open mind. 
It helps if the group is used to collaborative working.

### Methods

There are multiple methods to perform a Threat Modeling session. 
As with all methods, they depend on the setting and the group.

However, they all have one thing in common:

**Do not dismiss possible threats while finding threats!** 

You can do that when talking about risks and mitigation. 
There is a great reason to have a strong "All threats are valid" stance in that phase. 
As soon as you start to discuss if a threat is a threat to that specific system or not, you will hamper creativity.
People will then always try to validate their threats against their understanding of the system, including their assumptions and maybe not voice a real threat based on their understanding of the system.
Also, remember we are all human and some of us will shut down if their voices are invalidated.

#### Brainstorming

This is exactly what the name says. 
Set the scope, get up the diagram, and go. 
Everyone can either state a threat, write a post-it, or put up a digital note. 
You can go round-robin, you can set a timer.
Whatever works best with this group and in that setting.

It might help if the group already has some experience with Threat Modeling or some experience with thinking about threats

You can also try things like [scenario analysis](https://en.wikipedia.org/wiki/Scenario_planning), pre-mortem (assume failure) or whatever you can come up with to help the process.

#### STRIDE and variants

[STRIDE](https://en.wikipedia.org/wiki/STRIDE_model) stands for six thread types. 

* **S**poofing
* **T**ampering
* **R**epudiation
* **I**nformation Disclosure
* **D**enial of Service
* **E**levation of Privilege. 

Spoofing threatens authenticity, Tampering threatens integrity, Repudiation threatens non-repudiability, Information Disclosure threatens confidentiality, Denial of Service threatens availability, and Elevation of Privilege threatens authorization.

In his book Adam Shostack also writes about the DESIST acronym. 
This describes the same threats by other names. 
Personally, I find it a bit better to explain, but agree it has its issue with the same letter coming up twice.
DESIST stands for 

* **D**ispute
* **E**levation of Privilege
* **S**poofing, 
* **I**nformation Disclosure, 
* **S**ervice Denial, 
* **T**ampering.

In case you wonder, dispute replaces repudiation.

When it comes to privacy threats, Adam Shostack mentions [LINDDUN](https://linddun.org/linddun/whyuselinddun/) in his book.
LINDUNN stands for: 

* **L**inkability
* **I**dentifiability
* **N**on-repudiation
* **D**etectability
* **D**isclosure of information
* Content **U**nawarenes
* Policy and Consent **N**on-compliance.

Whatever acronym you like to use, they work the same. 
They should guide you in thinking about threats. 
As in "What happens if there is tampering?", not as in "Is this threat tampering?".
I had to learn that lesson. 
Because when I am tasked to find threats for each letter of the acronym, I automatically jump into classification. 
Better find more threats than classify them all correctly.
In the next phase, the group will talk about the threats anyway.

#### Card games

There are multiple card games out there to help you get started with Threat Modeling. 
The [Cornucopia series](https://cornucopia.owasp.org), [Cumulus](https://owasp.org/www-project-cumulus/) and [Elevation of privilege](https://shostack.org/games/elevation-of-privilege) are the one I know and have played.
Some of them you also can play online.

I really do like the cards, they help you to get started. 
However, what I never accomplished was to actually play the whole deck.
Usually the game dissolves after the first round into collaborative finding a place in the system you model where this threat could fit. 
Or into discussion how a specific threat will work. 
They really get the people talking. 
I guess one of the reasons is, no one has to make up that threat, they just have it on the card and can try to find a place where it fits.
Another way, people have told and have written, how they use these cards is to draw one card at each daily standup (or other regular meeting) and talk about the threat.
This can be very educational.

### Doing the Threat Modeling

Got your diagrams and your group ready?
There are still some small things the group can agree to. 
But first, I emphasize again how important it is to **not talk about mitigation and validity of a threat** during the threat finding state. 
Remember, the goal is to answer the question "What can go wrong?".

### Going by Element or by Interaction?

![A data flow diagram showing a User and a File Processing service with two arrows between them. The the arrow from User to File Processing service is captioned 'uploads file'. The arrow from File Processig to User is captiones 'confirms upload and returns read access URL'. User and File Processing are Elements, whereas the arrows are interactions.](../assets/threat-modeling/elem.png)

The first decision the group can make is, go by element or go by interaction.
When going by element, you might ask: What is a STRIDE threat to that particular service/feature.
When going by interaction, you might ask: What is a STRIDE threat to that request and also to that response?
Request and response are interactions.
Personally, I do not have a big preference here, but tend to go by interaction. 
Looking at the example above, I directly think of tampering with a file when I see 'uploads file' as interaction.
Whereas when I only think about 'File Processing' it can be everything and nothing.
Does it convert the file?
What checks are done?
However, this has the benefit of askin more general questions and not only focusing on the use case.
Who knows what the element is doing besides accepting a file and returning information?

### Going Wide or going Deep?

The other decision is: breadth first or depth first? 
Does the group want to find at least one threat per element/interaction? 
Then go breadth first. 
Does the group want to explore one element thoroughly? 
Then go depth first.
I think doing breadth first is better. 
I tend to discover more threats when I do not have to think about that one thing all the time and find some other threats to it.

There is yet another thing to keep in mind. 
Not every element or interaction must have threats for all the STRIDE categories. 
If there is no authorization needed, there might be no elevation of privilege threats. 
Also, spend not too much time on categorizing a threat. 
Writing it down is way more important than the category.

Got stuck?
There are some things that can help. 
The first thing is: Assume failure. 
Assume your system or mitigation is not working as intended.
Do not stop at the surface.
Do not think: Your WAF filters that out.
What if not?
You can also resort to [attack trees](https://en.wikipedia.org/wiki/Attack_tree) and attack libraries.
One of the well-known, and to me intimidating, attack libraries is the [MITRE ATT&CK](https://attack.mitre.org).
There you might find inspiration. 
Also, the above-mentioned card games might help to unblock the creativity.

### When to stop?

Will you ever find all the threats? Most likely not.
But somehow there needs to be a stopping point.

The easiest answer would be: When the time runs out, or when no one comes up with any new threats. 
However, it might be better to set a stopping point.
For example, there is at least one threat per element or interaction.
Or N threats are found.
Adam Shostack also mentions some kind of calculation in his book.
I do not remember it and will not look it up.
Read the book, it is worth it in its whole.

Personally, I find it better to keep the sessions short and focused and tend to "at least one threat per element/interaction".
It can always be repeated.
Especially if it is only a couple of hours.
You might be able to slip this in without (many) questions asked.

## Evaluating Threats

By now you hopefully have a list of threats ready to be evaluated.
But how to do this?
Do we need to address all those threats?
Yes, we should.

First, discuss the risk posed by the threats and what to do with it.
You could accept the risk.
Also, wait and see if anything happens can be valid.
You can stop building the feature to avoid the risk altogether, address the risk and mitigate it or transfer the risk.
You could also decide to ignore the risk, but then why go through the Threat Modeling at all?

Whatever you do.
Document it.

## Mitigate Threats

But how to mitigate a threat? 
There are multiple options.
Change the design if possible.
Or implement standard mitigations using the features of the platform/framework the software is built with.
Let the developers implement something specific and/or look for operational mitigations.
Or build something custom.
It really depends on the threat, the risk, and your system.
Also think about defense in depth.
[MITRE ATT&CK](https://attack.mitre.org) is not only a threat library but also has mitigations ready you can choose or see if you already use them, e.g., multi-factor-authentication.

If the group is not sure if a threat is mitigated or a real threat and makes an assumption, this assumption should be documented and later validated.

Which brings us to documentation and artifacts. 
Of course, you can create a fancy document for compliance reasons.
But you should at least create actionable tickets for things to be done. 
Those tickets can be:

* validate an assumption
* implement a test for mitigation if possible, at least think about how it can be tested
* implement mitigations – and then test them
* validate a threat

Better create a ticket and close it as "not relevant".

## Did we do a good job?

This is a tough question to answer. 
How to measure this?
Did you do a bad job if you have an incident and someone found a way around your mitigation?
Or if someone finds another vector to create an incident? 
Did you do a good job if there are no incidents?

Not so easy to tell.
But there are indicators:

* you have updated your documentation and understanding of the system
* you have actionable tickets
* you have documented assumptions to be verified
* you have ideas on how to test for the risks

If you can check most or even all of those points, you most likely did a good job.
Will it be enough to keep safe?
Most certainly not.
After all, systems and threats change.
It might be worth scheduling a repetition.
Maybe with a changed group?
Or a slightly changed scope?
Just to introduce a bit of variation and see if the outcome changes.

## When to threat model

I am inclined to say: All the time. 
The earlier, the better.
It is not a one-off activity to be done once.
Get into the habit of doing it for each feature.
It does not have to be very formal.
Build the habit of doing a threat model.

But how to start when you already have a bigger system in place?
I do not have a solid answer to that. 
Personally, I would start with the thing the team is doing right now and then branch out from there.
As written above, maybe create a threat model commonly used parts sooner than later to not have to address them all the time.
I do have the feeling it is more important to start and try than to do nothing.

## Why I like it

As I have written in the beginning.
For me Threat Modeling feels like a natural extension to the questions "how will it break" and "what if it breaks".
Two questions I find incredibly helpful in refinements and planning of stories.
Threat modeling sets the focus on security and at least helps me to voice risks clearer and also evaluate what to do about them.

## Resources

* Adam Shostack's [Threat Modeling Book](https://shostack.org/books/threat-modeling-book)
* [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org)
* [Adam Shostack's four question frame](https://github.com/adamshostack/4QuestionFrame)
* [OWASP - Threat Modeling Project](https://owasp.org/www-project-threat-modeling/)
* [A curated list of threat modeling resources](https://github.com/hysnsec/awesome-threat-modelling)