---
layout: default
title: "Portfolio Updates"
permalink: /Portfolio/2025/06/10/portfolio-website-blog
---

About a month ago, I graduated from college. 

I didn't have many job opportunities waiting for me after I graduated. This wasn't a struggle I was unaccustomed to; I even struggled to gain any attention for an internship during my time as an undergrad. At the time, I didn't understand why. I was a pretty good student; my projects were interesting but not groundbreaking; I was never a part of any club during school, but I kept myself so busy with my studies that I kind of put that stuff to the side. In retrospect, that was and is my biggest regret as an undergrad. Still, I knew that if I wanted a job (especially in data science) I needed, for the first time in my life, to put myself out there and show what makes me different instead of believing that I am different.

That's when I had the idea of building a portfolio website. While it may not be a unique idea, it's still a place that is all about me, built by me, for me. Wellllll, there's only one problem: I don't know the first thing about HTML, CSS, or JavaScript. Also, hosting a website costs money, and I'm not sure I can justify the investment right now.

Luckily for me, after doing some research, all I can say is: Thank God for the year 2025.

Learning HTML was never the problem; yes, it's a significant time investment, but as a 22 year old fresh grad, I have nothing but time. But as a 22 year old fresh grad, what money did I have to buy a domain and host a website? Luckily, GitHub hosts what are called GitHub Pages, which can host static websites for free, perfect for developing your own portfolio website. Seeing as the money problem was solved, it was time to learn HTML and CSS.


## Version 1:
One benefit of using Github Pages to host a website is access to Git's version control. Using version control, we can look at previous versions of what the home page is and easily see how far I've come. My original design for the website was very simple: Go to the domain, have a homepage, click on a portfolio tab, have an about page, a contact page, and call it a day. Using DeepSeek, I learned some of the basics of building an HTML website with some CSS elements. I had spent the whole semester on a write up with my professor that I found really interesting which was about the Metropolis-Hastings algorithm (which you can read here if you're interested wink wink) so I knew I also had the content I wanted to post. With that in mind, I created version one:

<img src="/assets/homepage(v1).png" width="400" height="300" style="display:block; margin: 10 auto">

This website was okay! For 2007. Seriously though, I could see the room for improvement, and I was happy with the work that was left to be done.

## Version 2:
V2 I would call my box era. Everything neeeded a box. If it it was text outside of the hero section, it better be in container. Here I learned how to place images in my boxes, add some links to outside my website and render some LaTeX that my content needed to include. However I found the content to be a little too seperated. I could reach my about, blog, and content, but it felt like for the little content that it contained, they made for underwhelming individual pages. Either way, in about a week I went from 2007 to 2010. (Honestly an insult to the websites at that time, lol)

<img src="/assets/about(v2).png" width="400" height="300" style="display:block; margin: 5 auto">
<img src="/assets/blog(v2).png" width="400" height="300" style="display:block; margin: 5 auto">

## Version 3:
After moving everything to the index page, I was focused on getting my md pages that needed LaTeX to render properly. I also didn't want these pages to be boring. They were loading on white pages that were filled with text and some images, but I really hated this. Luckily, GitHub Pages allows you to import themes into your website, however, these themes overwrite everything and assume you're building your whole website on top of these themes. I didn't want that; I still wanted to keep the structure of my page and introduce the theme when someone clicked on the link. The theme, Just the Docs, would look nice since it's similar to what online math textbooks use and the pages I want the theme to apply to are math heavy. Also, I force myself to use live preview here since I was originally pushing and waiting for the website to update to see my changes (CRAZY LOL).

<img src="/assets/homepage(v3).png" width="400" height="300" style="display:block; margin: 5 auto">
<img src="/assets/blog_content(v3).png" width="400" height="300" style="display:block; margin: 5 auto">
<img src="/assets/homepage(v4).png" width="400" height="300" style="display:block; margin: 5 auto">


## Version 4:
I finally got everything to work properly. YAY! I should be done and ready to present my website, right? Right? Well.... not quite.

I wake up one morning and realize, I hate everything about this website. It's hard to scale, it looks out of date, and it doesn't render properly on mobile. So, like any rational person would decide, I delete everything and start from scratch. This time I look for inspiration from other creators on Reddit and I find different layouts I really love. This time, I decide to use TailwindCSS as a modern framework for the page and start from a mobile first approach. And that leads us to today June 6th, 2025 where I'm currently updating my page and slowly adding back all the content from v3!

<img src="/assets/homepage(v4).png" width="400" height="300" style="display:block; margin: 5 auto">
<img src="/assets/home_pt2(v4).png" width="400" height="300" style="display:block; margin: 5 auto">