---
title: "Paranoid Habits. Privacy Tips"
date: 2021-03-10T12:01:16+01:00
description: "This is another write up of my recent talk where I share some information about the modern era of data collection and how you can keep your privacy at least partially."
coverCredit: "https://www.imdb.com/title/tt0268978/"
tocEnabled: true
keywords: ["Privacy", "Surveillance", "Tracking", "GDPR", "Google", "Facebook"]
draft: false
---

Recently I gave a talk to my colleagues where I shared some information about the modern state of data collection and privacy violation. It's impossible to avoid all of the tracking but one can take some steps to minimize the amount of collected data.


# Why Care about Privacy?

Surprisingly, when I raise privacy questions I often get something like:

> Why should I care?
> I have nothing to hide, I obey the law.

So, I hope after writing this post I could simply send the link and this article would give a good answer. Because it's not about whether you've done something illegal or not, it's about other people weaponizing your information for their profit.

## Data Leaks

If you're following the news I'm pretty sure you see data leaks quite often. There are always data leaks, doesn't matter how secure a company claims to be. Unless it's technically impossible to gain access to the data (end to end encryption, for example), the data will always find its way out.

Normally, big responsible companies have procedures and protocols in place to protect their data but one cannot avoid a human factor -- if the data is accessible by a human at some point in time it will be accessed. And this is the biggest problem, since a human is always a weakest link in the security chain.

### Corruption

First of all, no background check can guarantee that a particular person is corruption-resistant, just look at the [recent story with Yandex](https://www.zdnet.com/article/yandex-said-it-caught-an-employee-selling-access-to-users-inboxes/): an employee was selling access to email inboxes of their users. Even today, your email inbox is the most sacred place from the security point of view. Having access to your email inbox lets the attacker to gain access to practically everything else you have.

### Blackmail

Second, no person can avoid blackmail. It's not even about your nude pictures or some evidence against you, someone could simply threaten your family. A leverage can always be found. Recent political events in the world demonstrate it very well. Imagine for a second, you cannot reach your kids and you get a call "We know you have access to the database X. If you don't download it today and don't send it to us, you kids will die". What are the chances that you would ignore these demands or you would call the police? So, maybe it's a good idea to not have access to the data in the first place, would not you agree?


### Technical Vulnerabilities

Breaking news: the software is also built by humans. Humans do mistakes. This means it's likely that most of the software has some kind of security vulnerability but it has not been discovered just yet. A more terrifying possibility is that it might have been discovered already but the person who did it chose not to share this fact with the software owner and decided to sell this vulnerability or extracted data on the Darknet instead. For example, let's remember [Celebgate](https://en.wikipedia.org/wiki/2014_celebrity_nude_photo_leak): ~500 private pictures of various celebrities leaked online because Apple didn't implement protection measures from the password brute force attack in iCloud and let attackers to break simple passwords which, apparently, some people used.

### Some Companies Just Sell Your Data

Remember how you agreed to that End User License Agreement (EULA) that said you were fine with selling you personal data to other companies? No? What, you didn't read?

It's not a secret that people don't read these documents, I'm no exception. I think the problem is not those long unreadable EULAs but rather the lack of regulation of such companies that collect our data. Fortunately, it seems to be changing slowly and data collection becomes more and more regulated.

## Leaked Data Puts you in Danger

Okay, now we know how easy it is to get access to our data, now what? What can other person do with my cat pics or my vacation updates? Plenty of nasty things, actually.

### Social Engineering

The more another person knows about you the easier it is to convince you to do something. Imagine there is an attacker who has no connection with you but they know all your data, even if it's just public data from Facebook or Instagram. They send an email to you with something like:

> Hi Denis,
>
> It's Rick, remember we hanged out on Lanzarote last year? We talked about your Honda bike while having breakfast.
> Sorry to bother you but I'm in Berlin and I'm in big trouble right now. I don't know anyone here who could help me except you.
> In an hour or so, you'll receive a phone call and a robot would tell you a code, please send it to me via email as soon as possible, my life depends on it.
>
> Best regards,
> Rick

The facts like my vacation on Lanzarote, what I did there and my current location is just public information that everyone can find online. One should always assume that everything they shared online is public knowledge and they cannot authenticate a person based on that knowledge. This email above never happened for real and some people might say "this is such bulls**t, I would never fall for this" but the reality is that it works on many people and they give away their recovery codes or TANs to scammers because it does sound convincing to them. "It's not possible that somebody knows so much about me and it's not true, I must have forgotten that guy, it was long time ago." -- one might think.

And now a real story that really happened:

One person I know received a call from their bank. It was the bank's security department saying the bank got hacked and this person's account was affected. The bank needed to transfer all the funds to another account immediately in order to preserve all the funds. The security department representative, naturally, knew the person's full name, the account number, what bank products the person had been using and sounded very professional and convincing. The only thing the security department asked for was the TAN received by SMS as a way to "sign/approve the security measures taken with the account".

Well, do I have to explain that there was no hack and no real security department representative? All the funds were gone once the TAN was shared on the phone. And it was shared because of the feeling of urgency, time pressure and the amount of information this "security representative" was in possession of. Scammers don't let you think, they want your immediate action without a second thought. This is simply how they operate. This person lost all their money only because how convincing it was and it was so convincing because the scammer knew the full name, the account number, the products used, etc. This data was simply leaked by someone at the bank and then sold on the Internet, there is no other explanation. And this caused real harm.

After this incident the real security department of the bank would not be able to help you either because using the TAN is the same as signing a transfer with your own hand. Once the TAN is used, it's a legitimate transfer that cannot be undone.

### Politically-motivated Charges. Blackmail

In the current political situation we can easily see how personal data and privacy violation can serve agencies/governments in their operations. The reality shows again and again that one of the most dangerous professions in the world is to be an opposition leader in a totalitarian regime. So, it's very important to fully understand how and what of your personal data is sent to the cloud and how it can be used against you. One can be framed or found and killed simply because of a taken picture (with GPS in its EXIF) that was privately stored in the cloud.

To understand this topic more, I suggest you see the "[Snowden](https://www.imdb.com/title/tt3774114)" movie.

### Public Opinion Manipulation

Your personal data is not different to any other data and any data can be analyzed. Specialists can find patterns in data, forecast the future data and build machine learning models that would produce forecasts automatically without human involved. Like any other tool this one can be used to help the society and to benefit the rich and powerful.

It is possible to use data from Facebook that indicates how people interact with certain content in order to create a perfect advertisement campaign that would convince certain population to vote for a certain candidate. I personally believe that this is exactly what happened during the US elections when Mr. Trump won. It's really hard to prove something like this though, so it's just my personal guess and perhaps it had been done before this too.

## Annoying stuff

Here is a short list of things that are not really dangerous but they might annoy you on daily basis:

### Your Personalized Advertising Profile

There are some global marketing databases where all of the social network and app users participate. It works cross-web-sites, cross-mobile-apps and our [user] data is connected across all the devices and web-sites we use. Agencies who have access to databases like this perform marketing research to determine for other companies how to sell their products to certain population group with the least amount of expenses. I suppose, that's why the ad becomes more and more click-baity, crazy colorful and dumb. If you have a report that says you don't have to spend crazy amount of money on a masterpiece and it would work with way less effort anyway because X percentage of people click on a similar thing in Facebook, you, as a company would probably take the easiest route and save the money.

### Recommendations Based on Location

This is a creepy one. When I was still using the Facebook app, I attended a birthday party where most of the people I had never met before. Next morning I found that my entire friend recommendation list on Facebook consisted of those people at the birthday party I didn't know. I bet it's bi-directional. But what if I don't want those people to see my Facebook page? Nope, Facebook wants you to have more connections, see more content and interact more with the feed. I got rid of Facebook long time ago and had zero regrets since then.

### Gmail is Constantly Scanned

Hard to believe? Why? You don't pay for Gmail, do you? So, they [use it to collect keywords from your emails](https://easydns.com/blog/2019/06/03/googles-gmail-scans-parses-analyzes-and-catalogs-your-email/) in order to improve their ad engine. There is nothing free, if it did not profit the company it would not exist.

It's most likely not just Gmail but other "free of charge" providers too, unless they explicitly claim privacy as a part of their package and you pay for the service.

# How is your Data Collected?

We live in [Surveillance Capitalism](https://en.wikipedia.org/wiki/Surveillance_capitalism).

> Shoshana Zuboff's book The Age of Surveillance Capitalism was published on January 15, 2019. It is a detailed examination of the unprecedented power of surveillance capitalism and the quest by powerful corporations to predict and control our behavior. Zuboff identifies four key features in the logic of surveillance capitalism and explicitly follows the four key features identified by Google's chief economist, Hal Varian:
>
> * The drive toward more and more data extraction and analysis.
> * The development of new contractual forms using computer-monitoring and automation.
> * The desire to personalize and customize the services offered to users of digital platforms.
> * The use of the technological infrastructure to carry out continual experiments on its users and consumers.

-- [Wikipedia](https://en.wikipedia.org/wiki/Surveillance_capitalism#Key_features)

So, it should not be surprising that everything around us strives to collect as much data as possible. Ever got surprised that you bought a small device and its software forced you into getting a new account in some kind of cloud service? Well, they need you data.

How does one get tracked? There is an endless list of possibilities to track users, let's consider the most common ways.

## Cookies

I'm pretty sure who created this "Cookies" feature for the Web didn't even think it would be used to track people instead of functional purposes. Nowadays, it's almost impossible to find a web-site where you don't see this annoying warning about cookies which you're supposed to carefully read and accept. But how many people do? You see, because of the regulations like GDPR web-sites have to disclose what kind of cookies they set, what cookies will track you and what cookies are necessary for the web-site to function. In theory, every user should be able to reject tracking cookies and enjoy their privacy. In fact, most of the web-sites make it incredibly difficult to reject tracking cookies. They use dark UI patterns, tricking users into accepting them or they use some unclear terms like "Legitimate Interest" which is another way to trick you to accept the tracking cookies.

What are the tracking cookies exactly? I'd say it's anything but cookies required for functioning of the web-site. It can be analytics or 3rd party trackers that would help to build your profile for advertisers.

## Online Assistant

That was surprising for me personally when I learned about it. Turns out that when you visit a web-site and have a little pop-up in the right bottom corner of you screen saying something like "Need help? I'm here for you!" the person on the other end, most likely, is already seeing your personal file with your full name, email, social networks and perhaps even your phone number. At least this is how it was shown to me with [Intercom](https://www.intercom.com/customer-data-platform?on_pageview_event=nav) and they even advertise that as their feature.

## Android and iOS

This is about to change soon but it's still a thing. Your phone has a unique identifier that every app has access to (at least for now). On iOS it's called "Identity For Advertisers" ([IDFA](https://www.adjust.com/glossary/idfa/) or IFA for short), on Android it's "Google Play Services Advertisement ID" or [GPS ADID](https://support.google.com/admanager/answer/6274238?hl=en) for short. Using these identifiers marketing networks can track you across multiple mobile apps recognizing you as single identity and building your personal profile: what apps you're using, how often, what are your preferences, etc.

Nothing scary right? What if I told you that using this metadata it's quite easy to detect very personal things about you like: when you wake up, where, who you slept with, what car you drive, what are your moving patterns, whether you're pregnant and many more.

"Pregnant? No, that's bulls**t!" you might say. Then look at [this research](https://privacyinternational.org/report/2647/how-apps-android-share-data-facebook-report) from the 35C3 conference, there is a nice [presentation video](https://media.ccc.de/v/35c3-9941-how_facebook_tracks_you_on_android) too. A woman was using an app to track her health data and turned out because that app used the Facebook library for the "Login with Facebook" feature (which the woman didn't even use by the way) it was sending all her personal data to Facebook including very private health data. Based on that she was receiving ads for certain products.

Nowadays, you can regenerate these identifiers and break the link but it's like to reset your profile and start collecting the data all over again, not very helpful. However, Apple (maybe Google soon) is taking steps to make it harder for the apps to use the advertising ID or at least notify the users about accessing this identifier which might destroy the whole industry eventually.

## Google

I already mentioned how Google scans your emails but that's not all.

If you have a Google account, it's tracking most of the things you do by default, including your GPS coordinates, they call it Location History or [Timeline](https://timeline.google.com/?hl=en-GB). Google literally stores your every move if you have your Android phone with you. Also, Web and mobile app activity (what you install, what you open and when). Thanks to GDPR they now have to provide the tools to disable all of that which you can find [here](https://myactivity.google.com/activitycontrols).

## You Share it Yourself

When social networks became norm something weird happened to our society. A lot of people want to share their personal life with the Internet. If previously such thing was limited to the close friend circle now people want the whole world to know about their private things, taking the chance to somehow get popular, to have followers and to become a celebrity. In my opinion, every time you share something personal with the whole world you make yourself vulnerable to the bad people who are Internet users as well. It's not all rainbows and unicorns. Be careful and think twice: do you want an abstract pedophile murderer to know where your kids go to school or an abstract rapist to know from which gym you're returning late every day? I suppose not. These people are very likely to be looking for their victims online nowadays.

There is a list of less explicit things too:

If you send a document via email and it's not encrypted -- it can be intercepted and it's not considered private.

If you don't use an End-to-End encryption in a messenger -- your communication is not private. For example, one of the most popular messengers Telegram does not have E2E encryption for text messages enabled by default, you need to open a "Secret Chat" for that. Any message outside a secret chat is not private and can be accessed.

If you use IoT devices like in-house cameras they cannot be trusted. IoT devices get constantly hacked like the incident with [Ring](https://www.theguardian.com/technology/2020/dec/23/amazon-ring-camera-hack-lawsuit-threats) where attackers gained access to the cameras and its speakers watching and scaring adults and children.

If you share a picture, keep in mind every picture you take on your phone contains its GPS coordinates in the [EXIF](https://en.wikipedia.org/wiki/Exif) header and when you upload this picture, for example to Twitter, it's their responsibility to trim this information but do you trust them to do it for you? There have been instances of people disclosing their home address via sharing pictures taken at home.

If you share a document by link, for example Google Docs, you should treat it the same as you'd share it with the whole world. There is no guarantee that this document would not get indexed by the search engines somehow and it would not appear in search results of another person. Share private documents adding people one by one or send the document via a secure channel instead, don't be lazy.

## Many Many More

Things like voice assistants can expose your data to other people, for example, [Amazon let its employees to listen to Alexa recordings](https://edition.cnn.com/2019/04/11/tech/amazon-alexa-listening/index.html). It's not even about commands you say to Alexa, it's about the stuff happening in the background. It's possible that someone is talking about important things in the background while you're asking Alexa or Google Assistant about the weather.

Browser extensions which have a lot of access to your data can be sold to scammers. I once had a colleague who had used a browser extension for many years. It was simply for searching tabs. At some point the author of the extension decided to sell it to someone. This someone released an update and the extension started to collect all the data users typed on the page including their credentials. Probably, the new author wanted to collect logins and passwords and then make even more money on that.

Some mobile apps might be spying on you: for example, [Facebook was secretly using camera](https://thenextweb.com/apps/2019/11/12/facebook-camera-ios-iphone/) when the app was open, [so did Instagram](https://www.msn.com/en-us/news/technology/facebook-accused-of-watching-instagram-users-through-cameras/ar-BB199T81). People even [reported](https://www.thesun.co.uk/tech/7497249/facebook-listening-to-you-microphone-ads/) that Facebook was listening to your conversations.

Even essential things such as [VISA](https://usa.visa.com/partner-with-us/visa-analytics-platform.html) and [MasterCard](https://www.mastercardservices.com/en/data-analytics) sell you transaction data. Your [ISP](https://protonmail.com/blog/private-browsing-history/) (Internet provider) is selling your browsing history ([DNS](https://en.wikipedia.org/wiki/Domain_Name_System) queries) and more more more.

Government agencies have access to databases of big companies and they collect your data from them as well. So, everyone has your data. Frankly, it's better just to minimize the volume of the data we upload to the cloud instead of thinking who has access to it now.

It's just impossible to talk about every single way our data is collected but I hope the point is clear enough. The goal of the article is not to tell you about all the tricks and catches but rather to raise awareness and to show that you should make more mindful choices.

# How to Protect Data

Follow these steps in the strict order:

1. Delete all your social media
2. Destroy your mobile phone
3. Follow [this guide](https://wildernessmastery.com/living-in-the-woods/)
4. Enjoy your new life free of tracking.

You don't like living in the woods? Oh, well. Okay, there are some less radical options.

## Make Mindful Choices

* When accepting the cookie settings on web-sites -- don't just click “Accept All”, read the details. It's usually a one-time thing.
* When installing a mobile app -- choose right permissions, don't just accept everything. If you've installed a calculator app and it asks for you location it should raise a red flag.
* When installing a browser extension -- install only if you really need it, check the author, prefer open-source extensions. Read what access the extension has.
* When registering on a web-site/service -- do it if you absolutely need it, the fewer accounts the less data you share
* When sending sensitive data -- encrypt it, it's [really not that hard](/posts/tech/security/security-tips/#pgp-pretty-good-privacy)
* When paying with the card -- make sure it's okay to show your purchase to other people, otherwise it might be a good idea to pay with cash instead
* When uploading your very private pictures and videos to the cloud -- certain content is better off the cloud and should remain on local storage devices like SD cards or USB drives.
* Speaking of pictures -- trim EXIF data from your pictures e.g. using [ImageMagick](http://www.imagemagick.org): `mogrify -strip picture.jpg` or a similar mobile app if you trust it.
* etc. etc. etc.

> Don’t upload anything unencrypted that you would not show to the entire world

## Change your Settings

* [Disable](https://myactivity.google.com/activitycontrols) all the activity tracking in your Google account
* Opt-out from personalized ads and tracking everywhere
(many services have this option hidden in their settings)
* Don’t use a voice assistant, it's a listening device running around the clock at your apartment and in your phone
* Browser settings: set "no prediction", "no helper services", enable "Do Not Track"
* [Set a passphrase in Google Chrome](https://support.google.com/chrome/answer/165139?hl=en&co=GENIE.Platform=Desktop#zippy=%2Ccreate-a-passphrase), this is supposed to End-To-End encrypt your data like passwords, so Google would not have access to it.
* In general, always check settings in new apps/services you start using

## User Privacy Tools

* [DuckDuckGo](https://duckduckgo.com) – a search engine that does not track
* Pay for your email (my email costs me € 1.95/month on [Hetzner](http://hetzner.com))
* [Privacy badger](https://privacybadger.org) – blocks and reports tracking
* [tosdr.org](https://tosdr.org) – database of human-readable terms of service points (+extension)
* [ono.one.one.one](https://one.one.one.one/dns/) – claims to be a privacy-oriented DNS service which if you start using won't report domains of the web-sites you visit to your Internet provider.
* [UTM stripper](https://github.com/jparise/chrome-utm-stripper) – strips Google Analytics (i.e. UTM) parameters from URLs which won't allow to track you when you click marketing links, also when you share such links with others
* Learn how to [use GPG](/posts/tech/security/security-tips/#pgp-pretty-good-privacy) and encrypt everything important you upload or send

# Things are Improving

As I mentioned before, data collection gets regulated and things like [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) make lives of data collectors more difficult.

There is also [Electronic Frontier Foundation](https://www.eff.org) that's fighting for privacy rights and tries to hold big companies accountable.

Even some big companies like Apple are trying to make data tracking more exposed, so people can make their own choice to opt-out.

But don't forget that some companies and their profit are built almost entirely on data collection and advertisement, right Facebook and Google? So, these companies will most likely never change and we should not expect them to change. Just remember how they function when you use their products.

> You always pay for the service even if it’s free of charge

Here is the [link](slides.pdf) to my slides.

As usual, if you have any feedback or question feel free to write me an email to [denis@rdner.de](mailto:denis@rdner.de).
