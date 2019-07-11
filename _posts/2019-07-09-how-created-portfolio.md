---
layout: post
comments: true
title: "How I Created my own Personal Portfolio"
date: 2019-07-09
categories:
  - Personal
description: In this post, I write about how I created my own Data Science portfolio.
image: /images/chalkboard_leaves.jpg
---

This is my portfolio.

It was quite challenging to get to this stage, so I want to share how I created my own portfolio.

I was browsing about a week ago on GitHub and discovered GitHub pages for the first time. It provided a way to make websites out of any repository without getting help from website builders like Wordpress and Wix. I always felt these website builders are not really part of me, so I decided to create my own customized blog. So here I am, writing in this brand new blog.

![](/in-post-images/Screen Shot 2019-07-11 at 16.00.06.png)

To briefly explain, this blog will be a one-stop website where I document any new Data Science skills, personal projects, and simply my thoughts as well. Through this, I want to keep track of what I do, so that I will be able to roughly estimate what I did in certain point in time.

Whenever we try to gain new skills, there is always a learning curve and a point in time where we say, "Aha, now I can comfortably search on Google to dig 'deeper'". To learn how to make a [Jekyll](https://jekyllrb.com/) website and customize it, which is an engine that powers GitHub pages, I had to go through a quite steep learning curve. On GitHub Pages official website, it says it will take only minutes to get it up and running. It was true, but I wanted to go further than just using one of the themes already built in.

And I am sure many of those who want a website are also like me.

I started by exploring [Jekyll themes](http://jekyllthemes.org/). Among hundreds of themes, I chose [Trophy theme](http://jekyllthemes.org/themes/trophy/) which looked simply and elegant. Now, I faced a problem: How on earth should I go about using this theme?

![](/in-post-images/Screen Shot 2019-07-11 at 16.04.30.png)

GitHub pages documentation didn't help much. Instructions on Jekyll official website were too new to me and confusing. What I learned is that I can create one website using my GitHub username and set it as a blog. Building websites for projects is another topic. I created new repository and named it `sangwookcheon.github.io`. The name has to be formatted as `username.github.io` so that GitHub will automatically detect it.

First I decided to look at video tutorials for creating Jekyll websites. I watched [four tutorials](https://www.youtube.com/watch?v=T1itpPvFWHI&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB), and stopped right there. I went to Trophy theme's GitHub repository, downloaded all the files from the Master branch, and pasted them all into my Atom editor. I also configured version control.

From those four tutorials, I installed necessary packages and learned how to create a blank website. I also learned important commands.
* `bundle exec jekyll serve` builds the website and stores static content in `_site` folder, and produces a link containing a preview of the website.
* `jekyll build` only builds the website and store information in the folder.


Now that I can work on Atom and just push commits to GitHub, I decided to play around with the files. Because the files are supposed to create a site just like this [demo website](https://thomasvaeth.github.io/trophy-jekyll/), I compared it with code side by side to figure out what different components. Here are things I discovered:
* `_posts` folder is where I put all the blog posts. The posts are Markdown files, and each contains YAML Front Matter, where I type Title, Date, Category, and more. This is just like making a new post on Wordpress of Medium.
* `_sass` folder contains all the visual components like font style and  background color. This folder might not be in some of the themes, but the styles are always stored in `.scss` files.
* `_config.yml` is where I can define many things: Email, social links, defualt image, etc. Things I can customize differ theme by theme.
* `Gemfile` contains important plugins for the website. One important plugin for GitHub pages is `gem 'github-pages', group: :jekyll_plugins`, which allows us to see if the website will properly run in GitHub pages.
* `_includes` folder and `_layouts` kind of go hand-in-hand, and they contain main contents of the website. The files can be customized to change how things are displayed, such as buttons, images, etc.

In fact, I discovered that GitHub pages restricts what plugins can be rendered. Here's a [website](https://pages.github.com/versions/) that shows all whitelisted plugins.

This theme had a plugin called `octopress-autoprefixer` which is not allowed in GitHub pages, so I simply got rid of it. Fortunately, nothing changed visually in the website. It is important to first check whether the website can be deployed on GitHub pages before customizing things.

So this is the base I created for myself. Now that the website properly runs on GitHub with the link 'https://sangwookcheon.github.io', I decided to play around with files. I added 'About Me' and 'Projects' tab, changed font and color scheme, added Particles.js, and much more. I published all of this on [GitHub](https://github.com/SangwookCheon/sangwookcheon.github.io).

![](/in-post-images/Screen Shot 2019-07-11 at 16.34.00.png)
Here you can play around with the animation. Refer to its GitHub repository to install it on your own website.

I have to be clear: I was 2% familiar with CSS and Javascript, and maybe 20% proficient in HTML. But I didn't hesitate changing the website because I had my 'secret' weapon: Google. One of the sites I visited most is undoubtedly Stack Overflow.

I encountered a lot of unexpected problems due to my lack of experience in web development, and I wrote down major ones on my [GitHub repository](https://github.com/SangwookCheon/sangwookcheon.github.io).

Building Jekyll website was one of the things I had to struggle with to be proficient. I had to have certain level of knowledge to confidently and safely tinker with code located deep down in the file tree. And I think getting this foundation is the hardest, or the most boring process.

Data Science should be no different, as I must be able to clean and process any kind of data so that I can develop models. Looking ahead, imagining myself standing over the uphill, now I am going to go ahead and set the foundation. 
