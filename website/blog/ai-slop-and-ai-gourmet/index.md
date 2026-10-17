---
title: AI slop is real. So is AI gourmet.
description: Some people have called octomon "AI slop". Yes, AI has helped me build it. No, that is not what slop means. Here is what it does mean, and the name I'd like for the other thing.
date: 2026-10-17
draft: false
---

Since I started posting about octomon, one comment keeps turning up. 
It arrives in a few different flavours, but it always says the same thing:

> AI slop.

Sometimes it comes with a reason. Mostly it doesn't. Sometimes the comment has come from someone who's actually tried the tool, but mostly I don't think they have. So let me address it
head on, because I think the people saying it are half right, and the half they have wrong is worth a proper argument.

## The food processor

Let's get the easy part out of the way. If your objection is simply that AI was involved in writing the code, you are the cook who refuses to own a food processor because real chefs chop by hand. Today every professional kitchen has one (and a sous vide or two), and nobody can taste the difference in the soup. **Every** company that ships software today is using AI to write some of it. Show me one that isn't.

I'll wait.

This isn't a prediction, it's the reality of development right now. The question of whether software should be written with AI [was settled](https://www.theregister.com/ai-and-ml/2026/10/14/linux-kernel-devs-drop-obligatory-confession-of-ai-tool-usage/5303140) while people were
still arguing about it. What is left to argue about is whether a given piece
of software is any good, and that question has never cared about the tools used to write it. Someone at work mentioned a quote, I can't remember who it's from, but it's accurate.

> The difference between AI slop and something better, is you.

Another quote that I have LIVED by my entire career... quotable to no-one but the industry itself.

> Shit in, shit out

Finally, while I'm pulling out my favourite quotes, here's another one that has lived with me since working for my first startup in the 90s.

> Keep It Simple Stupid (K.I.S.S.)

Which actually applies a LOT to some of the AI coding i've done, because AI often can make a simple task overly complex and that's where the human has to rein it in.

So when "AI slop" means "AI touched it", I'm not interested. When it means something more specific, I am very interested, because slop is real and there is a lot of it.

## What I think slop actually is

Here is what I think people mean when the word is used with accuracy. Someone with a
shallow understanding of software asks AI for a thing, gets a thing, and
ships the thing. The first version is typically the only version. 

- Nobody asked what happens when the input is hostile. 
- Nobody asked what the program writes to disk, who can read it, or what it sends over the network. 
- Nobody asked what
happens on the second machine, or the hundredth hour of uptime, or when the user does the thing nobody expected. 
- Nobody designed the interface, it just fell out of whatever the model produced first. 
- And nobody who was an expert ever looked at it and said "no, that's wrong".

Security, privacy, reliability, change control, user experience. The AI
didn't skip those, the person didn't think about them.

As the quote above states, and more than one mentor has told me over the years: shit in, shit out. A
model given a vague prompt by someone who can't tell a good answer from a
plausible one will produce a plausible one. That is slop. On that, the commenters and I agree.

But let's be honest about something else. By that definition, slop is not
new, and it is definitely not an AI invention. A great deal of the software published
over the last 30 years was slop. I have used it, I have debugged it, and I have written some of it! Code copied from a forum
post without understanding it, then a tutorial, then Stack Overflow, with
the variable names changed and the error handling left as an exercise.
No threat model, no thought about what it wrote to disk, a UI that was
whatever the framework's default looked like, version 1.0 and never a
1.1. The tooling got better and the documentation got better, and the
slop got a bit better with it, but it never went away. If anything, a
modern model given a lazy prompt often produces something tidier than
what a lot of people were shipping in 2005.

So the honest version of the complaint is not that AI produces slop.
People have always produced slop and while a lot of it never gained much traction, unfortunately, I am old enough to remember using Internet Explorer 6, Adobe Flash and Lotus Notes. But I digress.

The issue now is that AI produces slop at a volume
we have never seen before, because the cost of making it has dropped to
nearly nothing. That is a real problem, and the Internet is now full of
it. It is also a very different complaint from "this particular tool is
slop", and the two keep getting confused.

## The other thing needs a name

But the same tools, in different hands, produce something else entirely. AI
is a fantastic way to write very good code, fast. The catch is that
everything upstream of the code has to be good first: the idea, the design,
the architecture, and yes, the prompts. Give it a clear specification from
someone who knows the field and it is like having a tireless pair
programmer who has read every RFC and never gets bored of the fiddly bits.

We have a word for the bad end of that spectrum and none for the good end,
which is how "AI slop" ends up thrown at everything. So I'd like to coin
one. If AI slop is the unappetising, watery, badly prepared meal that looks
and tastes like animal feed, then **AI gourmet** is the Michelin star
dish: high quality, a complex flavour profile, specialist preparation,
sometimes a rare ingredient or two.

Same kitchen. Same knives. Completely different chef.

## What makes it gourmet

So what are the qualities? Having spent the last two and a half months building octomon this way, I think it comes down to four things.

<figure>
  <img src="chefs-kiss.gif" alt="A cartoon chef in a tall white hat closes his eyes, kisses his bunched fingertips and flicks them open">
  <figcaption>Chef's kiss. Source: <a href="https://tenor.com/view/chefs-kiss-french-chef-perfect-dish-excellent-food-perfection-gif-20341505">Tenor</a></figcaption>
</figure>

### Iteration, and a lot more of it

AI lets you prototype, test and throw away in minutes instead of days. The
slop mistake is to treat that as a reason to iterate less: the first
version arrived so quickly, why not ship it? The gourmet move is the
opposite. Faster iteration means you can afford far more of it.

octomon has had 45 releases since the first changelog entry on 11 August.
That is not because I'm bad at planning. It's because every release went
onto real networks (home fibre, plane Wi-Fi, an Azure VM, a Windows
laptop behind NordVPN and WARP) and every one of those networks taught me
something the last version got wrong. The [changelog](/changelog) is the
record. Read a few entries and you'll see the same features being reworked
three and four times as the edge cases turned up.

### An experienced human in the review loop

I love that AI can bring software development to people who could never
have written a line of Rust before. I genuinely do. But I also believe that
right now, to produce production quality software, you need a human in the
review process who understands the domain, and that this is the single
biggest thing separating gourmet from slop.

While building octomon there were plenty of moments where the AI made an
assumption that looked perfectly reasonable and was wrong, and it was 25
years of networking and development experience that let me catch it. Three
that come to mind:

- **Targets you add were voting on whether the Internet was up.** The
  first implementation treated every monitored host equally. Then my son's
  three Fortnite servers, which never answer ICMP, outvoted the built-in
  targets into "Internet unreachable" while the game played fine. Only
  the built-in targets should ever decide that. Obvious once you've seen
  it, not obvious from the code.
- **A gateway that drops pings is not an outage.** Airport, hotel and
  office routers rate-limit or refuse ICMP all the time, and the hops past
  them are perfectly clean. An early version flagged this as a problem.
  A network admin knows it's a policy.
- **Switching a VPN on looked like 30% packet loss.** During the initial design and build, I never gave VPNs a thought and then while testing I switched mine on. The probe windows
  reset during the switchover and the first few seconds of loss were
  a combination of octomon finding its feet and the VPN handover finalizing, but I hadn't designed for this and instead octomon was reporting the connection failing. That needed a
  settling grace, and it needed someone who knew what a VPN handover
  actually does on the wire to recognise the pattern.

None of those are things the model got "wrong" in the sense of a bug. It
did exactly what was asked (or rather wasn't asked). They are things that only a person who has sat
in those networks would know to ask for. That is the review loop, and it
isn't optional.

### You have to actually understand the code

Closely related, and I think just as important: you have to be able to
read what comes back. Not every line, not every time, but well enough to
know when something is wrong, and well enough to write some of it
yourself.

In my case I am well versed in Java, Python and the web languages (HTML,
CSS, JavaScript), with some C in the distant past. I used octomon as my
excuse to learn Rust. So the language was new to me, but none of the
concepts were. A loop is a loop, a struct is a struct, and a function that
returns an error you then ignore is a bug in any language. The borrow
checker was new. The idea that you should think about who owns a piece of
memory was not.

That is a very different position from not understanding the code at all.
When the model produced something that compiled and looked plausible, I
could tell whether it was also right, and when it wasn't I could say why. I'm not convinced you can produce AI gourmet without that.
You can produce something that works, for a while, on your machine. But
you can't review what you can't read, and we've just established that
the review is where the quality comes from.

It's also been genuinely fun. A lot of octomon I wrote myself, by hand, as a way of learning the language properly. In fact, I let AI mostly
handle the UX as it was easy to verify the output (I just look at it) and I wanted to focus on the core monitoring code. But because I didn't know Rust, I would ask it to draft a function and teach me parts of Rust that I might not have known in that context. It really was pair programming at its best and I enjoyed that as much as
any of it. AI didn't replace the craft. It gave me a tireless tutor
while I picked up a new one. Have I mastered Rust? Not really, but I know enough to contribute my own code and review what it's writing.

### An idea that fills a real gap

The last ingredient is the one no amount of prompting can supply. The idea
has to be good. octomon exists because when my Internet connection wobbles
I end up with four terminals open, ping and traceroute and dig and btop,
and nine numbers that between them never answer the question I actually
have. You can find eight or ten tools that each do a piece of this. I
could not find one that put the whole connection on one screen and told me
what was going on. I had spent years looking for a good tool and never found what I wanted. So I built one. If the thing you're building already
exists in five better forms, speed of production doesn't help you; you've
just made duplicative slop.

## Judge the dish

I'll end where the food processor started. In a few years, "written with
AI" will be about as remarkable a disclosure as "chopped in a food
processor", and the cooks who swore by the knife will have quietly bought
one. What will still matter is whether the dish is any good.

So judge the dish, not the kitchen. octomon is open source, [the
algorithms and thresholds are written up](/blog/technical-paper/) in
tedious detail, [everything it stores is listed](/privacy), and it runs
unprivileged so you can try it without trusting it. If you taste it and
it's slop, tell me precisely what's off. That's a bug report, and I'll
iterate on it. Which, if you've been paying attention, is rather the
point.

Do I think octomon is AI gourmet? Nope. But it's on the stove, bubbling away and it's smelling pretty darn good. Maybe a little more salt?

---

Oh and full disclosure, AI drafted this blog article and then I hacked away at it for a few weeks before publishing.

If you find octomon useful, a
[GitHub star](https://github.com/securitypedant/octomon) is much appreciated.
