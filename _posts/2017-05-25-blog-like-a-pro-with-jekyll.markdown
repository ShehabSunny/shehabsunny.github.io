---
layout: post
title:  "Blog like a pro with Jekyll"
subtitle:
date:   2017-05-25 00:00:01
categories: [tools]
---

## Writing a blog
There are many ways to write a blog. Some requires few mouse clicks and some drag and drops. Tools like Wordpress makes it extremely easy to create your own blog site. However, let's talk about something that requires more than just mouse clicks. It's called "Jekyll". According to the official website "Jekyll is a simple, blog-aware, static site generator". Let's get more into it.

\*I assume that you have knowledge about **git** and have a **Github** account. If you don't have a Github account then you can create one [here](http://github.com){:target="_blank"}, It's easy and free.

## What and Why?
Jekyll is a static site generator, it converts raw text files in various formats (like Markdown) and converts them into html files. It generates ready-to-publish static websites that can be served with a web server.

The Unique feature of Jekyll is that it happens to be the engine behind  [Github Pages](https://pages.github.com/). As a result we can easily host our blog/site on Github's server for free.

## Overview
First of all we need to install and setup Jekyll. Then create a new site, build it and push it into a github repository that has been created for our Github Page. Our site will be live at **http://[username].github.io**. We will also be able to use a custom domain of our own.

While writing a new blog post we will be creating a new markdown file in the project, write everything, build using a single command and push everything into the repository. That's all it takes to publish a new blog post. No login and nothing. Open a text editor, write a blog and simply 'git push'.

## Installing Jekyll
Jekyll has dependencies on Ruby, RubyGems, GCC and Make.

**Requirements:**
- GNU/Linux, Unix, or macOS
- Ruby version 2.0 or above, including all development headers
- RubyGems
- GCC and Make

#### Install Ruby and RubyGems:
We can install Ruby from [official website](https://www.ruby-lang.org/en/documentation/installation/){:target="_blank"}. Make sure the version is 2.0 or above. RubyGems should be included with Ruby. Run these commands to check version number.
{% highlight bash %}
$ ruby -v
ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-linux]
{% endhighlight %}
{% highlight bash %}
$ gem -v
2.6.11
{% endhighlight %}

#### Install GCC and Make:
Install GCC and Make from their official websites.
- [Download GCC](https://gcc.gnu.org/install/){:target="_blank"}
- [Download Make](https://www.gnu.org/software/make/){:target="_blank"}

Again, check installation with following commands
{% highlight bash %}
$ gcc -v
{% endhighlight %}
{% highlight bash %}
$ make -v
{% endhighlight %}

#### Install Jekyll:
Now that RubyGems is installed, use this command to install Jekyll on your computer.
{% highlight bash %} $ gem install jekyll bundler{% endhighlight %}

## Create new blog site
After installing Jekyll, let's create a new blog site. Execute the following command. This will create the boilerplate of our site.
{% highlight bash %} $ jekyll new my-awesome-site{% endhighlight %}
This command will create a new folder named 'my-awesome-site' in the current directory with all the files needed. We can now start writing blog posts.

A basic Jekyll site usually looks something like this:
{% highlight bash %}
.
├── _config.yml
├── _data
|   └── members.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.md
|   └── on-simplicity-in-technology.md
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
|   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
|   ├── _base.scss
|   └── _layout.scss
├── _site
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid YAML Frontmatter
{% endhighlight %}

**_config.yml** contains all the configurations for the site. For example, site title, description, url, excluded files, syntax highlight etc.

**_posts** folder will contain all our blog posts in different markdown files. Keep in mind that all the files should have ***YEAR-MONTH-DAY-title.MARKUP*** naming format.

**_site** folder will be generated when we build our site. It will contain all the files necessary for the site.
For more information on project structure visit [this link](https://jekyllrb.com/docs/structure/){:target="_blank"}.

#### Running on local machine
{% highlight bash %}
$ bundle exec jekyll serve
{% endhighlight %}
This command will transform all the markdown files into proper html files and also generate all the files into **_site** folder and run a local server on your machine.


## Writing your first blog post
Let's create a new markdown file in **_posts** folder with ***YEAR-MONTH-DAY-title.MARKUP*** naming format.

Any file that contains a YAML front matter block will be processed by Jekyll as a special file. The front matter must be the first thing in the file and must take the form of valid YAML set between triple-dashed lines.

A blog post needs to have the following front matter:

{% highlight bash %}
---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-05-26 02:05:27 +0600
categories: [awesome-category]
---
{% endhighlight %}

This block contains configs for this specific blog post, After the block we will write our blog post in markdown language.

\**Remember that Jekyll supports **Liquid template language**, so we can use it's tags in our posts.

If you have the server running then it will automatically regenerate when you change in a file. As a result you can see the changes by just refreshing the page in the browser.  

## Publish
### Building the site

Run this command to start building the project. It will generate everything into **_site** folder.
{% highlight bash %}
$ bundle exec jekyll build
{% endhighlight %}

Now you can take the everything from **_site** folder and host it on a server, that will work fine. However, the best part is using with Github so that We can simply commit and push to update our site.

### Free hosting with GitHub Pages

As I have mentioned earlier that we can host our site on Github for free. In order to accomplish that we need to login to our Github account and create a repository named ***[username].github.io***. Now come back and push our project to the master branch of the repository. It's that simple to publish our site with Github Pages. The site will be published at **https://[username].github.io**. If you want to change anything (like changing CSS, Html or write new Post) then do it on your local machine and push to the github repository. That will update the site instantly.


## Tips
Jekyll has a number of themes available at [this link](http://jekyllthemes.org/){:target="_blank"}

## References
- [https://jekyllrb.com](https://jekyllrb.com){:target="_blank"}
- [https://github.com](https://github.com){:target="_blank"}
- [https://pages.github.com/](https://pages.github.com/){:target="_blank"}
