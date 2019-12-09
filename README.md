Topics over lunch is written with the motivation of bringing fascinating topics in Computer Science to a general audience.

# Table of Contents
1. [Encryption over lunch](#encryption-over-lunch)
2. [Assymetric Encryption](#creating-a-shared-secret)
3. [CS3216: The first task](#cs3216-what-i-will-learn)
4. [CS3216: Making Great Presentations](#cs3216-good-presentations)
5. [CS3216: On Cross Platform Mobile Apps](#cs3216-cross-platform-mobile-apps)
6. [CS3216: Financing A Startup](#cs3216:-financing-a-startup)
6. [Review of CS3216](#cs3216-learning-outcomes)

## Encryption over lunch

The subject of Crytography has always been the subject of much media speculation and public interest, and has suffered the degree of distortion guaranteed of such topics. To an uninformed reader, it may appear that any meaningful understanding of Cryptography requires a strong mathematical background. While this is certainly true of those aspiring to work with such systems, some valuable insights and ideas are conveyed well regardless of subject training.

### What is the problem?

We begin by characterising the problem that motivates some well known Cryptographic protocols. Suppose that two individuals Alice and Bob wish to communicate with each other through some medium (letters, perhaps), on some important matters. The problem arises that this communication could be attacked a malicious party, who decides to: 

- spy on the communication, hence compromising the **confidentiality** of the conversation
- forge messages in the communication, hence compromising the **authenticity** of messages
- seize all letters from the mailbox and preventing their delivery, hence compromising the **availability** of a communication system

It is easy to see that parties with truly malicious intent could cause damage by interfering in private communications such as those between a client and a bank, for example. Cryptography focuses on the former two of these problems; there is not much meaning to exploring any communication system when messages are arbitrarily stolen. 

### Protecting message privacy

We now set about the task of exploring systems that satisfy these requirements, step by step. As a first step we attempt to find a way to protect the confidentiality over the message. Suppose that Alice wishes to share with Bob a message `m = "Hello Bob"`. Sharing this message in its current form, would of course allow any eavesdropper gain the complete meaning of the message. 

However, suppose that we applied some transformation (called encryption) upon this message m, which resulted in some other seemingly unrelated string,(perhaps `and1c3de5jd` or any other string you can write down) and only shared this over the communication system. Unless the eavesdropper had knowledge of the exact way in which this transformation was applied, he/she has no way of recovering the message m, and hence is not aware that the message exchanged was indeed `Hello Bob`. If Bob knows a method to transform the secret message back to its original form (called decryption), then in a very primitive sense, this system allows Alice and Bob to communicate without anyone eavesdropping on the conversation. Anyone may acquire the encrypted string, but only Alice and Bob are able to derive the actual meaning from them.

In order for this to be useful, we require that:
- Alice and Bob agree on an encryption and decryption method.
- no one else knows of these methods.

In real life, it is neither possible nor desirable that every pair of communicating parties agree on a new and secret method of communication. Instead we consider an alternate approach where Alice and Bob agree on a secret password (called a secret key) before hand. We modify the above system such that:

- one can only decrypt a message if the key with which it was encrypted is known.
- all communicating parties use the same encryption and decryption method.

This is analogous in some sense to the way we generally log into our email accounts. That a username and password are to be typed in are common knowledge, however only your specific password unlocks your account. In other words, the **protocol**, or process to unlock an account is publicly known and common to everyone, yet impossible without your secret key. 

So Alice and Bob now agree on a secret key beforehand, and choose a publicly known encryption and decryption method. When Alice sends `Hello Bob`, she encrypts it with the function and that specific secret key. Bob then uses the same secret key to decrypt the message. The eavesdropper obtains the encrypted message, and knows the method to decrypt a message in general, but is unable to do so without the secret key. 

![Figure 1: Encryption and Decryption](images/Figure_1.png)

This is one simple way Alice and Bob can encrypt their messages to keep them secret from eavesdroppers. This systems seems to work, provided that:

- Alice and Bob have agreed on a secret key before hand, and nobody else knows of this key.
- we know of suitable encryption and decryption functions.

The system suggested above certainly still is not without issues. If a third party guesses the key correctly, then he/she will be able to read all the messages. It must hence be hard to guess the key. Alice and Bob will also need some way to secretly agree on such a key. Without this, this method will not work at all. Additionally, it provides Bob with no way to verify that Alice indeed sent the messages, and not someone else. 

The system discussed above is called **symmetric encryption**, since encryption and decryption both _symmetrically_ use the same secret key. There are other methods where different keys are used.

In future articles, we will discuss other methods that improve on the security of this scheme.

## Creating a Shared Secret

In the previous section, we discussed the benefits of encrypting communications for the benefit of privacy. While this might seem sufficiently secure at a first glance, we will see that the practicalities of the real world demand the specification of a number of other schemes and processes in order to enable this encryption. 

For example, the scheme discussed above (referred to as **symmetric encryption**) require Alice and Bob to share a secret key that no other party is aware of. It is unclear so far as to how such a secret may be generated in the event that the parties cannot agree beforehand on such schemes through some other means. In this section, we discuss this problem further, and open a broader discussion on the practicalities of symmetric encryption.

Let us begin again by characterizing the problem that motivates this discussion. Suppose that Alice and Bob are in different parts of the world, and wish to communicate without anyone else eavesdropping on their communications. Recall from the previous section that they can use symmetric encryption to do so, provided they somehow agree upon a secret key without others realizing this key. This requires that they first negotiate a secret key, before communicating. The problem now distills to the following:

```
Given that a third party can listen in on all possible communications between Alice and Bob, how can they agree upon a common secret that others do not know about?
```

In the context of the internet, it is possible that Alice and Bob have never met before (Alice could be a server in different country). It is hence not possible, or practical to rely on shared secret information to create a key. We must create a secret in plainsight.

The following is a brief description of the approach. Please note that this section compromises some mathematical accuracies in the interest of simplicity.

In 1976, researchers proposed the Diffie-Hellman protocol to address this problem. The approach to solving this problem begins in a seemingly disconnected mathematical property of numbers: `that under some conditions, it is easy to calculate the exponents of numbers, but  much harder to reverse this process`. This means that in certain groups, 

- calculating g<sup>x</sup> given `g` and `x` is a simple problem, and,
- calculating `x` given `g` and g<sup>x</sup> is a much harder problem

(ignore the semantics of the mathematical structure `Group` if it is not relevant to you) 

It can now be seen how this mathematical property can be used to create a shared secret. Observe the following interaction. In this discussion, Alice and Bob are attempting to negotiate a secret key and Eve is attempting to listen in on the conversation and steal the key:

- Alice randomly selects a number `x` and calculates g<sup>x</sup>. As we discussed before, this can be achieved easily since it is efficiently calculable. She tells Bob, "Hi, I'd like to chat and here are two numbers g<sup>x</sup> and g."
- Eve can hear this, and she writes down both g and g<sup>x</sup>
- Bob notes down the same details. He now samples a random number y, and similarly calculates g<sup>y</sup>. He tells Alice, "Hi, here are my two numbers g and g<sup></sup>"
- Eve notes these down as well
- Alice receives this and notes down as well. 

At this point, each party has the following details:

- Alice: x, g, g<sup>x</sup> and g<sup>y</sup>
- Bob: y, g, g<sup>y</sup> and g<sup>x</sup>
- Eve: g, g<sup>x</sup> and g<sup>y</sup>

Suppose they each attempt to calculate g<sup>xy</sup>:
- Alice can raise g<sup>y</sup> to the power x and obtain g<sup>xy</sup>
- Bob can raise g<sup>x</sup> to the power y and obtain g<sup>yx</sup>, which is the same as g<sup>xy</sup>
- Even cannot calculate g<sup>xy</sup> directly, without first calculating either x or y. As we discussed earlier, the problem of obtaining x or y from g<sup>x</sup> or g<sup>y</sup> respectively is considered very hard if the suitable g is chosen. 

Hence, Alice and Bob can generate a shared secret key in plain sight. Remember, these properties are not true of all numbers. They are exclusively true of some mathematical groups, and a further discussion on this would be non trivial. 

The above protocol allows use to now see completely how two unrelated parties can communicate a shared secret key in the presence of malicious actors. They first employ this key-exchange protocol to obtain the shared secret key g<sup>xy</sup> and then proceed to use that to symmetrically encrypt all communications. 

The practicality of symmetric encryption is now vastly extended. It can be possible for two parties who have never met before, to communicate confidentially without having to share any secrets before hand. This brings the simplicity of symmetric encryption to use in the era of the internet. 

Even with this protocol, a multitude of issues remain unresolved and undiscussed. We will further our discussion of secure communication in later sections.

## CS3216: What I will learn

One of the ideas that has stuck with me is that ordinary people can do extraordinary things (Elon Musk). In essence it captures the idea that any ceiling we define for our own abilities will only serve to limit us, and the only true way to keep improving is to always increase "the ask" from ourselves. This is the mindset I bring into CSS3216. My learning objectives from the course are hence of two streams.

 

The first stream is involved with the process of developing a software product and working through the (long) path from ideation to solution.

 

I am interested in learning about and experiencing the process of identifying a problem statement, and understanding some defining traits of a good product. I hope to be a resourceful teammate who can quickly adapt to different teams and projects, and contribute to the effort by hitting the ground running. Alongside, I would like to leap out of my comfort zone (in terms of tech stack or role within the team) and learn to contribute in roles that I currently do not feel confident in. After all, in a world as dynamic as software product development, there is no skill we can wave away as unnecessary.

 

I would also like to consciously engage in peer learning. During the networking activity in the first lecture, I discovered several (future) teammates who have skills I would like to pick up (design, UX, etc.) and I would like to actually learn some tangible skills from them. To me it appears that the problem statements of the assignments are really only head fakes to help with these learning outcomes. Finally, and most obviously, I would like to gain experience building quick prototypes that have the ability to showcase the impact of an idea.

 

The second stream of learning is to embark on a truly involved thought journey on the traits of highly motivated people and discover the limits of my abilities.

 

“The Last Lecture” was truly an incredible way to begin this journey, and I found myself inspired to think more about this stream of learning objectives. I am entering CS3216 with intent to push myself further and learn several things from this process. It is my previous experience that every hard learning experience teaches me some valuable lessons about how to learn better in the future, and they have served me well in every part of life. At the end of this module, I hope to emerge with a more refined process of learning, and valuable insights on how I can improve my work style. I want to teach myself to not be satisfied with a project, and go through with that extra iteration (or perhaps ten) that will help me improve even more.


I am sure my learning goals in CS3216 will evolve over the semester owing to the diversity of class, and the varied nature of the tasks. I look forward to the experiences, and of course "staying hungry and staying foolish".

## CS3216: Good Presentations

What if I told you that this thirty second read could increase the chances that your start-up wins a huge grant? I believe that the learning outcomes from the talks were extremely important, actionable and presented immaculately, and can be an asset if used properly to enhance your presentations.

This session covered two topics: UX and Presentation Skills. Through the class, I was able to develop an idea of the high level process of iterating over design and some ideas on how I could improve the impact of a presentation. 

The following are the key lessons for me from the class:

Focus is key: If you don't have focus, you're really just shooting from the hip.

This learning outcome was common to both sessions in the class.

During the presentation session, I learnt that if you don't have a 30 second version of your presentation, you don't have a presentation at all. The most important part of a presentation is the 2-3 key points you want to get across to the user, failing which the whole presentation was to no avail. It is important to structure your presentation to be able to deliver this message in an impactful manner, and some of the strategies to do this were my other salient learning outcomes. My action item from this is to always evaluate my presentations by checking that I can distill this essence out of all my talks, and that they are aligned with the goal I began with. All numbers, facts and content must hold some relevance to one of these key outcomes.

During the UX talk, I learnt that focus on the primary problem we are trying to solve is extremely important and that this must manifest itself not just in the form of the product and its features, but also in the experience we provide the users, from aesthetics to design and process. It is far more important to work on improving the feature that really solves the issue, and it's usability rather than developing other peripheral features. This should be the driving force behind the design phase of the task. 

Inspire Action: Presentations that inspire action from the audience are the most successful ones.

This learning outcome is specific to the presentation section of the class. 

There are presentations that change what you know or believe, and then there are those that change how you behave. I learnt that the most impactful presentations are of this kind, and provide the best avenue to share an idea with people, be it a product or something else. Inspiring action from the audience requires us to introspect on what they gain from this presentation (or WIIFY). Whats-In-It-For-You (WIIFY) is a great way to remember to share with the audience what they stand to gain from the presentation or what they risk losing if they miss it. 

Grabbing their attention is a necessary prerequisite to this, and this can be accomplished in a number of ways. The ones that I found most attractive were the ones that Challenged the user, those that were Personal, and those that began with a "What if I told you .. " statement.

Ultimately, I understood that a combination of rational and emotional factors play a role in inspiring action, and learnt some ways to tap into these. My action item form this is to ensure that I focus on inspiring action (e.g., using a product) from my audience than merely sharing information about my product.

Last peak of interest: You get a lot of attention when you're wrapping up. Don't waste it.

While this may seem like a small learning outcome for many people, to me this was extremely significant. Prof Damith shared a valuable insight on the fact that the last peak of interest that we get from the audience is when we say "In conclusion, .." or another sentence that flags the ending. I learnt that we must harness this, and from my own experience I have found that talks that used the ending to reiterate the key points left me thinking about them more. This is best utilised for a call to action, sharing a core message or something very important to the talk.

In summary this class left me with great learning outcomes on how to develop a presentation or a product that really speaks to the user's needs. Apart from these outcomes I learnt about a great way to structure presentations as well. I look forward to applying these during future tasks. 

## CS3216: Cross Platform Mobile Apps

During this week’s lecture, we explored the development of cross platform applications using React Native and Flutter, and contrasted these with developing native applications. Through this lecture, I learnt about the challenges of developing applications for multiple platforms (such as Android, iOS and web), and the nuances that come with selecting the tech stack.



I had three primary learning outcomes from this lecture:



Selecting the right tool for the right job
The trouble with cross-platform apps
Reality of maintaining cross-platform apps

 

Right tool for the right job

My first learning outcome from the lecture was that we must make sure we select the “right tools for the right job”.

 

Developing a good product involves taking into account a good representation of the expectations of the users, and this may involve factoring in the hardware they use, their primary use cases, internet connection speed, etc. This induces constraints on the tech stack, and the developers and product managers must be sure to respect these user requirements while choosing the stack.

 

In the real world, there is no silver bullet that ticks all the boxes. Sometimes, the right solution is a non-standard composite solution that selects the right tool for each job, similar to the one Prof Ben described from his experience.

 

Ultimately, it all comes down to separating the product from the tools we use. We must ensure that the product requirements come first, and that if need be, we alter the stack to suit those needs.

 

The trouble with cross-platform applications

 

Having never simultaneously supported an application on different OS, I was quite interested to learn some of the key difficulties of developing and maintaining cross platform applications.



Cross platform applications are challenging from the perspective of management, development and maintenance, and the lecture gave some great insights on industry practices and challenges.

Individually having multiple teams develop native applications for each platform is an expensive solution that also requires great deal of co-ordination to ensure that the different apps provide similar user experience, identical features and follow the same release schedule. As a result, the speakers presented some alternatives for cross-platform development.

But solutions such as React Native and Flutter come with their own baggage. While a single code base is supposed to be perform identically on different platforms, sometimes the requirements of the product might not permit the subtle differences between the platforms, and will then require manual effort. Further some native development is required to support custom features and views that are too complex for RN or Flutter.

 

Maintenance of this code also presents challenges as the frequent releases with breaking changes require manual intervention. The updates also come with a deadline and require quick action on these fronts.

 

How it happens in the real world

 

The perspective from the psLove and Amulya on how these challenges may be handled in a production environment were quite eye opening. I understood the importance of weighting capabilities, limitations and developer satisfaction, while deciding the tech stack for a product/project.

 

Additionally, I learnt about the tradeoffs while working with Flutter and ReactNative in terms of the performance, abilities and limitations. This gave me perspective on when to switch to Native, should the need arise.


I also understood the difficulties of performing regression testing while working with technologies that are updating so quickly, and learnt about how this is done for some production applications.

 

In summary, from this lecture I learnt about the importance of selecting the right tools, the difficulties of developing cross-platform apps and the practicalities of maintaining them in the real world.    

## CS3216: Financing A Startup

In the interest of avoiding the pitfalls of writing a wall of text (that we were warned against today), here are my top 3 learning points from today. These are the summary of what I found most useful today: 


1. how much money to take
2. from whom we should take money
3. how to do it.

### How much money is the right amount of money?

How much to raise money is an art and not a science. That said, there are some important issues that should be considered. Raising money should never become the focus of the business, it should only serve to mitigate two issues:

- Too little money can shorten your runway, and prevent you from taking off
- Too much money can land you in trouble:

    - It can encourage irresponsible spending practices, as evidenced by the Pakistani ride sharing companies which did not fare as well as the ones that raised lesser amounts. 

    - You may not be able to justify your valuation, e.g., WeWork

    - You may not be able to deliver on the expectations on that investment

### Who are interested in investing, and why?

Always bear in mind what your investors believe in, and what they want from you. This will help you ensure that your directions do not diverge vastly at some point, and manage expectations well.

- Friends, family and fools: They might provide money simply to help you pursue your dream.

- Angels: They care about receiving money back, or in some cases “paying it forward”

- Grants and governments: These must be worked on with caution since they are usually given out with some idea/agenda behind 

- Venture capitals: These people are in the business of actually identifying good ideas and throwing their money behind them. 

Venture capitalists are the most reliable, and here is how you excite them:

- Team: Show them that you have the right people 

    - These are evaluated by proxies (much like the job market, it seems)
    - Track records of the people and their backgrounds

- Unfair and exclusive advantages:
    - Patents, licenses and other permits that may prove to be a barrier to entry for competitors
    - Pre-existing assets relevant to this business
    - Vendors and customers locked in

- Hot markets and segments:
    - VCs will flock to hyped market segments
    - Potential 

- Early traction:
    - Revenues, real users, and people really paying to use your services are all good indicators for VCs

### FAQs on the Structure of Financing Rounds

This is one of my key learning points because I keep hearing these things, but didn’t know what they meant until now. This may be of some use to look at:

- Why does an investor invest? 

    They give you money to build your company, and in exchange they get a proportional amount of your future proceeds and profits.

    Dividend: Dividing up the profits among the shareholders of the company.

- Are there other ways of raising money?

    Debt: Asking for money from a bank, and promising to pay back with interest is one way. 

    This is risky for the investor for a startup without collateral, and that is dangerous for you. 

    Convertible Debt: If you do not pay, they will just take up shares of the company instead at a later point. 

    This can be useful when the valuation of the company is unclear. You can defer the discussion of valuation to a later date.

    This can go really fast, so speed of closing is higher. 

- How do people value start-ups?

    Depends. Do you want the theoretical answer or the practical answer?

    Theory: Revenue, profits, users, etc. 

    Practice: Whatever the entrepreneur and the investor agree its worth

- Who owns how much of the pie?

    In every investment round, new investors will take about 10-15%, and dilute the existing shareholders. So the ownership of the company gets fragmented.

- What do you pay yourself?

    Some general principles that can help you decide your own salary:

        - Your salary shouldn’t be too high: it should hurt if the company goes bust
        - It shouldn’t be too low: Don’t lay awake wondering what you will eat tomorrow


Nugget of Information: Why do you think Grab has to be a bank now? It seems to be the only way to justify their valuation.

## CS3216 Learning Outcomes

This is a long post, so here is a tl;dr. These are some of the things I learnt in CS3216:

 - Raise the bar and push yourself
 - People need to take responsibility and own their tasks - take pride in your work
 - Make decisions in time
 - Keep the big picture in perspective 
 - Take positive and negative feedback in the right stride
 - Document your progress
 - Learn to be flexible

So, what DID I learn in CS3216? I’m not sure where to start. This has been a crazy journey with plenty of disappointment, pondering, complaining and satisfaction - and there was time for each in just 12 weeks of perpetual deadline dash. 

By the numbers, it has been a very busy semester:
 - 3 different teams to work with (in my case)
 - 4 different assignments
 - 2 presentations
 - Product pitches
 - user testing
 - thirteen writing assignments
 - three consultations. 

This is more than any module in NUS has asked of me, and while I’m pretty sure my other mods have taken a good beating, this is as much as any module in NUS has taught me. 

Hindsight is 20/20, and so it’s probably the best way to learn some lessons. 

I learnt to be a better developer, I learnt how to work with different styles of teammates and push as hard as possible, but my key learning points have been the finer things in here:

 - Forget the requirements


    Every project in university begins with looking at what the course requirements are. Prudent as that may be, this is a major way I’ve held myself back, instead pushing to make my work that much better. Working on projects by just trying to keep pushing forward with improvements is the only way you’ll push the boundaries of what you know, and learn something new. 

    So sometimes, it’s best to set your bar higher than you’re required to.


- Responsibility is a necessary teammate

    People often think that finding the most skilled developers, designers (and whoever else you project needs) is the most important thing. Past a basic threshold of skill, those who take responsibility for their part in the project are always the better teammates. When the going gets tough, if people don’t take responsibility for their parts, it either piles up on the others, or is completely lost in the team. Either way, it’s not easy to tide the difficult times. 

    People who take pride and responsibility in their work contribute to a better team environment.


- Decisions, decisions, decisions

    Making the right decisions is not just about knowing the right things. It’s about actually pushing through to make sure that decisions don’t hang about longer than they have to, and that they are made by the right people.

    Starting to feel that the stack is going to constrain you? Worrying that you’re deviating too far from the main course? Are you having growing concerns about code quality? 

    Sometimes, nobody wants to be the one to make a decision. If you discuss the same topic over and over, it gets more and more stale, and it becomes the status quo to leave a meeting without resolving issues. Bringing up the right issues isn’t enough. You need to see it through to the end and ensure that the decision is made. 


- Big picture vs Details - Don’t lose perspective

    One of the major takeaways from the consultations we had with Prof Ben during the final project was that we regained some perspective of the “big picture”. It’s easy to be convinced that an idea is good, and get lost in the details before you’ve done your due diligence regarding validating an idea. 

    Always have a sounding board, and don’t dismiss their opinions as uninformed. After all, you’re building the app for a user base, not just yourself. 


- Taking feedback the right way

    One of the best things about CS3216 for me was how tight the feedback loop was. Every submission, assignment and task had some sort of feedback loop, and I actually found that to be quite useful. 

    We got plenty of feedback on our work - sometimes positive, sometimes negative. Taking this feedback the right way was an important lesson I learnt. Trying to take the negative comments as challenges to overcome, and thinking of them as personal milestones worked quite well for me. Getting recognition for both good and bad work from the Prof gave me drive to try harder.

- Document your process

    In the absence of accountability, processes break down. If you don’t have a backlog you regularly look at, or an audit trail of what decisions were made when, things will eventually devolve into a lot of confusion. 

    Every meeting should have minutes. 


- Learning to be flexible 

    Having a balanced team is important, you need some developers, some designers and someone to be a manager. But sometimes, when people don’t naturally assume these roles, try to take the initiative, and assume the role the team needs the most. You may not already have the skills you need for that role, but the onus is on you to discuss that.

    If your team needs a better (manager/devops/PM), then you should try to become that person. 

Working with the people in this module has given me so many good opportunities to learn. Thanks to everyone who helped me learn in this course and best of luck to you guys with the rest of your mods! 

Thanks Prof Ben for the great speakers, the inputs, and the continuous feedback you have given us throughout this module. 

