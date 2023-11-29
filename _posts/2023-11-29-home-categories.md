---
title: "Setting up the Staticman API for your personal site in 2023"
header:
  teaser: "/assets/images/2023-11-29-home-categories/screenie.png"
  og_image: "/assets/images/2023-11-29-home-categories/screenie.png"
categories: 
  - Software
  - Personal Projects
tags:
  - software
  - projects
  - webdev
  - meta
---

Brief post about the new homepage layout, plus some rambling on web dev and other goals for the website.

---

You may have noticed that I've added three category lists on the home page. My assumption is that some readers will be interested only in specific things I talk about, rather than every little thing I write up here. The goal of this change, therefore, is to make it easier for those readers to access what they want to see.

To make this work, I directly added elements to my `home` layout that reuses some content from Minimal Mistakes' existing layouts. There's some very small styling changes, but otherwise it's more or less the exact same as the "related content" material at the bottom of a default post. This isn't an amazing solution because it duplicates code, but the duplication is minor for now, and I think making a whole `_include` file for this right now would be overengineering. 

If you're curious, you can check out [this commit on GitHub](https://github.com/michigan-musicer/michigan-musicer.github.io/commit/828e8a76263a200e0ee80f8dac87e5192ddf04af
) for the additions I made. I imagine this isn't anything too crazy for a dev with a few years of experience.

For me, the big issue is that these category bars take up a lot of vertical space; you have to scroll a bit to get to the bottom now, and it's not obvious that the recent posts are at the bottom. I made a small custom class of the grid items to remove the bottom margins, and this saves some vertical space, but I'd like to squish it a bit more. On mobile, these eat up even more space. Considering only the PC experience, I think the most intuitive fix is to put these in a sidebar like the current "about me" sidebar taken from the Minimal Mistakes template. I could also add them . However, neither of these feel like amazing fixes on mobile. So I'll have to put a little more work into this to be satisfied.

Main questions:
- How to make this a proper `_include` and avoid code duplication? 
- How to reduce the number of words in the teaser?
- How to integrate smaller grid nicely into mobile? 
    - Ideally, instead of having everything appear vertically, they'll appear in a row like they do on PC?
    - Change limit on number of posts visible if on mobile?

---

Working with a big open-source Jekyll template and making stuff with that template is a mildly eye-opening experience. If I sit down and take the time to parse through everything[^1], I realize that the sites I've made in the past are...not that advanced. My old website at [www.michiganmusicer.com](www.michiganmusicer.com) has a lot of small CSS flaws (for example, try toggling between the different filters and observe the elements on the ), and certainly the technology I'm using in that website is not as advanced. Creating this new site has given me a lot of respect for actual web devs and their work.

[^1]: which I personally don't think is the most effective use of developer time, but if you want to learn stuff then by all means go ahead. 

The thing that feels difficult for me is finding the patience to sift through all the different options out there -- all the frameworks, tools, and language features that are all supposedly better than each other. I *can* do it, but I would really prefer if there was one authoritative source that said "this is the best one, use this, also this will be maintained and will continue to be the best one for a while". I know "best" isn't really a term in software engineering that means something[^2], but *surely* there's some better way to filter down to the better / more modern methodologies out there?

[^2]: well, it does, but you need to have well-defined constraints that don't move enough to destabilize that definition of "best".

Which brings a question to mind: should we be satisfied with the current web dev ecosystem, or should we be pushing for big fundamental changes? I mean, people bully JavaScript all the time (and at a glance there's a lot of stupid stuff in JavaScript), but given that the web is *already written* using mostly JavaScript, should we actually push for something better? Or just say JavaScript is fine? Let's say we are given the option to take down the entire internet for a year, no consequences[^3], so that we can develop alternatives to all our existing infrastructure. Would we come out with something better? I am NOT qualified to answer this question, not even 1% of the question, but it'd be nice to hear opinions from people who have been working in this space for decades.

[^3]: obviously this could never happen. If you disabled *all* internet for 30 days I imagine there'd be a very real risk of a global societal implosion. Too much of our infrastructure depends on internet comms.

At the very least, I think (based on my experience) it can be useful for every developer, especially students and other learners, to make their own website. The tech you're using is pretty different from anything backend or systems-oriented, and for engineers interested in those more "complicated" technologies (self included), you don't want to overinvest into this space. But to make a serviceable website, you will have to learn about reading, using and adding to other people's code[^4], creating projects using third-party services, and adjusting a product based on needs / requests from end-users. There's a saying somewhere that software engineers never say "it's impossible"[^5] regarding a product, and I feel like web dev preaches that saying more than, say, adding a new pass to the DirectX shader compiler. 

I am planning on starting up a coding club at my university, and I think to start, I'll teach everyone how to create a Jekyll website using the default Minimal Mistakes template. I think it'll be nice for people to have something that looks nice, but is still relatively easy to tinker with (at least in comparison to a typical beginner project from UofM).

[^4]: unless you code your site purely using raw HTML / CSS / JS. Technically you can do this and make a nice website, but you'd metaphorically reinvent so many wheels that you could make the world's first metaphorical...megacycle? A bicycle except it's 1 million wheels instead of 2? I can do that, right? English is weird enough of a language for me that I can do that? Anyway, people would honestly judge you for wasting time on solving problems no one cares about anymore.

[^5]: unless it's genuinely an undecideable problem, although then you could ask "do human minds have the same limitations as Turing machines?" and whether it might actually be solveable by humans. Thanks Professor Amir Kamil in EECS 376 for introducing that idea on Piazza a few years ago, it was mindblowing at the time.

---

Other goals for the site:

- custom colors!
- drawing a custom logo to replace my headshot
- incorporate ReCAPTCHA for comments
- add a portfolio page
- move to custom domain
- set up custom email addr that's not just my umich email (in case alumni services axes that)
- set up a newsletter (i.e. somewhere you can sign up to receive updates)
    - Note that the newsletter must come after the custom domain and email address. If I set it up now, I have to set it up again after moving, and that would be a hassle.

All in due time. At the very least, these need to be completed before I start applying for jobs in April 2024.

I don't want to keep doing web dev, though, so eventually I will have to say "this site is good enough" and transition to something systems-y (LLVM, Godot, or some other large software ecosystem for me to do stuff in). I have contacts in Poland interested in Godot, so that is probably what I'll pursue.
