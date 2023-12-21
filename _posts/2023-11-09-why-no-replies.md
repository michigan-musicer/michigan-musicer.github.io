---
title: "Why can't you reply to comments?"
header:
  teaser: "/assets/images/2023-11-09-why-no-replies/comments.png"
categories: 
  - Software
tags:
  - software
  - projects
  - webdev
  - meta
---

# actually i dum dum

While writing this article I realized that comment replies are perfectly doable right now. What isn't doable right now is email notifications on updates to comment threads; that's fine. I guess you'll see me adding replies soon, once I find the time...

What I originally wrote is all down below, but what it actually applies to is Mailgun notifications, not replies / nested comment threads by themselves.

## What I was originally writing

Most of you who read blogs (and I don't mean you CS students reading those raw HTML pages put out by people in our field who have decided that CSS and JavaScript are for losers) will be used to being able to reply to and converse with other readers in the comments section. My blog currently does not have this feature, and it will not have said feature until December. The best non-technical explanation I can offer is this: I am using different code for the comments than most other blogs, and the way that the code is set up requires me to do a lot of work I would otherwise not have to do. Because of the business structure of certain companies and broader rules governing the internet, doing that extra work would require me to pay a lot of money every month until December of 2023. I would like to not pay a lot of money, so your experience on the site will be a little worse until I can do that work for free. My apologies for the inconvenience.

For folks interested in the technical details: the short of it is that because I am using the Staticman API and because my DNS service provider is not designed for integration with GitHub Pages, adding comment replies either requires me to wait until mid-December or to pay $100 for two months of DNS routing from AWS Route 53. Diving a bit further gets into some interesting details on webpage hosting with GitHub Pages, reply functionality using the Staticman API, and the ecosystem of domain registration and DNS for devs working on personal projects.

## Hosting with GitHub Pages 

I wanted to host and maintain a blog for the first time, and to me the easiest way to do this was to use a Jekyll template hosted on GitHub Pages. My previous websites have been messy self-created React apps, and while I'm pleased about what I learned from working on those sites, I wanted someting that would look more polished and professional-looking. At least in web dev, I've soured on the notion of writing my projects from scratch; there are so many reinvented wheels out there, and I don't think having your own means a lot.

GitHub Pages are by default hosted at `<your GitHub username>.github.io`, which would be a subdomain of a GitHub-owned domain. You can deploy the website to your own domain if you own one, but this requires you to reconfigure whatever you are using for DNS services per [the GitHub docs here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages). I'm not too sure on the specifics, but I think if you want to "do stuff" with your GitHub pages site, you have to have a custom domain[^1]. This requires you to go through the setup process, which normally wouldn't be an issue but (as discussed later) ended up being a blocker for me.

It's less hassle if you don't have a custom domain, but Staticman requires you to use Mailgun for reply notifications, which in turn requires you to have a custom domain.

## Adding reply notifications with Staticman and Mailgun

I'll focus specifically on replies with Staticman; for more details on Staticman in general and why I chose this API over more common APIs like Disqus or Wordpress, please see [this post](/software/personal%20projects/setting-up-staticman/) as well as [the Staticman page itself](https://staticman.net/). 

Staticman_v2 allows for replies[^2] and reply notifications using Mailgun. Mailgun is a service that automates sending emails from your website to users (plus additional services to build your web presence, but I mostly care about the emails themselves). You don't need to know too much else about Mailgun except that you will need to update your DNS rules to integrate it into your website. Which gets to the next point...

## Domain name / DNS providers, fees, and transfer rules

The custom domain I prepared for this website is currently sitting in AWS Route 53[^3]. AWS (from what I can tell) does not play nice if you try to use resources outside of AWS, so attempting to configure DNS records to work for a custom domain would cost $50 a month. At least, that's the price I ended up facing when I tried to do this.


To work around this, I looked at other DNS providers and settled on hosting with CloudFlare. Their free tier is enough for me to do what I need to do, so all that remained was to transfer my domain registrar from Route 53 to CloudFlare.

This finally gets to the concrete reason why I can't just transfer the domain over and configure the DNS records on CloudFlare right now: ICANN (the [Internet Corporation for Assigned Names and Numbers](https://www.icann.org/)) prevents the transfer of domains that you purchased in the last 60 days. I purchased the domain in October, so I have to wait until December to do anything with Mailgun.

## Closing thoughts

So...yeah. I can actually implement comment threading and replies perfectly fine, but making a more complete reply setup with notifications will have to wait until December. I don't blame myself for running into this issue; it's a lack of experience more than anything, and going down this rabbit hole has taught me a bit about some of the practicalities of the web dev ecosystem. 

I did not run into blockers like this before because in the past, I've only hosted websites from AWS directly using AWS Amplify and Route 53 domains -- no other vendors involved. There is no fee to host web apps on domains registered via Route 53 (I mean of course not, what kind of predatory business model would that be?), so no $100 pricetag on tech changes. 

Again, if you would like to learn more about Staticman in general and why I'm using it, see [my post here](/software/personal%20projects/setting-up-staticman/) or the many detailed posts written by other Staticman users / contributors on the internet. (Most of them are more experienced than me and blab a little less, hehe.) I'll aim to get replies working before Thanksgiving, although whether or not I actually end up doing that depends on how much time my Fulbright work and my other projects take up. Will post a hopefully shorter update once that comes through.

[^1]: I think what the specifics boil down to is that you can't configure GitHub's DNS servers, so if you need to connect a service that uses network requests of some sort, you need your own domain registrar with customizable DNS settings. This is the same reason why you need to reconfigure DNS settings if you point your GitHub Pages repo to a custom domain.  

[^2]: If you recall my note at the beginning, the misunderstanding I had is that the API will let you do replies even if there are no notifications. Mailgun (the main Staticman dependency rooting this chain of blockers) is only required for reply *notifications* to an email address, not to replies and comment threads by themselves. Good job me :P

[^3]: Fun bit of info that isn't actually relevant to the financial blocker: DNS records come in a number of different flavors. I haven't taken the time to learn the difference between all of them, but the two types the Route 53 docs distinguish between are alias and CNAME records. Route 53 seems to be designed for alias records ([see docs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)), and if you use CNAME records Route 53 will charge you extra. However, any queries to non-AWS resources are charged anyway, so in this case it didn't end up mattering whether I used CNAME or alias records to redirect my GitHub Pages. The $50 figure comes from trying to use alias records, so I imagine CNAME records would be even more expensive.
