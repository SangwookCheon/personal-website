# sangwookcheon.github.io
One-stop website for non-stop learning - Personal portfolio made with Jekyll using [Trophy theme](https://github.com/thomasvaeth/trophy-jekyll).

Through making this website, I learned that I don't have to be a professional web developer to make a customized portfolio for myself. My main focus is Python and Data Science, so I did not have much background in HTML, CSS, and Javascript. But I wanted to make a portfolio that suits my taste, instead of creating one on Wordpress or Wix, so I decided to jump in.

Learning how Jekyll website works was quite a challenge, but once I got hang of it, I was able to steer myself into digging into each and every file to see what I can customize.


# Features
This portfolio is built on top of [Trophy theme](https://github.com/thomasvaeth/trophy-jekyll). Adding on to its basic structure, I customized many of the features:
* Changing the layout of the image of each post
  - Full-page image and centered title and descriptions
  - Addition of Particles.js onto the image
* Adding "About Me" tab introducing myself
* Changing color scheme of the pages
* Addition of custom Google Fonts

# Problems encountered
### 1. Category page
Trophy theme is originally not 100% supported by Github pages, most likely because of a plugin that is not [whitelisted by Github](https://pages.github.com/versions/), which is called **"octopress-autoprefixer"**. Simply getting rid of the plugin itself didn't solve the problem, but it didn't have any effect on the webpage and significantly reduced website deployment time (from command `bundle exec jekyll serve`), so I decided to keep it removed.

Now, the main problem from the original theme was that individual card referencing each category in the **"Categories"** page and tab did not work when clicked. Every card produced "404 file not found error." Strangely, everything worked fine when the webpage was deployed locally using local server. Hence, I reasoned that this is a problem specific to Github Pages platform.

I probably spent more than 10 hours trying to solve this problem by digging through all the files, html files, Gemfile, _config.yml, files in _layouts, etc. However, I was not able to find a viable solution. Then, I came across this amazing [issue on GitHub](https://github.com/thomasvaeth/trophy-jekyll/issues/23) which gave me the answer I was looking for.

### 2. Rendering Particles.js on Jekyll

# Usage


# What I hope to do next:
If possible, I want to add another tab named "Projects," where I  can put all my Data Science projects into one place, separated from regular blog posts. Each project will be one card with name and link to the project. Each project will have its own dedicated blog post.

I want to put my profile image shown in "About Me" tab into the left pane, so that I can write text only in the right pane. This is surprisingly more challenging than I thought, because the styling structure is quite complicated, and it is hard to know how they are styled
