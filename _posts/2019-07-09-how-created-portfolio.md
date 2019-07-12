---
layout: post
comments: true
title: "How I Created my own Personal Portfolio How I Created my ow"
date: 2019-07-09
categories:
  - Personal
description: In this post, I write about how I created my own Data Science portfolio.
image: /images/chalkboard_leaves.jpg
---

This is my portfolio.

About a week ago, I discovered GitHub pages for the first time. It seemed to provide a handy way to make websites out of any repository without getting help from website builders like Wordpress and Wix. Because I always felt these website builders are not my favorite, I decided to create a customized blog. So here I am, writing in this brand-new environment.

![](/in-post-images/Screen Shot 2019-07-11 at 16.00.06.png)
*The main page of GitHub pages. Making a customized website was more complicated than a scroll to the bottom.*

To briefly explain, this blog will be a one-stop website where I document any new Data Science skills, personal projects, and my thoughts. Just like any blogger, I want to keep track of what I do so that I will be able to roughly estimate what I did at a certain point in time.

Whenever we try to gain new skills, there is always a learning curve and a point where we say, "Aha, now I can confidently search on Google to dig 'deeper'." But to dig deeper, we need a shovel and a smoothened ground. To learn how to make a [Jekyll](https://jekyllrb.com/) website, which is an engine that powers GitHub pages, I had to go through a quite steep learning curve. The official website of GitHub pages said it will take only minutes to get it up and running. It was true, but I wanted to go further than just using one of the built-in themes.

And I am sure many of those who want personal websites are also like me.

I started by exploring [Jekyll themes](http://jekyllthemes.org/). Among hundreds of options, I chose [Trophy theme](http://jekyllthemes.org/themes/trophy/) which looked elegant and simple. Now, I faced a problem: How on earth should I go about using this theme?

![](/in-post-images/Screen Shot 2019-07-11 at 16.04.30.png)

GitHub pages documentation didn't help much. Instructions on Jekyll's official website were too new to me and confusing. What I learned is that I can create one blog using my GitHub username. Building websites for projects is another topic. I created new repository and named it `sangwookcheon.github.io`. Then I went to Settings, scrolled down to GitHub pages section, and built the website from the master branch. The name has to be formatted as `username.github.io` so that GitHub will automatically detect it.

First, I decided to look at video tutorials on creating Jekyll websites. I watched [four tutorials](https://www.youtube.com/watch?v=T1itpPvFWHI&list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB), and stopped right there. I went to Trophy theme's GitHub repository, downloaded all the files from the Master branch, and pasted them all into my Atom editor. I also configured version control.

From those four tutorials, I installed necessary packages and learned how to create a new website. I also learned some commands.
* `bundle exec jekyll serve` builds the website and stores static content in `_site` folder, and produces a link containing a preview of the website.
* `jekyll build` only builds the website and store information in the folder.

Now that I can work on Atom and just push commits to GitHub, I decided to play around with the files. Because the files are supposed to create a site just like this [demo website](https://thomasvaeth.github.io/trophy-jekyll/), I compared it with code side by side to figure out different components. Here are things I discovered:
* `_posts` folder is where I put all the blog posts. The posts are Markdown files, and each contains YAML Front Matter, where I type Title, Date, Category, and more. This is just like making a new post on Wordpress of Medium. I noticed that this is surprisingly simple compared to complicated-looking code files.
* `_sass` folder contains all the visual components like font style and  background color. This folder might not be in some of the themes, but the styles are always stored in `.scss` files.
* `_config.yml` is where I can define many things: Email, social links, default image, etc. Each theme provides different options to customize.
* `Gemfile` contains plugins for the website. One essential plugin for GitHub pages is `gem 'github-pages', group: :jekyll_plugins`, which allows us to see if the site will properly run in GitHub pages. When site is built, the plugin checks if the site will work not only on local machine, but also on GitHub pages platform.
* `_includes` folder and `_layouts` kind of go hand-in-hand, and they contain main contents of the site. The files can be customized to change how things are displayed, such as buttons, images, etc. The files can vary theme by theme. Digging through files is necessary to be able to edit them.

In fact, I discovered that GitHub pages restricts what plugins can be used. Here's a [website](https://pages.github.com/versions/) that shows all whitelisted plugins. If a theme has any unsupported plugin, it might run on a local server, but not when it is published.

This theme had a plugin called `octopress-autoprefixer` which is not allowed in GitHub pages, so I simply got rid of it. Fortunately, nothing changed visually on the website. You first need to check whether the website will work on GitHub pages before customizing things.

So this is the foundation I built. Now that the website properly runs on GitHub with the link 'https://sangwookcheon.github.io,' I decided to play around with files. I added 'About Me' and 'Projects' tab, changed font and color scheme, added Particles.js, and much more. I published all of this on [GitHub](https://github.com/SangwookCheon/sangwookcheon.github.io).

![](/in-post-images/Screen Shot 2019-07-11 at 16.34.00.png)
*Here you can play around with the animation. Refer to its GitHub repository to install it on your own website.*

To be clear, I was 2% familiar with CSS and Javascript, and maybe 5% proficient in HTML. But I didn't hesitate to change the website because I had my 'secret' weapon: Google. One of the sites I visited the most is undoubtedly Stack Overflow.

I was once quite skeptical about learning by searching, just because it seemed like a perfectly unorganized way of doing things. But for this project, I had no choice but to rely on Google. I noticed that when I gain knowledge by going through multiple sources to filter out bad ones, things seemed to stick in my brain much longer.

I encountered a lot of unexpected problems due to my lack of experience in web development, and I wrote down major ones on my [GitHub repository](https://github.com/SangwookCheon/sangwookcheon.github.io).

Building a Jekyll website was one of the things I had to struggle with to be proficient. I had to have a certain level of knowledge to confidently and safely tinker with code located deep down in the file tree. I think getting this foundation is the hardest or maybe the most tiresome process.

Data Science should be no different, as I must be able to clean and process any form of data so that I can develop models on top. Surfing on the web is an inevitable process awaiting in front of me. Looking ahead, now I am going to go ahead and set the foundation.
