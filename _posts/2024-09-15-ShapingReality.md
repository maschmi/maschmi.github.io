---
layout: post
title: "Shaping Reality"
date: 2024-09-15 10:00:49 +0200
image: assets/images/computerno.webp
categories:
  - impact
  - sociotechnical
tags:
  - impact
  - sociotechnical  
---

If you are working in software development, chances are high you are working on something user-facing. And if you do this, your decisions may shape reality.
But how is this? Let me give you three examples.

## Gender

If your system needs to ask for gender categories, your requirement may state only to ask for female and male. Maybe you are required by law to ask for a third gender. 
Even if you are not required to ask for a third, you should. But maybe you can keep the option open to select "none"? 
As part of a development team, you are in a position to point out to the business (I will use business and client mostly interchangeably), that providing more gender options or to provide a "none" option is technically rather cheap.
After all, you already must act on different conditions. And if not, you may ask the business **why** they need this information. Chances are high, they only want this data
for statistics, marketing campaigns or personalized communication. Yes, personalized communication may mean to add more conditions to email templates or other user facing content. But you already need to do this anyway, 
and maybe there is a way to write the texts in a form where there is no big difference between the genders.

But business may need the gender for a very specific reasons. Let us say they need the gender to put a person into a sports team. And that sport may only know "male", "female" and "mixed" teams. But isn't 
this another use case? This may warrant for an extra data field. You can point that out to.

Further, think about first and last names. Your service may need the legal ones, the ones written in your ID. But maybe the ID is not up to date as someone is in the process of changing names.
Here you can add a "call sign" field and use this instead of the first name if it is filled and applicable. This should also be rather easy to model from the beginning.

Generally, as long as you do not have to act on information, e.g., use a gender-specific form in a text asking for additional data to model a more diverse reality is easy. If you have to act
on this information, it may not be that cheap, but even more important.

Thank you, [Julian Traut](https://www.linkedin.com/in/julian-traut-13b91a211/) for keeping an eye out for such issues. And also for keeping me aware of these problems. Not only, but also because of this, I enjoy working together with you.

## Couple or Family?

Next up, an example which annoys me personally a bit. When my partner and I are going on vacation, we are mostly tracked as family.
While being in a long-term relationship, we neither share the same family name, nor are we, in fact, a family. Every time we book
a journey or hotel stay, the one responsible for the communication will lend the last name used by both of us. This sometimes
gets confusing when signing a bill or an invoice.
While this is a minor inconvenience, it annoys me. Especially if we must state our full names on all forms and then are treated
as a family with one family name.

But let us look deeper into this. While fixing this on the software side looks simple enough; it may pose problems to the
whole sociotechnical system our software is interconnected to. The service we are responsible for will most likely interact with other services. Let us say we are working
on a booking service which anyway needs full information on both persons. What if the hotel management software is not capable of doing this?
And even if it is, what if the templates for printing table reservation cards cannot print two (full) names? And how should the staff address these
persons? You now may argue, why bother then; we as part of the software developer team cannot change this. And I concur, we cannot change this alone.
But we can raise awareness to this sort of problem. Maybe we are the first people raising this question. Maybe business decides to offensively 
market this as a feature - say in a hotel management system. Or at least put it into documentation. 
Maybe business thought of this and considered it too expensive and did not even ask for such a feature — it would be our responsibility to give them a better cost estimate then. 
Maybe they outright deny there is an issue. But this should not hinder us posing such questions.

## Automated Decisions Making Systems

Thanks to [Daniela Stangl](https://www.linkedin.com/in/daniela-stangl/),
whose bachelor thesis on automated decision-making systems (ADMS) in social services, I had the opportunity to read and also discuss some parts with her, I now can put a somehow vague scary feeling into words.
Daniela wrote in "Algorithmische Entscheidungssysteme in der Sozialen Arbeit unter kritischer Perspektive" about ADMS in social work analyzing it under aspects of [critical theory](https://en.wikipedia.org/wiki/Critical_theory). Besides the analysis, and for me, an introduction to critical theory, the thesis
had some examples.

I will paraphrase one of these examples:

> Child care services are always overworked. It would be great if ADMS could help them to reduce workload.
> If they need to remove a child from a family, a lot of metrics and reports are evaluated. Also experience
> comes into play. Wouldn't it be great to use those metrics, feed it into a model and get a score by which 
> the decision should be made? Of course, a human will need to make the final decision, but just envision the
> objectivity and efficiency!

I will not go into details of data model biases and such. These are well known, and often enough ignored problems. And also 
not completely responsible for the scary feeling I have. Further, this is also something we may not be able to influence directly or
successfully as a software developer. We may raise the issue, but most likely this warning will be ignored. But what can we do?

Well, imagine we are working on integrating such an ADMS into software for child care services. How will we present the data? 
How can we influence the end-user-interface and therefore the end-user? Depending on the project setup we may be rather unrestricted or hopefully, at least
still can voice ideas and will be heard. Here are some questions we could ask:

* Should the service only display the recommendation without further details? After all, it is considered an objective value.
* Should the score be calculated automatically and presented as a single number?
* Should the score only be calculated on request? The user will then not be influenced when looking into the case the first time.
* If the score is calculated on request and the result is basically available instantaneously, should we delay it to give the user time to think about the score they expect based on their experience and the case?
* Should there be some explanation visible next to the score?
* Should the score only be revealed when the user wants to see it and stay hidden by default? Again, the user will not be unconsciously influenced in their decision.

These are all questions that should be asked by the team. Even if the requirement states, e.g., display the recommendation immediately.
Personally, I would opt for a solution of only displaying it on request and the first time delay the calculation. This comes a bit from personal
experience with other software and processes. If a system presents me with a single number which seems to sum up all the other metrics, and I am in a hurry or stressed, my eyes will be
jumping to this number and my bias is set. To prevent this, I like to have to go through extra motions. It slows me down and lets my brain catch up with what I am doing, even if I am not stressed or in a hurry, this helps.

## What shall we do?

Enough of examples. You may already wonder why a software developing team or a single developer should take questions
like this into consideration. You may even say "I'm only the code monkey, implementing defined features. I have enough 
technical details to worry about." I know this feeling. And yet I disagree. I do not ask you to pick up the fight. I do, however,
ask you to voice your concerns and share your insights. Maybe they are heard. Maybe not. Personally, I sleep more sound if I voiced them. And yet, often enough
I do not have the energy to do so. Or I am in a hurry and forget it. Or the company/team culture does not fit (this then becomes a complete other topic).

I also see a problem in fixed-price and fixed-feature set projects. The client has already made up their mind. Contracts are agreed upon and the team
may be more or less hampered by time and budget constraints. Then there is no time to ask ourselves how the software or the model may impact reality. Maybe there is not
even the chance to talk to the business anymore, even about simple ideas how to shape a better reality.

Maybe this is a reason I find "agile" development and DDD good ideas. It is not only about value, but also about impact and shaping reality. 

## Conclusion

To sum it all up. I see software shaping reality. I see software not only as a technical thing but a sociotechnical one. You cannot
solve a social problem by throwing technology on it. Technology may help or may harm. And this is the reason I see every person
involved with software development in the responsibility to shape reality. But remember, this is a team sport. We need diverse teams, we need all
the experience we can get to shape a friendly and inclusive reality. We need to stay in permanent contact with the business domain. We need people
who excel in the technical part. We may also need code-monkeys. Most likely, the needs will shift during project progress or product lifetime.

Personally, I do enjoy and sometimes get lost implementing features and closing tasks. This feels like measurable productivity. But I also enjoy
shaping reality and try to make it more friendly, and maybe I find this even more rewarding. How about you?


## A note of thanks

This train of thought took its time to arrive at a station. I think it started well before SoCraTes 2024, but resurfaced when [Juke Trabold](https://www.linkedin.com/in/juke/)
posed a question on widening the impact of SoCraTes during the world café. And it was put right back on track, when I reflected the conference 
talking to [Lisi Hocke](https://www.linkedin.com/in/lisihocke/) on the way back - fittingly inside a train. Thank you both for feeding the nagging thought. And of course, 
thank you, [Julian Traut](https://www.linkedin.com/in/julian-traut-13b91a211/) and [Daniela Stangl](https://www.linkedin.com/in/daniela-stangl/) for unknowingly helping me
find examples.

DALLE created the image on top of the article.

### Disclaimer

None of the persons linked in this article had the chance to read it before. They were not even aware it would be written. These are all
my very own opinions. To the persons linked in here: If you want to be unlinked/unmentioned, please contact me. Same if I shall abstain
 from linking without asking. To people having issues with this opinion, I am looking forward to hearing from you. 