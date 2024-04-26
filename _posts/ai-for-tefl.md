
Summary and analysis of Teffie, a portable, 100% open-source genAI teaching assistant.

---

## Table of contents

For a five-minute executive summary, [jump here].

For a discussion on motivation and previous work, [jump here].

For a teaching-centric discussion on usage of generative AI in the classroom, [jump here].

For an engineering-centric discussion on Teffie's current state and future roadmap, [jump here].

## Executive summary

Teffie is a **100% open-source** vocal AI agent for usage in foreign language classrooms. The current product, when prompted, will listen for and transcribe spoken English audio from a speaker, generate a response, and send it back to output. This can be used as a partner for speaking or listening exercises, particularly when students are engaging in freer practice.

Most generative AI products have a pay-per-month business model dependent on the user's usage rate, commonly known as "software as a service" (SaaS). Considering what options AI-centric companies have to make money, this is unlikely to change. However, teachers trying to use generative AI will be restricted by this business model on how often they can use AI and how many agents they can use in the classroom. This is particularly in TEFL classrooms in countries with lower purchasing power than the United States (where most AI products are developed).

To address this problem, Teffie is **serverless and entirely local**, meaning it is designed to run quickly and produce satisfactory results on hardware that isn't specialized for AI, i.e. laptops and desktop computers with weak GPU resources. On a laptop with a 3050 Mobile GPU (4 GB of VRAM), the current prototype takes 5 seconds to run from the beginning of transcription to the start of audio output. This matches the hardware on more modern laptops and some weaker / older builds for desktop computers[^0].

[^0]: iOS devices do not have dedicated VRAM; graphics and CPU use the same memory. To my understanding, the division on Android devices varies depending on the brand. Newer devices tend to have more memory than older ones; a brief exploration of device offerings suggests anywhere between 4-12 GB of RAM.

Teffie is in a prototype state; the two most pressing needs are friendlier UI / UX and improvements to text and speech generation. I have successfully used generatve AI in the classroom and so have other foreign language teachers, but based on short experiments in some of my classes, Teffie has at least months to go (with a single developer) to reach an MVP state that is easily usable by the average foreign language teacher. 

If you are interested in contributing or providing critique, please reach out! I am currently the only developer on Teffie, and since Teffie is in the early stages and since this is my first open-source project, I am open to discussions on scoping, design, and tech stack. [The source is available on GitHub here](https://github.com/michigan-musicer/teffie) and my email is kevin \[at\] musicer-kw.com. 

[Back to table of contents.](#table-of-contents)

<!-- [^0]: To my understanding, it will be significantly more difficult to provide high quality performance on mobile devices due to inferior hardware. In my experience, most classrooms will at least be equipped with a computer, so the project is angled towards maintaining performance on laptops and desktops. That said, availability on mobile could greatly benefit classrooms in regions of the world with less access to technology. -->


 <!-- . -->

<!-- For TEFL teachers, example lesson plans with such a chatbot are provided [below](#somewhere).

For software engineers, future goals of the software are as follows:-->

## Background and motivations

I am a current scholar in the U.S. Fulbright Student Program in Poland, where I teach English and computer science to college students in rural Poland. I developed Teffie to explore generative AI options in my own classrooms. Teffie's name is a play on TEFL, the standard abbreviation for "teaching English as a foreign language".

My original inspiration is an online entertainer named DougDoug ([YouTube](https://www.youtube.com/@DougDoug), [Twitch](https://www.twitch.tv/dougdoug)). One common way DougDoug structures his streams is to create an AI application that reads from Twitch chat[^0] and, based on that, generate various responses for entertainment. This aspect of DougDoug's content is fundamentally different from other Twitch content because in addition to audience-streamer interactions, the audience and streamer can both interact with the AI agent for more varied content. DougDoug and the creativity of his AI-supplemented content were recognized by the online entertainment industry in the 2023 Streamer Awards with the "League of Their Own" award.

[^0]: for the uninitiated, Twitch is a livestreaming platform with a chat feature that allows audience members to comment and react live to what is going on in the stream.

A few example videos are shown below:

(https://www.youtube.com/watch?v=zLAn7Qp69yA)

(https://www.youtube.com/watch?v=2fWsaEfQjZc)

DougDoug's content is proof that generative models provide novel options for social interaction, particularly when there is an activity leader (e.g. an entertainer or teacher) and an audience (e.g. Twitch chat or a class of students). I do not recommend directly using DougDoug's content in the classroom, but the variety of interactions that makes DougDoug's content interesting to online audiences can similarly be used to raise student engagement in course content. Worth noting is that LLMs (large language models) have problems with accuracy of generated responses, which precludes their usage in many STEM and social science fields. However, LLMs *can* generate stories, answer questions, and simulate human conversation ad-hoc in natural languages. As such, in education, **LLMs are uniquely ideal for the foreign language classroom.**

<!-- AI-supplemented activities can challenge students to solve non-traditional assignments.  -->

In my view, there are two primary points worth addressing:
- Teachers want successful lesson plans and other positive results to be convinced to try generative AI themselves.
- Teachers want a product that is as technically simple as possible - ideally something that "works out of the box".

Regarding the first point: using generative AI to entertain a large online audience has some fundamental differences from using it to make students practice a foreign language. Unlike some other subjects, students learn the most in the foreign language classroom if they are actively practicing and attempting to use the language. TEFL teachers, for instance, are taught to minimize the amount of time they spend talking[^0] and maximize the amount of time students spend talking. Pointing to an online entertainer and saying "it works there!" should not be convincing by itself, because content in entertainment typically focuses on the entertainer. 

[^0]: This is drilled so deeply into TEFL pedagogy that we have a name for it - TTT (teacher talk time).

To establish that there is a basis for Teffie's existence, I have successfully used ChatGPT in lessons to raise student engagement and to make students practice vocabulary that they wouldn't normally use. I have created other lessons used with Teffie that have been hit or miss depending on the quality of generated responses; I believe that if Teffie were developed, said lesson plans would be engaging and successful. If you're curious for more on the teaching aspect specifically, [jump down here](#for-teachers).

Regarding the second point: I believe the most technically simple solution should be as local[^0] as possible. In other words, as much of the work should be done on the user's computer as opposed to remotely on a server (i.e. some other computer somewhere on the internet, or in the "cloud" as software people like to call it). A user may need the internet to install the product and download models, but that should be the extent of it; there should be no back-and-forth with a server when generating responses. From what I've seen[^0], there's a lack of generative AI projects that run entirely locally. There are many, many chatbot projects out in the wild, but the vast majority of the ones on GitHub provide server code and client code. Others utilize existing APIs that require subscription to a paid / freemium service. 
<!-- it would using existing SaaS solutions in a vocal chatbot product (e.g. OpenAI STT/TTS, ElevenLabs, Speechify...) could severely limit . Consider a course that meets 90 minutes per session twice a week for four months - about 12 hours a month. If a single chatbot instance is used for 30 minutes a week, we can approximate the cost breakdown as follows:  -->
<!-- - 
- $22 / monht for Elevenlabs' creator tier.  -->

[^0]: There's always a possiblity that I missed something, especially with how fast-paced the ML space is. 

To be clear, a cheap, easily usable SaaS-based solution *is* possible, and paid solutions are likely going to produce higher quality responses than software running entirely on a user's computer. That said, I will point out three longer-term concerns: 

- Will such a solution be easy for teachers to set up and use? Most teachers will not have the training to set up their own server-client architecture, which rules out most of the smaller projects that already attempt to do this. 
- If a project uses APIs, how reliable is the API performance? Considering how popular generative AI has become, I think it's reasonable to worry about occasional performance degradation. Relying on server performance also disadvantages users that are geographically far away from those servers - particularly parts of the world where TEFL teachers would benefit more from something like Teffie. 
	- This similarly calls into question the scalability of open-source client-server products. For my blog, I use an open-source solution for comments called Staticman, which used to be hosted on a publicly accessible endpoint. However, the maintainer had to shut down that server because demand outpaced Patreon contributions, making it financially unfeasible to keep an open endpoint for everyone to use. This also ignores security problems like DDoS attacks. 
- How easily understandable is the API pricing? 
	- Most APIs that provide direct access to a model have pay-as-you-go business models[^0], where users pay some amount of money to generate some number of tokens (usually phrased as some amount of money per 1 million tokens). These are very affordable by themselves; however, it would be unclear to inexperienced non-technical users how much they'll actually pay[^0], and it would require the user to set up API keys with whatever products are in use. It also means any new user also has to get an API key, which could become an obstacle to wider adaptation of the technology. 
	- Companies selling AI products at an organizational level are usually selling at a flat rate per user per month; however, these are usually priced far higher than direct usage of APIs[^0]. I can't say how good or bad this is from the perspective of an education administrator; personally speaking, I would not pay $30 a month to add generative AI to my classes.

[^0]: You can find threads on OpenAI's help forum complaining about significant downtime for API users. To be fair, it doesn't seem to be a constant problem.

[^0]: technically a server hosting that model. See [OpenAI](https://openai.com/pricing), [Mistral](https://mistral.ai/technology/) for examples.

[^0]: Back in college, I did a team project that required setting up MLIR, a project within the LLVM compiler ecosystem. We couldn't set it up on our class-dedicated server, so I took the lead and decided we would set it up on Google Cloud. Google Cloud gives you some amount of free credits when you link a credit card, more than enough for our project; however, there was still a lot of concern and pushback from my teammates that we'd somehow end up getting charged lots of money, and I had to do some convincing to explain why we'd be okay. The moral of the story here is that **communicating about money and payment is hard**, and you shouldn't underestimate how much users can be dissuaded by pay-as-you-go plans in particular (even if in 99% of use cases for a specific domain, it is objectively cheaper than a flat rate per month). Those teammates were great, by the way! I just didn't realize the credit card thing would be a concern. 

[^0]: ChatGPT's "Team" tier and Microsoft Copilot are both $30 per user per month.

Teachers are not wealthy. Neither are most schools or other education organizations. The lack of strong, accessible, and truly cost-free options will likely dissuade teachers from trying out vocal chatbots like Teffie. Even if the performance of a free product is lower than that of paid solutions, using something for free could greatly facilitate the usage of generative AI in the classroom and open up opportunities for teachers to try more creative lesson plans. 

With all this in mind, I believe Teffie's emphasis on portability adds a level of user accessibility that is inherently absent from non-local code. Most of the technical concerns come from having to fit performant models on very restricted hardware while maintaining high throughput. If you are interested in that technical aspect, jump down to [the engineering section here](#for-programmers).
<!-- It would be great to take it even further and create something that can run locally on mobile devices -->


<!-- Teffie addresses these points by running . . . Teffie utilizes models that are completely open source (MIT or GNU license only **CHECK IF THIS IS TRUE**) and is designed to work immediately out of the box - no usage of paid products necessary

There is a plethora of open-source solutions to the individual subproblems Teffie has to solve (audio speech recognition, text generation, text-to-speech). Finding those solutions is not the main challenge; in my opinion, the main challenge becomes more obvious when considering what resources are at a typical teacher's disposal. -->


<!-- entire semester's worth of lessons. the typical model for businesses selling software as a service, or SaaS[^0], is to do something on demand (e.g. generate text, stream a film, host a virtual meeting...). In most cases, this means setting up a server (a computer accessible on [^0]) and having users make requests to that server to perform those tasks. For the business, the cost comes from users us. Teachers often don't receive enough resources from teaching administration, and teachers are usually underpaid relative to the work that they do[^0]. With these facts in mind, most teachers would be unwilling to pay, say, over $100 over the course of a semester for teaching tools - especially if they're teaching in a country where a monthly wage is in the hundreds of dollars.  -->

<!-- [^0]: You may also see AI-as-a-service (AIaaS) to specifically refer to SaaS products using AI.

[^0]: Something I learned while teaching in Poland is that there was a nationwide teachers' strike beginning in 2019 to protest extremely low wages [(English-language summary from *Notes from Poland* here)](https://notesfrompoland.com/2020/02/10/teachers-paid-less-than-discount-store-cashiers-in-poland/). From my experience, it is standard for professors and teachers at all levels to work multiple jobs to support themselves and their families. I can't speak to how things are for other countries.



[^0]: really, people who have the mic - not sure on exact wording here -->

<!-- Herein, however, lies a problem - DougDoug uses several paid solutions to implement AI in his content. Many companies are founded on a  business model - most notably OpenAI, but also vendors including ElevenLabs, Deepgram, and formerly Coqui AI, all of whom build products to address a wide range of problems in NLP[^0]. For DougDoug, it's probably worth it to pay for these solutions and not have to worry about all the messy details. However, based on my what I've heard from teaching in both the United States and Poland, teachers at all levels are often underpaid - meaning they are left without sufficient resources to sufficiently run their classes[^0] - and overworked, meaning that they often do not have the time or energy to explore new lesson plans. Teachers, therefore, are unlikely to want to pay commercial vendors for an AI tool in the classroom, and given the choice between not using AI in the classroom and paying tens or hundreds per month to use commercial AI products, almost all would take the former[^0]. -->

<!-- a better statement here is that fees based on usage place a limit on how much teachers can feasibly try to use AI in the classroom, which discourages innovation. If there was a product that cost a set amount per month to use, then great - but that kind of pricing model inherently goes against what makes sense for AIaaS because server maintenance and whatnot. -->

<!-- Additional problem is that there's a lack of lesson plans involving generative models. Teachers don't want to put in huge amounts of extra work lesson planning, so if you provide a fancy software tool and say "here you go, come up with something!", you will probably get a few creative lessons, but probably not enough to really explore and take advantage of the possibilities offered by the technology. -->
 

[^0]: There's a lot of XaaS abbreviations out there (let X = whatever the heck you want); can we agree this is one of the least roll-off-the-tongue ones out there? Too many vowels in a row. 

[^0]: It's pretty common to hear of teachers paying out of their own pockets for school supplies or other tools to use in the classroom.

[^0]: Natural language processing is the science of getting machines to "understand" human (i.e. natural) languages. Read more on [Wikipedia](https://en.wikipedia.org/wiki/Natural_language_processing) or in the [HuggingFace documentation](https://huggingface.co/learn/nlp-course/en/chapter1/2).

[^0]: This is a false dichotomy; a teacher can certainly go in between and use free trials or stay within the rate limit for free-tier service. However, keeping track of minutes / tokens used and managing all the different accounts for all the different services that one would use for an application like Teffie would be its own unwanted burden.




### Portable architecture

As mentioned before, teachers do not have a lot of resources. It follows that teachers are unlikely to be buying hardware that is usually used to train, deploy, and run state-of-the-art models. No GPU clusters, no TPUs - heck, many instructors may not even have access to a computer with a dedicated graphics card and may be stuck running integrated graphics. (I imagine many a graduate student forgot to download proprietary drivers and spent orders of magnitude more time training a model than they needed! Or maybe I'm just dumb hehe.)

One way to solve this is to create a server with sufficient memory and compute, host models and perform computaiton there, then serve the AI tools on a front-end that teachers can access on the web. Indeed, if you go to GitHub and search for, say, live transcription by Whisper, this is in line with how most solutions will look. However, from what I've seen, it is not sustainable to host a server for open-source technology like this. If an idea / product takes off hard enough, then there will be too many users to reasonably host a server without requiring compensation. (My experience came from Staticman, a service I'm using to store comments on my static website. Originally there was one single Staticman server everyone used; however, there got to be too many users for the author to be able to justify hosting that endpoint. It didn't help that Heroku has gotten rid of their free tier service.) Most counterexamples to this have commercial sponsors or are provided directly by a commerical entity, and I am uncertain whether a project like Teffie would receive the same kind of attention[^0].

[^0]: You could argue that companies could be interested in high-performance machine learning models on weaker architecture. This is .
  <!-- don't directly translate to business like other funded, well-maintained open source projects, e.g. Anaconda, Docker, Hadoop... -->

Again: teachers will want to pay monthly subscription fees for additional technology. I don't believe monetization is evil or that it can't help a product develop, but I believe it's important to at least decouple Teffie from freemium AIaaS services. With that in mind, **Teffie must be designed to be portable.** Portable means that a teacher can run Teffie on their laptop without running into CUDA out-of-memory errors.

The main argument against a portable architecture is that most teachers will have hardware too weak to load and run state-of-the-art machine learning models, but on the other hand will have a strong enough internet connection to connect to a server hosting a model. . Additionally, there is always the possibility that AIaaS vendors will paywall their free tier services[^0]. These frequently used, high-performance endpoints cost a lot of resources to develop and maintain, and if a customer can't get the same quality of product elsewhere, then a company can always pressure them to upgrade by pushing down the quality / quantity of free service.

[^0]: For example, as of 21 April 2024, Elevenlabs already limits free tier users to 10,000 characters per month, which is about 10 minutes of audio. An individual lesson typically lasts 1.5 hours, so this limit would significantly restrict possibilities. 

[Back to table of contents.](#table-of-contents)

# For teachers

On LLMs in general and what to expect, my experiments with using Teffie, and suggestions about lesson plans with Teffie. 

## How does generative AI work?

When it comes to communication, the cardinal sin of engineers is to assume the other person already understands everything. This is amplified when engineers are explaining something to a non-technical audience. I will do my best to summarize, but please feel free to leave a comment if something is unclear. 

Nowadays, "AI" usually refers to a machine learning model. Machine learning works by collecting data, then making a program (which is called our model[^0]) try to replicate that data in some way. 

[^0]: Some engineers will complain that the verbage here is inexact, which it is. Technically, the model is the result of the program; the program itself often contains a lot of other functionality that isn't used in the final model.

When you are using ChatGPT, Gmail's autocomplete feature, or a fun website that says things in your voice, you are using a "finished" version of that model. I believe that .

## Existing resources


There is already some content online on using generative AI as an educator. Wharton has a crash course on [generative AI in the classroom](https://www.youtube.com/watch?v=t9gmyvf7JYo&list=PL0EdWFC9ZZrUAirFa2amE4Hg05KqCWhoq&ab_channel=WhartonSchool) that targets a general pedagocical audience; this goes into basics and doesn't focus too much on using AI directly in the classroom. Christina Cavage from Immerse also provides some [TEFL lesson plan formats employing generative AI](https://www.linkedin.com/feed/update/urn:li:activity:7188577152210976768/). In general, however, most resources on generative AI for education are for , *not* using generative AI directly in the lesson as described.  

In terms of the technology out there: LLM stands for "large language model", which is a catch-all term for technology like ChatGPT[^0]. In tech, LLMs fall under a subfield of AI called [*natural language processing*](https://en.wikipedia.org/wiki/Natural_language_processing), which is the science of getting machines to "understand" natural (i.e. human) languages.  

[^0]: If you do some exploring, you will find other LLMs like Mistral, LLaMa 1/2/3, Gemini, Grok, Claude...The latest, best models are all founded on similar technology: transformer architecture usually in combination with reinforcement learning human feedback (RLHF).

<!-- [HuggingFace documentation](https://huggingface.co/learn/nlp-course/en/chapter1/2) -->

## Lesson plans and activities with generative AI

.

If .

Generative AI is not . For instance, Teffie would more or less be useless without the capacity to listen and speak. .

Not every lesson should contain Teffie, even when the product reaches a more advanced stage. Traditional student-student and teacher-student interactions will still be the most beneficial for practicing English in realistic scenarios. Additionally, responses generated by LLMs will likely be too advanced for beginner and beginner-intermediate audiences; I had acceptable performance with students at the B2 level and above, but B1 and below may struggle to understand the chatbot. In short, Teffie should be used occasionally as a supplement to freer practice with students who are capable of understanding it - it should not replace the teacher entirely.  

To my understanding, there is a lack of literature on the teaching model I've described in this section. If you are a TEFL teacher and know about pedagogy research into setups with multiple teacher / third-party actors, please let me know!


## Teffie trial runs



[Back to table of contents.](#table-of-contents)

# For programmers

On the current state of the code, todos and possible adds, and some notes on a more multithreaded approach that I tried at first.

## Technical requirements

- Maintain near-live performance - ideally, keep time between the start of input transcription and the start of audio output within a 10 second time limit. Any longer than this makes it difficult to maintain students' attention.
- Remain entirely local (within feasible VRAM limits) so that users can run the software without internet connection and without worrying about setting up their own servers.
- Avoid interfacing with paid / freemium APIs so that users never have to worry about having to pay - Teffie is intended to be free for use in the classroom, forever! 

I emphasize again: Teffie is *not* going to maximize the quality of generated text or audio. If you have the means and the technical know-how, then in 99% of cases, the correct approach should be to use APIs from commercial vendors for your ML apps. I do think there is something to be said about a 100% local, open source chatbot - unlike paid APIs, scaling upwards has no direct financial cost - but if you are as restricted on hardware as Teffie is meant to be, then there is no way that you will compete with models with 300 billion parameters running on servers that are dedicated to executing ML applications. 


## Current state

Teffie is in a prototype state with out-of-the-box models from HuggingFace. 

This is implemented as a command-line application. The chatbot runs forever in a loop that goes audio input → audio transcription → text generation → text to speech generation → audio output. 

At the start of the loop, the user is prompted to hit ENTER to start listening for input; this goes until either 30 seconds pass or ENTER is hit again (whichever comes first). After that, a Whisper `base-en` model transcribes the speech, which is then sent as input into a `phi-1.5` text generation model in HuggingFace's `AutoModelForCausalLM` mode. The output is then sent to an `MMS` model that produces corresponding output audio, which is then sent to output to play[^0]. Once the audio finishes playing, the user is prompted again as the loop repeats from the top. 

[^0]: No intermediate files are created, which I note because I've seen some example code that creates a file. I assume it's faster to skip the OS-level work and just pass the audio waveform in directly as a `numpy` array but there could be something I'm missing.

Logging is included throughout the program; detailed debug logging can be disabled by setting the `DEBUG` variable.

## Things to work on

- The vast majority of TEFL teachers will not be running Linux but rather Windows or MacOS. There would be a lot of work needed to improve user experience to levels acceptable for teachers, e.g. UI, installation / program setup, selection of audio hardware[^0]...I am not familiar with the process of releasing apps on Windows or MacOS, so I'm not sure how difficult this would be.
- **Fine-tuning is a must**. I did not pursue this for the prototype because it was more important to demonstrate a valid proof-of-concept. However, the quality of generated text and speech needs to improve drastically to really consider Teffie "conversational". This is less conceptually challenging and moreso something that will take time and trial-and-error to figure out.
    - Whisper out-of-the-box struggles significantly with accurately capturing accented English. That is, English learners with an accent will often be mistranscribed by the model. This doesn't be fully corrected; learners will make pronunciation mistakes, and the model shouldn't necessarily behave as if they didn't.
    - Phi-1.5 is trained on textbooks and has a tendency to fixate on specific topics - soccer games, a classroom setting, a girl named Lily...
	<!-- A model trained on real-world data (e.g. DistilBERT) could provide more human-like responses out-of-the-box, although it's hard to guess which model might perform better with fine-tuning. This is something I haven't researched thoroughly since I was focused on finding models that fit the hardware limits.  -->
	- MMS just isn't good enough. The cadence of the voice is too unnatural for students in the TEFL classroom to understand, and they shouldn't - no real human talks like MMS. I would like to look into a distilled and/or quantized version of Bark going forward.
- [Post-training quantization](https://huggingface.co/docs/optimum/en/concept_guides/quantization) reduces model complexity by converting floating point weights to simpler `long` or `int` types. This can reduce model size and computation time. This can be used to make Teffie fit on even smaller hardware or to utilize larger, more effective models without breaking the VRAM limit. I'm currently using quantization to reduce the size of Phi-1.5, but
- Some stories regarding usability and QoL, such as handling edge case user behavior and using a config file instead of hard-coding prompts and other information.
- I get CUDA-related glitches on my platform (an Ubuntu 22.04 install) that stop the program entirely; however, I haven't set up the code to properly capture those glitches, and I suspect they may have to do with proprietary NVIDIA drivers on my computer that I can't easily solve.

[^0]: `sounddevice` automatically detects the default input and output audio devices, but for various reasons a user may want to change which devices Teffie uses. 

## Possible additional features

- There are other methods to reduce model size, e.g. [LLM shearing](https://github.com/princeton-nlp/LLM-Shearing) by Xia et al, that might be worth exploring.
- Instead of hardcoding to specific models on a specific VRAM limit, we could detect the VRAM capacity of a device's hardware and to plug in a selection of models for specific VRAM capacities. For example, pick one set of models if you have 4 GB of VRAM or less to work with, a more powerful set of models for 8 GB, etcetera. 
	- This could also include a selection of models for hardware that only supports integrated graphics, which is common for older and/or lower-end computers. Integrated graphics uses RAM instead of storing models in dedicated VRAM, which necessitates usage of even smaller and faster models than the 4 GB VRAM case.
	- Something to note is that . Here in Łomża, I get 
- Depending on the architecture of Whisper, it maybe possible to create a fork and use a submodule of a Whisper variant that doesn't hardcode sample rate and duration or that hardcodes them to smaller values that can better fit on weaker hardware. If this is possible, this may help reduce computation time. (Without having read the paper or code in depth, my guess is that this would require changing the weights and in turn retraining the model; however, distillation may offer a workaround for this.)
	- You could make similar arguments about all models; I highlight Whisper because most audio input will be shorter than 30 seconds. This would be most useful in a fully live setting such as the one outlined below; otherwise, the time saving is minimal because you're only transcribing once per loop iteration.
- There are .cpp implementations of many models that, since they are compiled, take considerably less space and run signifcantly faster; I haven't experimented with these myself. Note that per the second bullet point in this section, I don't necessarily want to attach myself to a strict set of models. 

## Limitations

[Back to table of contents.](#table-of-contents)

## Personal notes

A few thoughts that aren't angled towards audiences with a particular skillset.

. However, open source products are only truly meaningful if they are maintained. 

I'm biased towards building something new . I really like building things from scratch and reasoning about things at the very lowest 

In a weird way, .

- A big part of me is hoping that what I'm writing is careful and measured enough to not make me seem like an AI influencer. I think a lot of software professionals roll their eyes when they hear about the umpteenth AI startup, and in my opinion, rightfully so - a lot of people are applying AI in places that don't seem to really need it. Since I have had success with it myself and since there are education professionals with an eye on the technology themselves, I would like to think that I am applying AI somewhere with real potential. 

### Consideration of a multithreaded live architecture

The current code runs sequentially (minus keyboard interrupts and usage of callbacks in processing and producing audio input / output). However, I originally envisioned a live chatbot that would continuously listen transcribe, produce responses, and output audio throughout the course of an entire lesson. This doesn't necessarily make sense from an end-user standpoint, but I want to document the progress I made and the issues I encountered in case this ever comes up for consideration.

The main difference is in how you handle I/O; in a live app, you need to be continously reading and writing from and to audio input / output. It doesn't make sense to transcribe and respond to every frame or every 100 frames of audio input, so transcription needs to be done in chunks of, say, 5 seconds. (This is a hyperparameter you'd need to mess around with.) Additionally, you likely need some additional logic to make sure you are responding to complete sentences, since it wouldn't make sense to create two replies to a sentence that is 8 seconds long (i.e. 2 or 3 chunks).

There's a few different ways to split up tasks. One is audio input / transcription / text generation / speech generation / audio output; basically, each piece of the chatbot pipeline receives its own thread, and each consecutive pair of threads has a producer-consumer relationship. This is the architecture I originally attempted to implement. In retrospect, I think you can get away with just an audio input thread, a response generation thread, and an audio output thread. Again, the most important parts of the pipeline to isolate are the I/O components; in a performant multithreaded application, the largest model will likely be the main performance bottleneck, so there's not much point in splitting up the work within that response generation component[^0]. 

[^0]: I think the more general lesson to take away from this is that if you have a producer-consumer relationship where the producer is the only one supplying the consumer and the consumer is the only one receiving from the producer, that's a big code smell indicating you really don't need them to be separated into threads. I can see a counterexample where the producer runs in, say, 1/10th the time of the consumer and you need to track every produced item (probably on a queue or similar), but in the case of a chatbot, this doesn't make sense - you shouldn't try to generate a separate response to something that was said before you started saying a response back.

One thing that sticks in my head is the thought that a live app could react more realistically to things that happen while a response is being spoken. For instance, if the model is currently playing a response in audio output but is able to detect a speaker interrupting said output with new input, then it could stop the current audio output to then generate a response to the interrupt. Figuring out whether a response "should" be interrupted, however, might be a whole research project by itself[^0]. Note that what we really need here is concurrency between audio input, transcription, and audio output - we don't need concurrency between the response's text and audio generation.

[^0]: I have not scoured the literature for existing solutions to this problem. If anyone knows of a relevant paper here, feel free to send it to me! That said, an easy "should" is just that if someone else speaks at all, you interrupt - from my experience, that's what most people do in regular conversation.

At the end of the day, this live functionality isn't necessarily beneficial. A live bot may require less non-verbal interaction to maintain back-and-forth conversation with an individual speaker, but without additional logic to filter out what a chatbot "shouldn't" be listening to (which itself requires some thinking about how human conversation works), I imagine it would be a fair amount of hassle for listeners to keep track of where the chatbot is in the conversation. Additionally, people in natural human speech usually take turns back and forth speaking, so having a chatbot that queues and attempts to respond to all audio input would not provide a realistic experience. I think this may be an example of me choosing a solution because I want to use a tool (threading / concurrency) that isn't actually needed for the task at hand (TEFL assistant with portable models and quick inference).

Threading may not even be the best solution to implementing a live app; Python's `asyncio` library provides concurrency within a thread via coroutines and an event system. (This is in fact what `sounddevice` suggests for threads that need to do work while processing audio I/O.) In any case, I eventually decided to pivot because A. I was getting stuck on what specific tools to use, and B. I realized I was wasting a lot of time thinking about features that weren't critical to the success of the final product. There's probably more notes I have in my head on using Python's `asyncio` and `threading` libraries, but those are mostly dealing with the language toolkit and not necessarily meaningful thoughts on the architecture or possible features enabled by said architecture.




---

- concerns abt division between teaching and student time 
	- easier alternative to getting a native speaker? 
	- students turn to foreign language as a first option if they can't - but chatbots won't change language
- controlling amount of output - teachers talk too much (TTT), chatbot won't talk too long and won't interrupt and won't control conversation because it has to be controlled
- downside: students talk to chatbot instead of each other
- port it to students - HW
	- more sense for productive tasks; you couulllld have it in class and significantly multiply student speaking
	- content is a problem, but should be viewed *relative to content in typical discussion*
- trust teachers to be creative enough to make interesting lesson plans
- practice w/o specifying rules - acquisition of language is *usage-based** (usage based model of language acquisition)
	- versus chomsky's universal grammer theory
	- neurolinguistic programming

