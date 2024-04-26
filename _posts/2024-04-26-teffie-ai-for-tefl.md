---
title: "Teffie: a portable AI chatbot for foreign language teachers"
header:
  teaser: "/assets/images/..."
categories: 
  - Software
tags:
  - software
  - projects
  - teaching
  - ml
  - ai
---

Summary and analysis of Teffie, a portable, 100% open-source generative AI teaching assistant for the foreign language classroom.

This is meant to be a longer, detailed writeup broken up into smaller, more easily digestible chunks. Jump to a desired section using the table of contents below.

## Table of contents

For a five-minute executive summary, [jump here](#executive-summary).

For a discussion on motivation and previous work, [jump here](#background-and-motivations).

For a teaching-centric discussion on usage of generative AI in the classroom, [jump here](#for-teachers).

For an engineering-centric discussion on Teffie's current state and future roadmap, [jump here](#for-programmers).

## Executive summary

Teffie is a **100% open-source** vocal AI agent for usage in foreign language classrooms. The current product, when prompted, will listen for and transcribe spoken English audio from a speaker, generate a response, and send it back to output. This can be used as a partner for speaking or listening exercises, particularly when students are engaging in freer practice.

Most generative AI products have a pay-per-month business model dependent on the user's usage rate, commonly known as "software as a service" (SaaS). Considering what options AI-centric companies have to make money, this is unlikely to change. However, teachers trying to use generative AI will be restricted by this business model on how often they can use AI and how many agents they can use in the classroom. This is particularly in TEFL classrooms in countries with lower purchasing power than the United States (where most AI products are developed).

To address this problem, Teffie is **serverless and entirely local**, meaning it is designed to run quickly and produce satisfactory results on hardware that isn't specialized for AI, i.e. laptops and desktop computers with weak GPU resources. On a laptop with a 3050 Mobile GPU (4 GB of VRAM), the current prototype takes 5 seconds to run from the beginning of transcription to the start of audio output. This matches the hardware on more modern laptops and some weaker / older builds for desktop computers[^0].

[^0]: iOS devices do not have dedicated VRAM; graphics and CPU use the same memory. To my understanding, the division on Android devices varies depending on the brand. Newer devices tend to have more memory than older ones; a brief exploration of device offerings suggests anywhere between 4-12 GB of RAM.

Teffie is in a prototype state; the two most pressing needs are friendlier UI / UX and improvements to text and speech generation. I have successfully used generatve AI in the classroom and so have other foreign language teachers, but based on short experiments in some of my classes, Teffie has at least months to go (with a single developer) to reach an MVP state that is easily usable by the average foreign language teacher. 

If you are interested in contributing or providing critique, please reach out! I am currently the only developer on Teffie, and since Teffie is in the early stages and since this is my first open-source project, I am open to discussions on scoping, design, and tech stack. [The source is available on GitHub here](https://github.com/michigan-musicer/teffie) and my email is kevin \[at\] musicer-kw.com. 

[Back to table of contents.](#table-of-contents)

## Background and motivations

I am a current scholar in the U.S. Fulbright Student Program in Poland, where I teach English and computer science to college students in rural Poland. I developed Teffie to explore generative AI options in my own classrooms. Teffie's name is a play on TEFL, the standard abbreviation for "teaching English as a foreign language".

My original inspiration is an online entertainer named DougDoug ([YouTube](https://www.youtube.com/@DougDoug), [Twitch](https://www.twitch.tv/dougdoug)). One common way DougDoug structures his streams is to create an AI application that reads from Twitch chat[^1] and, based on that, generate various responses for entertainment. This aspect of DougDoug's content is fundamentally different from other Twitch content because in addition to audience-streamer interactions, the audience and streamer can both interact with the AI agent for more varied content. DougDoug and the creativity of his AI-supplemented content were recognized by the online entertainment industry in the 2023 Streamer Awards with the "League of Their Own" award.

[^1]: for the uninitiated, Twitch is a livestreaming platform with a chat feature that allows audience members to comment and react live to what is going on in the stream.

A few example videos are shown below:

<iframe class="invidious-container" src="https://www.youtube-nocookie.com/embed/zLAn7Qp69yA" title="Invidious video player" frameborder="0" allowfullscreen></iframe>{: .align-center}

<iframe class="invidious-container" src="https://www.youtube-nocookie.com/embed/2fWsaEfQjZc" title="Invidious video player" frameborder="0" allowfullscreen></iframe>{: .align-center}

DougDoug's content is proof that generative models provide novel options for social interaction, particularly when there is an activity leader (e.g. an entertainer or teacher) and an audience (e.g. Twitch chat or a class of students). I do not recommend directly using DougDoug's content in the classroom, but the variety of interactions that makes DougDoug's content interesting to online audiences can similarly be used to raise student engagement in course content. Worth noting is that LLMs (large language models) have problems with accuracy of generated responses, which precludes their usage in many STEM and social science fields. However, LLMs *can* generate stories, answer questions, and simulate human conversation ad-hoc in natural languages. As such, in education, **LLMs are uniquely ideal for the foreign language classroom.**

In my view, there are two primary points worth addressing:
- Teachers want successful lesson plans and other positive results to be convinced to try generative AI themselves.
- Teachers want a product that is as technically simple as possible - ideally something that "works out of the box".

Regarding the first point: using generative AI to entertain a large online audience has some fundamental differences from using it to make students practice a foreign language. Unlike some other subjects, students learn the most in the foreign language classroom if they are actively practicing and attempting to use the language. TEFL teachers, for instance, are taught to minimize the amount of time they spend talking[^2] and maximize the amount of time students spend talking. Pointing to an online entertainer and saying "it works there!" should not be convincing by itself, because content in entertainment typically focuses on the entertainer. 

[^2]: This is drilled so deeply into TEFL pedagogy that we have a name for it - TTT (teacher talk time).

To establish that there is a basis for Teffie's existence, I have successfully used ChatGPT in lessons to raise student engagement and to make students practice vocabulary that they wouldn't normally use. I have created other lessons used with Teffie that have been hit or miss depending on the quality of generated responses; I believe that if Teffie were developed, said lesson plans would be engaging and successful. If you're curious for more on the teaching aspect specifically, [jump down here](#for-teachers).

Regarding the second point: I believe the most technically simple solution should be as local as possible. In other words, as much of the work should be done on the user's computer as opposed to remotely on a server (i.e. some other computer somewhere on the internet, or in the "cloud" as software people like to call it). A user may need the internet to install the product and download models, but that should be the extent of it; there should be no back-and-forth with a server when generating responses. From what I've seen[^3], there's a lack of generative AI projects that run entirely locally. There are many, many chatbot projects out in the wild, but the vast majority of the ones on GitHub provide server code and client code. Others utilize existing APIs that require subscription to a paid / freemium service. 

[^3]: There's always a possiblity that I missed something, especially with how fast-paced the ML space is. 

To be clear, a cheap, easily usable SaaS-based solution *is* possible, and paid solutions are likely going to produce higher quality responses than software running entirely on a user's computer. That said, I will point out three longer-term concerns: 

- Will such a solution be easy for teachers to set up and use? Most teachers will not have the training to set up their own server-client architecture, which rules out most of the smaller projects that already attempt to do this. 
- If a project uses APIs, how reliable is the API performance? Considering how popular generative AI has become, I think it's reasonable to worry about occasional performance degradation[^4]. Relying on server performance also disadvantages users that are geographically far away from those servers - particularly parts of the world where TEFL teachers would benefit more from something like Teffie. 
	- This similarly calls into question the scalability of open-source client-server products. For my blog, I use an open-source solution for comments called Staticman, which used to be hosted on a publicly accessible endpoint. However, the maintainer had to shut down that server because demand outpaced Patreon contributions, making it financially unfeasible to keep an open endpoint for everyone to use. This also ignores security problems like DDoS attacks. 
- How easily understandable is the API pricing? 
	- Most APIs that provide direct access to a model[^5] have pay-as-you-go business models, where users pay some amount of money to generate some number of tokens (usually phrased as some amount of money per 1 million tokens). These are very affordable by themselves; however, it would be unclear to inexperienced non-technical users how much they'll actually pay[^6], and it would require the user to set up API keys with whatever products are in use. It also means any new user also has to get an API key, which could become an obstacle to wider adaptation of the technology. 
	- Companies selling AI products at an organizational level are usually selling at a flat rate per user per month; however, these are usually priced far higher than direct usage of APIs[^7]. I can't say how good or bad this is from the perspective of an education administrator; personally speaking, I would not pay $30 a month to add generative AI to my classes.

[^4]: You can find threads on OpenAI's help forum complaining about significant downtime for API users. To be fair, it doesn't seem to be a constant problem.

[^5]: technically a server hosting that model. See [OpenAI](https://openai.com/pricing), [Mistral](https://mistral.ai/technology/) for examples.

[^6]: Back in college, I did a team project that required setting up MLIR, a project within the LLVM compiler ecosystem. We couldn't set it up on our class-dedicated server, so I took the lead and decided we would set it up on Google Cloud. Google Cloud gives you some amount of free credits when you link a credit card, more than enough for our project; however, there was still a lot of concern and pushback from my teammates that we'd somehow end up getting charged lots of money, and I had to do some convincing to explain why we'd be okay. The moral of the story here is that **communicating about money and payment is hard**, and you shouldn't underestimate how much users can be dissuaded by pay-as-you-go plans in particular (even if in 99% of use cases for a specific domain, it is objectively cheaper than a flat rate per month). Those teammates were great, by the way! I just didn't realize the credit card thing would be a concern. 

[^7]: For example, ChatGPT's "Team" tier and Microsoft Copilot are both $30 per user per month.

Teachers are not wealthy. Neither are most schools or other education organizations. The lack of strong, accessible, and truly cost-free options will likely dissuade teachers from trying out vocal chatbots like Teffie. Even if the performance of a free product is lower than that of paid solutions, using something for free could greatly facilitate the usage of generative AI in the classroom and open up opportunities for teachers to try more creative lesson plans. 

With all this in mind, I believe Teffie's emphasis on portability adds a level of user accessibility that is inherently absent from non-local code. Most of the technical concerns come from having to fit performant models on very restricted hardware while maintaining high throughput. If you are interested in that technical aspect, jump down to [the engineering section here](#for-programmers).

[Back to table of contents.](#table-of-contents)

## For teachers

On LLMs in general and what to expect, my experiments with using Teffie, and suggestions about lesson plans with Teffie. 

### How does generative AI work?

When it comes to communication, the cardinal sin of engineers is to assume the other person already understands everything. This is amplified when engineers are explaining something to a non-technical audience. I will do my best to summarize, but please feel free to leave a comment if something is unclear. 

Nowadays, "AI" usually refers to a machine learning model, where "model" refers to what is essentially a lot of numbers[^8]. Machine learning works by collecting data, then making a program try to replicate that data in some way. The program will repeatedly adjust the numbers for the model as it makes mistakes and "learns", and the end result of that program is basically a final setting for those numbers. Users can "use" a model by giving it an input, which is then converted into numbers and multiplied by all the numbers in the model to create an output. 

[^8]: If you remember matrix math from high school or college, the numbers in models are stored in matrices. Linear algebra is the relevant mathematical subject here, and it's a must-have for serious machine learning practitioners. 

Large language models (LLMs) are a particular kind of machine learning model that rely on recent innovations in AI model setup to achieve high performance. LLMs fall under a subfield of AI called [*natural language processing*](https://en.wikipedia.org/wiki/Natural_language_processing), which is the science of getting machines to "understand" natural (i.e. human) languages. LLMs are most well-known for their usage in text generation. ChatGPT is the prototypical example here, but there are other models out there - Teffie uses a model from Microsoft called `phi`.

Machine learning models for other tasks exist. Speech to text (also known as automatic speech recognition) takes in human speech and transcribes it, while text-to-speech takes text and outputs human-like speech. Teffie uses one speech to text model, one text generation model, and one text to speech model. 

When you are using Teffie, your data is not saved or used by Teffie in any way. Teffie just uses the models; it does not send data back to Microsoft or anyone else for use in future model training. The tricky thing is that other AI products *do* use user data - [see this help article from OpenAI on how ChatGPT uses your data to improve performance](https://help.openai.com/en/articles/5722486-how-your-data-is-used-to-improve-model-performance). From my experience, it's hard to tell when an AI product trains on user data or not; if you are concerned about a particular AI product, it's best to look this up.

### Existing resources for educators

There is already some content online on using generative AI as an educator. UPenn's Wharton Business School has a crash course on [generative AI for education](https://www.youtube.com/watch?v=t9gmyvf7JYo&list=PL0EdWFC9ZZrUAirFa2amE4Hg05KqCWhoq&ab_channel=WhartonSchool). This series targets a general pedagocical audience and, at a glance, doesn't focus too much on using AI directly in the classroom. Christina Cavage from Immerse also provides some [TEFL lesson plan formats employing generative AI](https://www.linkedin.com/feed/update/urn:li:activity:7188577152210976768/). In general, however, most resources on generative AI for education are to make materials for courses and individual lessons, *not* using generative AI directly in the lesson like Teffie is intended to be.  

If you are a TEFL teacher and know more about language learning research about settings with multiple teachers / third-party actors, please let me know!

### Activities with generative AI 

I learned to teach from a [TEFL certification course called CELTA](https://www.cambridgeenglish.org/teaching-english/teaching-qualifications/celta/), which is considered the standard qualification for TEFL teachers. If you have a different background, the general lesson plan structure taught in CELTA is as follows. (There are of course other perfectly fine ways to structure your lesson; this is just the template I usually use.)

- lead-in to get students interested in the lesson topic
- pre-teach need-to-know vocabulary / grammar
- introduce reading or listening material
- gist task (short skim)
- specific information / details task (deeper immersion with material)
- freer practice (conversation using newly learned information)

When I use generative AI, I generally fit it into the freer practice portion of the lesson. At least if you're sticking close to this template, AI in other parts of the lesson would start to replace student time rather than supplement student time, and lessons should be as student-centric as possible.

For my first lesson with AI, I used ChatGPT in a Santa roleplay; students had a normal freer practice session talking about their Christmas wishlists, then asked ChatGPT for something on said wishlist. I was the one inputting prompts into the ChatGPT web interface, and I would ask students to put Santa in creative situations[^9] and then narrate Santa's response. This was successful enough that some of the other experienced teachers at my school borrowed it for their lessons.

[^9]: Examples include pirate Santa, Santa running from a forest fire, Santa after he is bitten by a zombie and about to turn...if you're doing something like this, it definitely helps to be comfortable with your students.

A different activity that didn't go so well had students and teachers on different teams racing to take over the US with diplomacy. The idea was that ChatGPT would act as the governors of US states and each team would have a representative attempt to negotiate with governors to join their team. (DougDoug watchers will recognize this idea from some of his videos; the results here made me realize that directly copying online entertainment in the classroom is a bad idea.) The idea isn't bad by itself, but it's just too complicated of an activity to fit in the last 10-20 minutes of a 90 minute lesson, particularly for students who know very little about US states. Students were confused what they were supposed to do, and we didn't get anywhere.

After creating the Teffie prototype, I came up with two more freer practice activities using it: 

- In a speaking-centric lesson about improv acting, students would ask Teffie to make me do something[^10] (e.g. sing my favorite song, act like an Instagram influencer, act like one of the vampires from Twilight...). Based on what students said, Teffie would then generate a more fleshed-out scenario that I would subsequently act out. This one went very well - students were coming up with lots of creative requests, and Teffie fleshed out some of the prompts reasonably (e.g. "sing a song" turned into "Kevin was very scared to sing in front of an audience, but his teacher Ms. Sanders encouraged him to take the stage", which I subsequently interpreted). I'm wondering if it would have gone as well if students were the actors.
- In a speaking-centric lesson about meeting new people, students would introduce themselves to Teffie and Teffie would greet them back and ask a question. This one is fundamentally flawed because you don't *do* anything with the AI response. You could try to have a conversation with it, but then students are just talking 1-1 with an LLM, which is a bit awkward by itself[^11]. Teffie also struggled more with the prompting on this one; it often attempted to turn the student's greeting into a story.

[^10]: With a more talkative group, you could popcorn and have the students nominate each other to take turns doing improv. However, the group I did this with was a little more introverted, and I figured it would be a bad idea to force these particular students to do improv in front of their peers.

[^11]: Teffie also doesn't track conversation history and will not remember what you just said to it, unlike ChatGPT. Adding this feature requires some more technical work, which I don't thiiiiiink is too hard? Not sure if it's a priority compared to other critical story points. 

In general, from my experience, generative AI is a beneficial and easy addition to freer practice activities if it serves as some sort of intermediary. If a student says something that generative AI interprets that then goes on to the teacher or another student, then it introduces new content that the teacher or (ideally) student has to react to. Generative AI is detrimental when it occupies too much space; if you use generative AI for the sake of using generative AI, expect students to get lost and for the lesson to miss out on a lot of interactivity.

it should be noted that I am a native English speaker who doesn't understand the local language; this means students have to communicate with me in English, forcing them to practice. Often, if students are struggling and can communicate with the teacher in their native language, they will try to do so, circumventing the learning experience. Teffie and other generative AI solutions can get around this because they will only understand English[^12]; if you set up activities using Teffie or similar products, then students are forced to use and learn the foreign language.

[^12]: This is a simplification; in actuality, there are models that have been trained on a wide range of non-English languages. ChatGPT's web interface can respond in other languages without any additional work; I'm not sure if this is true for generative AI models generally or if ChatGPT is just coded that way. Whisper runs language detection before transcription, but Teffie is set up so that it skips the language detection part and just assumes English.

Not every lesson should contain Teffie. Even if the generated responses improve to ChatGPT levels (which is extremely unlikely), traditional student-student and teacher-student interactions will still be the most beneficial for practicing English in realistic scenarios. Additionally, responses generated by LLMs will likely be too advanced for beginner and beginner-intermediate audiences; I had acceptable performance with students at the B2 level and above, but B1 and below may struggle to understand generative AI responses at all. In short, Teffie should be used occasionally as a supplement to freer practice with students who are capable of understanding it - it should not replace the teacher entirely.

[Back to table of contents.](#table-of-contents)

## For programmers

On the current state of the code, todos, and possible additions.

### Technical requirements

- Maintain near-live performance - ideally, keep time between the start of input transcription and the start of audio output within a 10 second time limit. Any longer than this makes it difficult to maintain students' attention.
- Remain entirely local (within reasonable memory / video memory limits) so that users can run the software with as little technical complication as possible.
- Avoid interfacing with paid / freemium APIs so that users never have to worry about having to pay - Teffie is intended to be free for use in the classroom, forever! 

I emphasize again: Teffie is *not* going to maximize the quality of generated text or audio. If you have the financial means and the technical know-how, then in 99% of cases, the correct approach should be to use APIs from commercial vendors for your ML apps. If you are as restricted on hardware as Teffie is meant to be, then there is no way that you will compete with models with 300 billion parameters running on GPU-laden servers that are dedicated to executing ML applications. 

### Current state

Teffie is in a prototype state using out-of-the-box models from HuggingFace. It is implemented as a command-line application; the chatbot runs forever in a loop that goes audio input → audio transcription → text generation → text to speech generation → audio output. 

At the start of the loop, the user is prompted to hit ENTER to start listening for input; this goes until either 30 seconds pass or ENTER is hit again (whichever comes first). After that, a Whisper `base-en` model transcribes the speech, which is then sent as input into a `phi-1.5` text generation model in HuggingFace's `AutoModelForCausalLM` mode. The output is then sent to an `MMS` model that produces corresponding output audio, which is then sent to output to play[^13]. Once the audio finishes playing, the user is prompted again as the loop repeats from the top. 

[^13]: No intermediate files are created, which I note because I've seen some example code that creates a file. I assume it's faster to skip the OS-level work and just pass the audio waveform in directly as a `numpy` array but there could be something I'm missing.

Logging is included throughout the program; detailed debug logging can be disabled by setting the `DEBUG` variable.

### Things to work on

- The vast majority of TEFL teachers will not be running Linux but rather Windows or MacOS. There would be a lot of work needed to improve user experience to levels acceptable for teachers, e.g. UI, installation / program setup, selection of audio hardware[^14]...
	- I am not familiar with the process of releasing apps on Windows or MacOS, so I'm not sure how difficult the platform engineering would be. To my understanding, however, Python takes much less work to port than C++ or other lower-level languages.
- **Fine-tuning is a must**. I did not pursue this for the prototype because it was more important to demonstrate a valid proof-of-concept. However, the quality of generated text and speech needs to improve drastically to really consider Teffie "conversational". This is less conceptually challenging and moreso something that will take time and trial-and-error to figure out.
    - Whisper out-of-the-box struggles significantly with accurately capturing accented English. That is, English learners with an accent will often be mistranscribed by the model. This doesn't be fully corrected; learners will make pronunciation mistakes, and the model shouldn't necessarily behave as if they didn't.
    - Phi-1.5 is trained on textbooks and has a tendency to fixate on specific topics - soccer games, a classroom setting, a girl named Lily...Microsoft also emphasizes that there was no pretraining done to avoid offensive responses, which is problematic - you don't want something hateful coming out of your teaching assistant in a classroom setting. Good behavior is very difficult to enforce via fine-tuning, so this may be something to handle with more rudimentary filtering (e.g. censoring tokens in output that match to a list of forbidden words).
	- MMS just isn't good enough. The cadence of the voice is too unnatural for students in the TEFL classroom to understand, and they shouldn't - no real human talks like MMS. I would like to switch to a distilled and/or quantized version of Bark going forward; the main concern is keeping Bark inference under that 10 second limit.
- [Post-training quantization](https://huggingface.co/docs/optimum/en/concept_guides/quantization) reduces model complexity by converting floating point weights to simpler `long` or `int` types. This can reduce model size and computation time. This can be used to make Teffie fit on even smaller hardware or to utilize larger, more effective models without breaking the VRAM limit. I'm currently using quantization to reduce the size of Phi-1.5, but this was something thrown in ad-hoc to fit Phi-1.5 into the limit - not something carefully chosen.
- Some stories regarding usability and QoL, such as handling edge case user behavior and using a config file instead of hard-coding prompts and other information.
- I get CUDA-related glitches on my platform (an Ubuntu 22.04 install) that stop the program entirely; however, I haven't set up the code to properly capture and log those glitches, and I suspect they may have to do with issues on my OS[^15] or proprietary NVIDIA drivers that I can't easily solve.

[^14]: `sounddevice` automatically detects the default input and output audio devices, but for various reasons a user may want to change which devices Teffie uses. 

[^15]: I'm running NVIDIA drivers on Ubuntu 22.04, which from my experience can be a hassle compared to the Windows experience.

### Possible additional features

- There are other methods to reduce model size, e.g. [LLM shearing](https://github.com/princeton-nlp/LLM-Shearing) by Xia et al, that might be worth exploring.
- Instead of hardcoding to specific models on a specific VRAM limit, we could detect the VRAM capacity of a device's hardware and to plug in a selection of models for specific VRAM capacities. For example, pick one set of models if you have 4 GB of VRAM or less to work with, a more powerful set of models for 8 GB, etcetera. 
	- This could also include a selection of models for hardware that only supports integrated graphics, which is common for older and/or lower-end computers. Integrated graphics uses RAM instead of storing models in dedicated VRAM, which necessitates usage of even smaller and faster models than the 4 GB VRAM case.
- Depending on the architecture of Whisper, it maybe possible to create a fork and use a submodule of a Whisper variant that doesn't hardcode sample rate and duration or that hardcodes them to smaller values that can better fit on weaker hardware. If this is possible, this may help reduce computation time. (Without having read the paper or code in depth, my guess is that this would require changing the weights and in turn retraining the model; however, distillation may offer a workaround for this.)
	- You could make similar arguments about all models; I highlight Whisper because most audio input will be shorter than 30 seconds. This would be most useful in a fully live setting such as the one outlined below; otherwise, the time saving is minimal because you're only transcribing once per loop iteration.
- There are .cpp implementations of many models that, since they are compiled, take considerably less space and run signifcantly faster; I haven't experimented with these myself. Note that per the second bullet point in this section, I don't necessarily want to attach myself to a strict set of models. 

[Back to table of contents.](#table-of-contents)

## Personal notes

A few thoughts that either aren't angled towards audiences with a particular skillset or are related, but weren't essential details.

### Thinking-out-loud bullet points

- I am uncomfortable talking about a project that is in a prototype state, even though I know it's best to get feedback and publicity as quickly as possible. Part of it is perfectionism (something I'm working on), part of it is that there's some quick and dirty things I could do (at least in theory they're quick and dirty), and part of it is what I think is a legitimate concern that if you publicize too early, people will get impatient waiting for the project to reach a "final" state and will stop caring. But is that much worse than waiting, building something all the way, and only then realizing that people didn't care in the first place?
- As a software engineer, I want to work on problems that people care about. Working on this project and this writeup in particular has made me realize that I have to do a lot of work to convince people that my work matters - it's not just a matter of learning what people *already* care about and following what everyone else is doing. It's nice to feel like you can target a previously unseen need, and I think this is a good takeaway regardless of how much free time I spend in the open source community.
- Open source products are only truly meaningful if they are maintained. Will this be maintained? I think it's interesting to have a projects that cares a lot about portable architecture, but I am not great at selling to people and I tend to not fight for my ideas hard enough. I will publicize this a little further 
- Moreover, I'm not sure how much time I will have or how much I want to prioritize Teffie. It's job application season, and there's other technologies I want to learn in more systems-adjacent spaces. I think I can learn a lot with steady progress, but if I want to utilize those learnings in the job application cycle, the pivot probably needs to happen now. Doesn't stop me from running fine-tuning epochs on my computer overnight...heh. Feels like grad school again when I say that.
- I'm biased towards building something new, which is almost certainly because I'm at a junior level. I really like building things from scratch and reasoning about things at the lowest level possible, which honestly makes using HuggingFace and ML libraries a little uncomfortable to some degree. I am self-aware enough to understand that rewriting everything and doing "new" things all the time is not a good idea for business or for having a super nice, easily maintainable codebase. Code debt that doesn't show up for 18 months is a heck of a thing.
- I am also biased towards doing something local and believing that there is value in a local application because that's the philosophy behind games, where I did an internship in 2022. Because you need to be within hardware limits on consoles, games are one of the few remaining problem spaces in tech where you can't solve business-critical problems by just buying more cloud compute. (Saying that that's a "solution" by itself is an exaggeration, but a lot of problems do become a lot easier with additional compute.) 
- A big part of me is hoping that what I'm writing is careful and measured enough to not make me seem like an AI influencer. I think a lot of software professionals (and normal people) roll their eyes when they hear about the umpteenth AI startup, and in my opinion, rightfully so - a lot of people are applying AI in places that don't seem to really need it. Since I have had success with it myself and since there are education professionals with an eye on the technology themselves, I would like to think that I am applying AI somewhere with real potential. 
- Other stuff I'm not thinking of right now...

### Consideration of a multithreaded live architecture

(This is from a draft of the writeup and might be a little disjointed.)

The current code runs sequentially (minus keyboard interrupts and usage of callbacks in processing and producing audio input / output). However, I originally envisioned a live chatbot that would continuously listen transcribe, produce responses, and output audio throughout the course of an entire lesson. This doesn't necessarily make sense from an end-user standpoint, but I want to document the progress I made and the issues I encountered in case this ever comes up for consideration.

The main difference is in how you handle I/O; in a live app, you need to be continously reading and writing from and to audio input / output. It doesn't make sense to transcribe and respond to every frame or every 100 frames of audio input, so transcription needs to be done in chunks of, say, 5 seconds. (This is a hyperparameter you'd need to mess around with.) Additionally, you likely need some additional logic to make sure you are responding to complete sentences, since it wouldn't make sense to create two replies to a sentence that is 8 seconds long (i.e. 2 or 3 chunks).

There's a few different ways to split up tasks. One is audio input / transcription / text generation / speech generation / audio output; basically, each piece of the chatbot pipeline receives its own thread, and each consecutive pair of threads has a producer-consumer relationship. This is the architecture I originally attempted to implement. In retrospect, I think you can get away with just an audio input thread, a response generation thread, and an audio output thread. Again, the most important parts of the pipeline to isolate are the I/O components; in a performant multithreaded application, the largest model will likely be the main performance bottleneck, so there's not much point in splitting up the work within that response generation component[^30]. 

[^30]: I think the more general lesson to take away from this is that if you have a producer-consumer relationship where the producer is the only one supplying the consumer and the consumer is the only one receiving from the producer, that's a big code smell indicating you really don't need them to be separated into threads. I can see a counterexample where the producer runs in, say, 1/10th the time of the consumer and you need to track every produced item (probably on a queue or similar), but in the case of a chatbot, this doesn't make sense - you shouldn't try to generate a separate response to something that was said before you started saying a response back.

One thing that sticks in my head is the thought that a live app could react more realistically to things that happen while a response is being spoken. For instance, if the model is currently playing a response in audio output but is able to detect a speaker interrupting said output with new input, then it could stop the current audio output to then generate a response to the interrupt. Figuring out whether a response "should" be interrupted, however, might be a whole research project by itself[^31]. Note that what we really need here is concurrency between audio input, transcription, and audio output - we don't need concurrency between the response's text and audio generation.

[^31]: I have not scoured the literature for existing solutions to this problem. If anyone knows of a relevant paper here, feel free to send it to me! That said, an easy "should" is just that if someone else speaks at all, you interrupt - from my experience, that's what most people do in regular conversation.

At the end of the day, this live functionality isn't necessarily beneficial. A live bot may require less non-verbal interaction to maintain back-and-forth conversation with an individual speaker, but without additional logic to filter out what a chatbot "shouldn't" be listening to (which itself requires some thinking about how human conversation works), I imagine it would be a fair amount of hassle for listeners to keep track of where the chatbot is in the conversation. Additionally, people in natural human speech usually take turns back and forth speaking, so having a chatbot that queues and attempts to respond to all audio input would not provide a realistic experience. I think this may be an example of me choosing a solution because I want to use a tool (threading / concurrency) that isn't actually needed for the task at hand (TEFL assistant with portable models and quick inference).

Threading may not even be the best solution to implementing a live app; Python's `asyncio` library provides concurrency within a thread via coroutines and an event system. (This is in fact what `sounddevice` suggests for threads that need to do work while processing audio I/O.) In any case, I eventually decided to pivot because A. I was getting stuck on what specific tools to use, and B. I realized I was wasting a lot of time thinking about features that weren't critical to the success of the final product. There's probably more notes I have in my head on using Python's `asyncio` and `threading` libraries, but those are mostly dealing with the language toolkit and not necessarily meaningful thoughts on the architecture or possible features enabled by said architecture.

### Older draft dump on portable architecture hardware

(old bits that may be repeating stuff from above)

As mentioned before, teachers do not have a lot of resources. It follows that teachers are unlikely to be buying hardware that is usually used to train, deploy, and run state-of-the-art models. No GPU clusters, no TPUs - heck, many instructors may not even have access to a computer with a dedicated graphics card and may be stuck running integrated graphics. (I imagine many a graduate student forgot to download proprietary drivers and spent orders of magnitude more time training a model than they needed! Or maybe I'm just dumb hehe.)

One way to solve this is to create a server with sufficient memory and compute, host models and perform computaiton there, then serve the AI tools on a front-end that teachers can access on the web. Indeed, if you go to GitHub and search for, say, live transcription by Whisper, this is in line with how most solutions will look. However, from what I've seen, it is not sustainable to host a server for open-source technology like this. If an idea / product takes off hard enough, then there will be too many users to reasonably host a server without requiring compensation. (My experience came from Staticman, a service I'm using to store comments on my static website. Originally there was one single Staticman server everyone used; however, there got to be too many users for the author to be able to justify hosting that endpoint. It didn't help that Heroku has gotten rid of their free tier service.) Most counterexamples to this have commercial sponsors or are provided directly by a commerical entity, and I am uncertain whether a project like Teffie would receive the same kind of attention[^32].

[^32]: You could argue that companies could be interested in high-performance machine learning models on weaker architecture. I'm just not sure what market this targets because businesses (the biggest consumers of AI products) can usually buy additional resources, which is going to be easier than developing more efficient solutions.

Again: teachers will want to pay monthly subscription fees for additional technology. I don't believe monetization is evil or that it can't help a product develop, but I believe it's important to at least decouple Teffie from freemium AIaaS services. With that in mind, **Teffie must be designed to be portable.** Portable means that a teacher can run Teffie on their laptop without running into CUDA out-of-memory errors.

The main argument against a portable architecture is that most teachers will have hardware too weak to load and run state-of-the-art machine learning models, but on the other hand will have a strong enough internet connection to connect to a server hosting a model. . Additionally, there is always the possibility that AIaaS vendors will paywall their free tier services[^33]. These frequently used, high-performance endpoints cost a lot of resources to develop and maintain, and if a customer can't get the same quality of product elsewhere, then a company can always pressure them to upgrade by pushing down the quality / quantity of free service.

[^33]: For example, as of 21 April 2024, Elevenlabs already limits free tier users to 10,000 characters per month, which is about 10 minutes of audio. An individual lesson typically lasts 1.5 hours, so this limit would significantly restrict possibilities.  

[Back to table of contents.](#table-of-contents)
