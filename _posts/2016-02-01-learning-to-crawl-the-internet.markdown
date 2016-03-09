---
layout: post
title:  "Learning to Crawl (the Internet)"
date:   2016-02-01 18:00:00 -0500
categories: web programming
---

Most projects start with wanting to know something: how many dinosaurs would be necessary to re-take over the world, could cats become world-famous scientists, etc. In my case, I just wanted to know what famous people were born on a particular day of the year. Google does a great job with their [doodles], but they are very selective with who gets one. You probably think this is a non-issue — there are tons of sites that have this information, right? And you would be correct, except for one problem: they look awful. 

If I wanted to use a website with the UI/UX from the early 2000s, I would not be writing this article. Like most people, I really enjoy using the Internet, and I want it to look good. I also believe in minimal, unintrusive design without ads for hoverboards in my face. Based on these principles, I had the requirements for my minimum viable product. I would develop a website that:

* Shows you a famous person that was born on the current date. 
* Information about the person (birthday, some information, a picture, and a link to learn more about them).
* Loads a different person every time you refresh (or click a button).
* Allows you to choose any day of the year.
* Be available for offline usage. 

Seems simple. Except, as I got started, I ran into **problem one: what people would be on this website?** It would be a bit bias to put myself on, and I could not find any good APIs that solved this problem. After some quick google-fu, I learned that Wikipedia was my answer (as it is the answer to everything else). It turns out that Wikipedia has a page for every day of the year. On this page are significant events, births, and deaths. Awesome!

Except this led me to **problem two: I had no clue how to get the information off that page**, unless I wanted to embed an iFrame of that unstyled list into my site. Take a second to visualize that. Sorry about that, next time I will include a trigger warning. Another option could just be manually copying the list from every page into a giant JSON file. There are only 366 days (do not forget February 29th!), after all, and this is a MVP. I decided not to do that, however, as I was reminded of a quote.

> “I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it.” 
> 
> — Bill Gates

Save every list myself? That would be madness and probably take longer than writing the actual program. And that is how I found the solution to problem two: **learn how to crawl**. Or, more specifically, learn how to crawl every one of these wiki pages to get the information I needed.

The code of my project went through three major revisions. I want to take some time to thank [Jonathan E. Magen] for inspiring the second two revisions, lessons learned, and other major aspects of this application. Without his advice and guidance, we may still be looking at famous birthdays on a list that looks like this: 

![Birthday List](/public/img/onthisday-birthday-list.png)

`We want to avoid sites that look like this. Source: http://www.onthisday.com/today/birthdays.php`

Anyway, let us look at the major takeaways from each of these revisions and lessons learned.

### 0.1: Everything is in an Angular service. 
Version 0.1 of the site had everything in an Angular service. Every request meant multiple sub-requests to the Wikipedia API. That’s right, all parsing, formatting, and querying was done on the fly. As you can imagine, this was not very efficient. The code was extremely messy and unorganized.

Lessons learned: 

* **Use test driven development.** I had no idea what worked, what did not, and what would break if I changed something.
* **Break code into logical classes via the [single responsibility principle].** I like to think of this as the “do-not-have-a-function-be-a-hero philosophy”. Functions should have one responsibility, they should do it well, and they should rely on their fellow functions to do the other parts.
* **Stop doing the same actions more than once!** Why query/parse/format this data on-the-fly when it only has to happen once. 

### 0.2: Use the latest, most hipster technology ever. 
Version 0.2 had two goals: break the application in half, and use the latest, coolest web frameworks. One of these was accomplished.

In order to make the app faster, Jonathan  suggested breaking the application into two parts: the web app, and the crawler. The crawler would be responsible for:

* Parsing every Wikipedia page and grabbing the list of names.
* Querying every name to get additional information about them (short biography, birthday, picture).
* Saving the information into a data store.

The web-app would be responsible for:

* Serving the birthday data.

The first part went great. Using my newly learned lessons, I created a Node.JS application called WikiCrawler. Using the MediaWiki API and Wikipedia, it makes the necessary requests and saves the data into JSON.

It takes ~100 seconds to gather the data for ~90,000 famous people. Not too bad when you factor in rate-limits and me trying not to get IP-banned from using the API. Note: at this time, this does not include the requests needed to get images.

The final file size wound up being 42.8 megabytes. Not too bad, but not something you want to load every time you use a website.

The web-app was supposed to be a super-sleek Angular 2 app, built with TypeScript, componentized, and compiled with Babel. Angular 2 just hit beta, and it is awesome! It also has a fraction of the documentation that Angular 1.x has, which can be a problem when you are not sure what is broken, and what is you doing something wrong. After internally struggling with how to build my web-app, I was reminded of another quote.

> There are two kinds of start-ups. Those with beautiful code, and those that ship. 
> 
> — Silvio Galea

This quote is what inspired the next (and current) version.

### 0.3: Keep it simple, get it working, get it public. 
The goal of version 0.3 was to use my (mostly working) crawler and get this app shipped! This version solved my main problem:

* Write code that makes sense.
* Loading the data quickly and efficiently.

I ditched Angular 2, for now, and went back to the stable yet reliable Angular generator. I have experience with it, it works, and there is tons of support for it.

When I talked to Jonathan, he had a simple solution for the data problem: “why not just break the data up by day”. So I did, and created 366 JSON files. Boom, instant loads. Completely offline. Now that my code was in a workable format, I was able to easily create services to handle my data. And Famous Birthdays was born.

## Let’s wrap it up.  
Famous Birthdays is currently live, on the web, and is hosted on GitHub Pages. At the time of this article, we do not currently have images, but they will be added soon.

I learned to crawl. This project may not change the world, but it was an amazing learning experience for me, for future projects. It was not about the problem of finding out what famous people were born, but solving the surrounding problems:

* How can we programmatically use Wikipedia to quickly and easily gather required data.
* Learning to use the right tools for the job.
* Writing a program that is readable and reliable. (Or, the importance of building a solid foundation on important concepts, such as test-driven-development and the single responsibility principle).
* Approaching a problem that seems easy on the outside, but is in fact a lot more complicated on the inside. 

Thank you for taking the time to follow the journey of how I learned to crawl! I have learned a lot on this project, and there is still more to go. Next stop, maybe the Chrome Web Store. And after that is out-of-scope for this post.

*You can check out the project [here](http://justinmaslin.com/birthday).*

[doodles]: http://www.google.com/doodles/
[Jonathan E. Magen]: https://medium.com/@yonkeltron
[single responsibility principle]: https://en.wikipedia.org/wiki/Single_responsibility_principle
