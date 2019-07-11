# sangwookcheon.github.io - SWC :tada:
**N(one)-Stop Data Science** - Personal Data Science portfolio made with Jekyll using [Trophy theme](https://github.com/thomasvaeth/trophy-jekyll).

[GitHub](https://github.com/SangwookCheon)
[Kaggle](https://www.kaggle.com/sangwookchn)
[Medium](https://medium.com/@sangwookcheon)
[LinkedIn](https://www.linkedin.com/in/sangwookcheon/)

I am a high school student at Jakarta Intercultural School, passionate about Data Science, computers, and undiscovered gems hidden in the sea of information. I mainly program in Python and use statistics, data analysis, machine learning and other tools to work with data.

Through making this website, I learned that I don't have to be a professional web developer to make a customized portfolio for myself. My main focus is Python and Data Science, so I did not have much background in HTML, CSS, and Javascript. But I wanted to make a portfolio that suits my taste, instead of creating one on Wordpress or Wix, so I decided to jump right in.

Learning how Jekyll website works was quite a challenge, but once I got hang of it, I was  able to dig into each and every file to see what I can customize.

# Features
This portfolio is built on top of [Trophy theme](https://github.com/thomasvaeth/trophy-jekyll). Adding on to its basic structure, I added multiple extra features:
* Changing the layout of the image of each post
  - Full-page image and centered title and descriptions
  - Addition of Particles.js onto the image
* Adding "About Me" tab introducing myself
* Adding [Disqus](www.disqus.com) comment section section below each blog post
* Changing color scheme of the pages
* Addition of custom Google Fonts


# Usage
First, as my website is based on Trophy theme, you can check out its GitHub page for instructions if you want to start building from scratch. Instructions for changing `_config.yml` is written in that page.

You can surely implement some of the features I added by downloading or cloning this project.

These are things I need to point out:
### Adding Posts

To add a blog post, create a new Markdown file under  `_posts` folder, and include YAML Front Matter.

Sample Front Matter:
```yaml
---
layout: post
comments: true
title: "My First Post"
date: 2019-07-09
categories:
  - Personal
description: My first post
image: /images/chalkboard_leaves.jpg
---
```
* Layout should always be 'post'
* If `comments: false`, then Disqus will not show up for the post. If `comments: true` or didn't include the line at all, Disqus will show up by default.
* Format of the date: year-month-date
* You can specify multiple categories. Each category is case-sensitive, and should match with the `title` of the category specified under `_categories` folder.

Below Front Matter, add post content in Markdown.

### Adding Categories

Under `_categories` folder, add html file with the category name, and add Front Matter like this:
```YAML
  ---
  layout: default
  title: Personal
  description:
  permalink: /category/personal/
  ---
  {% include category.html %}
```

### Changing Particles.js animation

You can customize Particles.js animation, such as adding more particles and changing color, by changing `particles.json` file located in `/assets/js/`. Go to [this website](https://vincentgarreau.com/particles.js/#default) to play around with different variables, and go to its GitHub repository to learn how to edit `particles.json`.

In `particle_style.md`, I added some styles that I like.

### Changing "About Me" section

You can change 'About Me' section by editing content inside `about-tab.html`. `about_me.md` is just for me to write content first and transfer it into html file.

### Using disqus

When using disqus, you need to change shortname to your own. Please refer to Disqus website to learn more about Disqus in Jekyll. The path to file is `/_includes/disqus.html`.

# Problems encountered
### 1. Category page
Trophy theme is originally not 100% supported by Github pages, most likely because of a plugin that is not [whitelisted by Github](https://pages.github.com/versions/), which is called **"octopress-autoprefixer"**. Simply getting rid of the plugin itself didn't solve the problem, but it didn't have any effect on the webpage and significantly reduced website deployment time (from command `bundle exec jekyll serve`), so I decided to keep it removed.

Now, the main problem from the original theme was that individual card referencing each category in the **"Categories"** page and tab did not work when clicked. Every card produced "404 file not found error." Strangely, everything worked fine when the webpage was deployed locally using local server. Hence, I reasoned that this is a problem specific to Github Pages platform.

I probably spent more than 10 hours trying to solve this problem by digging through all the files, html files, Gemfile, _config.yml, files in _layouts, etc. However, I was not able to find a viable solution. Then, I came across this amazing [issue on GitHub](https://github.com/thomasvaeth/trophy-jekyll/issues/23) which gave me the answer I was looking for.

Following instructions, I added _categories folder where I manually define each category in html format.

### 2. Rendering Particles.js on Jekyll
Using Particles.js is supposed to be relatively easy, but I faced a big challenge.

I first thought Javascript itself is not supported on Jekyll and GitHub pages because they are meant to render static properties. However, the animation was correctly rendered in my local machine while it didn't when published to GitHub. Then I came across this [repository](https://github.com/nrandecker/particle) where Particles.js actually worked, so I compared my implementation side by side and finally caught the error. I had to properly set the full path of particles.json file.

### 3. Getting Disqus to work (small error)
Disqus provides Universal Code that I can just copy and paste, but strangely the comment box didn't show up. After inspecting webpage on Google and FireFox, I noticed that a common error was produced: `Uncaught error: Unexpected end of input`, which means there's syntax error somewhere in Javascript code, most likely unexpected curly brackets. The real problem was the comment in Disqus Universal Code.

The comment `//DON'T EDIT BELOW THIS LINE` had to be removed because the interpreter interpreted this as a regular piece of code, not a comment.

# What I hope to do next:
First and foremost, I hope to populate my website with many posts related to Data Science.

Because I am still not proficient in web development, I was not able to add further styling to additional features. Here are things that I want to do if I have more skills:

- If possible, I want to add another tab named "Projects," where I  can put all my Data Science projects into one place, separated from regular blog posts. Each project will be one card with name and link to the project. Each project will have its own dedicated blog post.

- I want to put my profile image shown in "About Me" tab into the left pane, so that I can write text only in the right pane. This is surprisingly more challenging than I thought, because the styling structure is quite complicated, and it is hard to know how they are styled.
