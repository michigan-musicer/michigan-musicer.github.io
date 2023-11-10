---
title: "Setting up the Staticman API for your personal site in 2023"
header:
  teaser: "/assets/images/2023-11-09-setting-up-staticman/staticman-avatar.png"
  og_image: "/assets/images/2023-11-09-setting-up-staticman/staticman-avatar.png"
categories: 
  - Software
  - Personal Projects
tags:
  - software
  - projects
  - webdev
  - meta
---

The main reason I've taken so long to get my blog up and running is because I've had a hard time getting comments with Staticman to work. It took a week or two to understand the basics of all the tools I was using; I spent a further week or two trying to figure out what it would take to add replies, then spent a day executing only to realize I was blocked. So the blog is coming out much later and with less functionality than I wanted. Post-mortem, the obvious question is "was there a point to sticking to Staticman?".  

Based on what I value in software, I feel like Staticman was worth the effort and the delay. I'm much happier with the ethics of running Staticman on my blog as opposed to a commercial vendor, and I prefer the architectural promises of Staticman over those of the other open-source options that my template offers. I'd like to explain my reasoning for choosing Staticman, but feel free to skip the first bit if you just care about the technical nuances I had to figure out. 

## Why Staticman?

[There's some perfectly fine documentation from Staticman](https://staticman.net/) in favor of its own existence, which for the most part I agree with. I'll flesh out the most salient point:

Most people who write their own websites do not implement their own comments systems. A basic comments system requires actual integration with the internet that isn't necessary with a basic static site; you need to write the logic to figure out how to receive POST requests from your users and update site content based on the content of said requests. Personally, this is something I could learn, but it's not something I know currently. At this point, when it comes to building  something, I don't see much wisdom in making my own thing from scratch when I can plug in a pre-existing solution that other developers say works perfectly fine[^1]. And even if you were to take the time to make this basic system, it wouldn't even scratch the surface of the full, modern feature space you might want: replies, likes, editing, user notifications, moderation, spam filtering, ...so on and so forth.

The point being: comments on blogs are mostly provided by third-parties. My Jekyll template has building blocks pre-written for Disqus, Discourse, Facebook comments, utterances, giscus, and Staticman. Disqus and Facebook are corporate services, while Discourse, utterances and giscus are open-source projects. Discourse has its own infrastructure while utterances and giscus are built upon GitHub Issues and GitHub Discussions respectively. 

Having a commercial vendor for your comments presents privacy concerns. The big thing is that your commenters' activity will likely get fed to trackers without their consent. You can find plenty of past controversies on Disqus, and plugging into the Facebook ecosystem is asking for obvious trouble. From a more fundamental architectural standpoint, it feels undesirable to have content on your previously static website suddenly depend on a third party; you are dependent on their services to stay up and for their databases to store your content, and you can't easily add on features to your comments if you wanted to. Discourse avoids the privacy issues of Disqus and Facebook by virtue of being open source, but the dependency is still there. (Still might be worth it depending on the breadth and quality of features it provides, but I haven't looked into it enough.) utterances and giscus are less problematic in this aspect because the content is at least contained within the same GitHub project; I still think that it's not ideal.

Staticman is the only public open-source API I've found that adds and maintains comments within the site sources itself. Here's a helpful architecture diagram for the service, which is taken from [the Staticman docs here](https://staticman.net/docs/index.html). 

![Staticman architecture diagram. POST requests on the websites are sent to a Staticman instance, which then goes to a Git provider. Git adds and commits the comment as a change to the repository, then sends the change to the remote repository as a PR. The comment gets published when the PR is merged in.](/assets/images/2023-11-09-setting-up-staticman/staticman-architecture.jpg "Staticman architecture diagram")

The important point is that all the content that gets added ends up in the GitHub repository. Comments are stored as website data, and there is no service outside your website that you need to communicate with or rely on for comments your work[^2]. Ergo, the concerns outlined above with the other options for comments do not apply to Staticman. 

I'm curious how secure the Staticman pipeline is in practice; just based on the architecture, I don't see how Staticman exposes vulnerabilities by itself[^3]. Even if an attacker does secure a commenters' information, that information only includes a submitted name, comment, and email address; so a "breach" does not give attackers particularly sensitive information. And, as mentioned before, using your own Staticman instance means that you can decouple commenters from the ad tracking ecosystem.

Lastly, the nice thing about Staticman not receiving further updates (at least based on GitHub activity) is that there probably won't be breaking changes down the line. One of my projects (Brilliant Little Fires) is currently down because of some deprecations to JavaScript pacakges[^4]. Changes to services provided by Heroku and Mailgun may be impactful, but given how long they've been around and how widely used their services are, I doubt they will push breaking changes without notice. 

Staticman has some noticeable disadvantages (besides the cumbersome setup). Most of these disadvantages have to do with reduced features; you can't edit your own comments, and without doing a lot of legwork you can't reply to comments either. You could technically do stuff like add pictures, GIFs, and other media in your comment because it's all just Markdown, but Markdown by itself does not provide a pleasant user interface (and don't count on users wanting to do any of the fancier stuff on mobile). I don't think anything is preventing you from building this stuff on your own and using it to extend your Staticman instance, but it would be a bit of a pain.

Discussions on coding philosophy are all well and good, but in the end I don't think any of this is too insightful for engineers who have stronger web dev chops. I'd be curious to hear from developers using other comment providers about why they use those providers over Staticman. Regardless, let's move on to the setup details.

## Creating and connecting a Staticman instance using GitHub Apps 

It used to be that Staticman services were provided for free from a server hosted by Eduardo Bouças, the original dev. Due to increase in users, this is no longer sustainable, and you have to host your own instance of Staticman. The instructions on the Staticman website are mostly fine, but I ran into some confusion that I'd like to help clarify for folks using Staticman in the future. 

I hosted my instance on Heroku. As of now, Staticman's docs say it's free; however, Heroku recently dropped their free tier and you now need to pay $15 a month or so to maintain an instance. If you have your own server running somewhere, it's probably worth setting up there instead. If you know of an alternative to Heroku or you are going forward with setting up your own server, do please document it. I'd be curious to see what the process looks like and whether there might be any other updates due to Staticman's docs. 

### Setting up authentication (Connection closed without response)

If Heroku logs give you a `Connection closed without response` error, this probably means that you have an issue with authentication. If you follow the setup guide, you should have authentication set up so that your instance can communicate with your Git provider. I am using a GitHub application for my GitHub repo. 

You will need to fill in some key-value pairs. **For GitHub Apps, fill in** `GITHUB_PRIVATE_KEY` **and the** `RSA_PRIVATE_KEY` with the contents of the .pem file that is generated after you create the GitHub App. I also set a value for the `GITHUB_APP_ID` key, but I don't think you need it. For both `GITHUB_PRIVATE_KEY` and `RSA_PRIVATE_KEY`, be sure that you include the entire contents, including the header and footer[^5] of the RSA key. 

```
-----BEGIN RSA PRIVATE KEY-----
blahblahblah...
-----END RSA PRIVATE KEY-----
```

The Staticman docs are vague on which key / values you should be setting. `GITHUB_PRIVATE_KEY` is for GitHub Apps specifically. If you are using a bot or your own personal account and running Staticman, then you use `GITHUB_TOKEN` instead. I referenced [this issue](https://github.com/eduardoboucas/staticman/issues/406) to figure this out.

If you are hosting on Heroku and you aren't getting 504 authentication errors when you do `heroku logs -a=<appname>`, then your key-value pairs should be correct. Look elsewhere if you're getting an error.

###  (`GITHUB_READING_FILE` 500)

Staticman reads a config file to know what settings to follow on your website. **You need to add and push a `staticman.yml` to the root of your site's remote repository** in order for Staticman to work.

I received what turned out to be `GITHUB_READING_FILE` errors because I was testing locally and didn't actually push `staticman.yml` to my remote repository. Your Staticman instance needs to read your `staticman.yml` file for the config, so if that file isn't in the remote, then Staticman doesn't work. In Heroku this showed up as 200 OK, but in my browser developer tools I saw 500s. 

These were the two main errors I got hung up on while setting up Staticman. Hopefully if you're working with Heroku and GitHub Apps, your experience will be similar and you won't have anything hairier to deal with.

## Closing thoughts

Staticman is relatively well-documented compared to some of the stuff you can see in both open source and corporate. However, it's not perfectly up to date, and it was good practice for learning how to learn about, use, and integrate pre-existing software in my projects. College has made me better at writing code than reading code, so I appreciate opportunities to practice reading and learning about how skilled developers around the world write software.

Again, I think the difficulties I had with integrating Staticman were worth its advantages. Privacy is turning into a bigger deal over time, and I'd like to respect it as best I can. It is, in my amateur mind, also the most architecturally satisfying solution, with all content kept with the rest of the site data and the service instance itself being entirely under your own control.

I hope this article is useful to anyone looking to set up comments on their own blog. Do please leave a comment if you have any questions while setting up your own Staticman instance, and I can try to help out as best I can.

*Teaser and OpenGraph header for this post are from the [Staticman front page](https://staticman.net/).*

[^1]: I can hear folks saying "but then stupid code just floats around forever!" and it reminds me of what my senior design project professor said: people will care a lot more about middling solutions to problems that really matter than elegant solutions to problems no one sees as a priority.

[^2]: There is a concern here about having so many comments that your GitHub repo gets full, but [according to the docs](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github) they don't complain until you get around 5 GB. Individual Staticman comments are usually a few kB at most, so I don't anticipate this being a huge issue.

[^3]: I'm not sure how to word this exactly? What I mean to say is that unless you can expose a vulnerability in Heroku or the GitHub App, or if you are doing a more generic network attack, Staticman shouldn't be vulnerable. In other words, using Staticman should not broaden your attack surface besides adding in Heroku / GitHub / your own servers if you are using them. I hope Roya and Ram aren't reading this footnote and cringing at me hehe.

[^4]: I *think* the issue here is that AWS Amplify (or maybe `npm` at the root? but my site runs locally, so I think it's on the AWS level) refuses to "properly" install packages on the server if the versions of those packages have security issues (which is perfectly fair). I couldn't just update it because making some of the packages up to date would cause other packages to go out of date, and vice versa. (Not sure why now that I think about it? Probably packages being on different branches of React or something?) I'm guessing some of this is from the boilerplate that comes from using `create-react-app` (which appears to be sunset now), so I will echo some of the recommendations I've found online and say that you should follow [the docs](https://react.dev/learn/start-a-new-react-project) on which new tools to use to bootstrap your React projects. Either way, fixing this project is not a top-priority ticket at the moment. It is something I like showcasing as a teacher, so it'd be nice to take a weekend at some point and remake the site with up-to-date packages.

[^5]: no idea if that's what it's actually called.