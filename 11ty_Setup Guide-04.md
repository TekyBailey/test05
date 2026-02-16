# Setup Guide For An Eleventy Project


### What is Eleventy

Eleventy is a **static site generator**. It’s a smart tool that does all the hard work up front by taking content from either markdown files, JSON files or remote data sources (and much more). It then smooshes them together, providing us with an API to grab that content and build templates. It’s sort of like working on a dynamic CMS site, but instead of relying on the server to work hard on each request, we do all of that up front instead.

The end result is a static website, made up of mainly HTML pages in folders. These folders and HTML pages can be hosted anywhere that allows you to put HTML pages on the internet. This makes static site generators (referred to as **SSGs** throughout the rest of the course) incredibly useful.

### Before We Start

There are several tools you will need before we can install and setup Eleventy. They are:
  - Command Line Terminal (referred to as terminal)
	- Node.js installed
  - Code Editor or an IDE (Intergrated Development Environment)

#### Terminal
	All three of the main Operating Systems (OS) Windows, Mac & Linux alreadey have one  a terminal built into their OS. Open your computers Search-Bar and type in "terminal" and it should list the name of the terminal installed on your system.

#### Node.js
	To see if you have Node.js installed open your terminal and type in: node -v  (Return).
	If it shows a version number you have. If it is older than version 18 you will need to upgrade it to version 18 or higher.

#### Code Editor or an IDE
While Windows, Mac and Linux come with a basic text editor that you could use. Your user experience will be exceptionally better if you download/install a code editor. VScodium is a good choice and is availble for all three OSs. What makes this so great is that you can do a lot from within it. Some of its built-in features are:
- A file/folder manager
- A terminal
- An interface so you can interact with Git without using a terminal. Git has to be installed on your computer for this to work.


### Starting From Scratch
Eleventy does not come pre-configured out of the box. There are many things you need to install and configure to tailor Eleventy for your project. Some things can be ported to a new project and other settings discarded or modified.

From here on out I will not provide a detail description of what needs to be done or mention every step to follow to set up Eleventy. *I will expand this guide when I have the time.*

*Note: Folder and Directory mean the same thing. A place where you place all sorts of files and subfolders/subdirectories. Which these subfolders can contain more files and subfolders.

 1. Create folder: Websites
 This folder will be used to hold all of your projects/websites.

 2. Create subfolder inside folder /Websites: Site01
 Inside this Websites folder create another folder for the actual site you are creating.
 Short names are best with no spaces. Linux does not like spaces in filenames and since most internet servers run on linux it's best not to use them. Instead use a dash or underscore between filenames. For this guide let's call this folder "Site01".

 3. Launch VScodium and click on the File menu and select Open Folder and navigate to Websites/Site01 then click on Select Folder.

 4. Create a file in folder Site01: .gitignore
	In VScodium/Explorer Panel hover over Site01 folder and click the Create New File icon that pops-up. In the text window that opens type ".gitignore".

 5. Add the following to it:
 ```yaml
# Misc
*.log
npm-debug.*
*.scssc
*.swp
.DS_Store
.sass-cache
.env
.cache

# Node modules and output
node_modules
dist
src/_includes/css
```
***Note:*** *See lesson-19 Section-Letting Eleventy see our critical CSS for why `src/_includes/css` is here.*

 6. Create a file in folder Site01: .eleventy.js
 This is the configuration file for Eleventy and this is the most important dotfile in our setup. It delegates and instructs everything that makes up the final output.

 7. Add the following to it:
 ```javascript
 module.exports = (config) => {
	return {
		dir: {
			input: 'src',
			output: 'dist',
		},
	};
};
```

The main thing to focus on now is the line that has "dir:", the dir stands for directory and it will tell Eleventy where the input directory will be (src) that holds all the items that will be used to make your website. All the generated files for the website will be put into the output directory (dist). These are abbreviations for source (src) and distribution (dist). In computing a particular package of software ready for distribution to users.

 8. In terminal at folder /Site01 run command: npm init -y
Make sure you are in the Site01 folder. This will create a package.json file and by using the -y flag we told it to answer **yes** to all the questions it would normally ask.

 9. The package.json file should contain the following:
 ```json
 {
	"name": "Site01",
	"version": "1.0.0",
	"description": "",
	"main": ".eleventy.js",
	"scripts": {
		"test": "echo \"Error: no test specified\" && exit 1"
	},
	"keywords": [],
	"author": "",
	"license": "ISC",
  "type": "commonjs"
}
```

10. In terminal at folder /Site01 run: npm install @11ty/eleventy
This will install a local version of Eleventy to this folder. Specifically it installs the node_modules folder and the package-lock.json file.

11. Create a subfolder in folder /Site01: src
 The src folder will hold all the other folders and files to create our website.

12. Create a file in the folder /src: index.md
This will be used to create our home page. For now type into it "Hello, World". We told Eleventy to use the "src" folder in step 7 as the input folder for all the source material for our website.

13. In the terminal run command: npx eleventy --serve
This command will take the folders and files in the src folder, process them into HTML and output these files in the dist folder. It also spins a mini server and serves the files to the localhost at port 8080. Here is an example of what the command does:

```
PS D:\websites\Site02> npx eleventy --serve      
[11ty] Writing ./dist/index.html from ./src/index.md (liquid)
[11ty] Wrote 1 file in 0.12 seconds (v3.1.2)
[11ty] Watching…
[11ty] Server at http://localhost:8080/
```

*Notice also by default Eleventy uses the Liquid templating engine.*

14. Open file: package.json
Delete the lines that says ` "test": "echo \"Error: no test specified\" && exit 1" ` and replace it with `"start": "npx eleventy --serve"`

15. In the terminal run command: Ctrl+C
This will stop the node.js server

16. In the terminal run command: npm start
This will now do the same thing that `npx eleventy --serve` did in step 13. Here is an example of the output:
```
PS D:\websites\11ty Master Folder\Master-02> npm start

> master-02@1.0.0 start
> npx eleventy --serve

[11ty] Writing ./dist/index.html from ./src/index.md (liquid)
[11ty] Wrote 1 file in 0.18 seconds (v3.1.2)
[11ty] Watching…
[11ty] Server at http://localhost:8080/
```
Notice that the `start` command calls the`npx eleventy --serve` command.

17. (L3-Start using Nunjucks) Open file: .eleventy.json
Add the following 3 lines, right after the `return { statement:`
```js
markdownTemplateEngine: 'njk',
dataTemplateEngine: 'njk',
htmlTemplateEngine: 'njk',
```
With the code we’ve just added, we’re telling Eleventy that markdown files, data files and HTML files should be processed by Nunjucks. That means that we can now use `.html` files instead of having to use `.njk` files.

18. (L3) Create a folder in src/ called: _includes and inside src/_includes/ folder create another folder called: layouts
We are building our directory structure to put our files that in turn will be used to build the website.

19. (L3) Create a file in the folder src/_includes/layouts named: base.html
This will be the main layout template file used by all the other web page files. This file will contain all the `HTML` code that would be repeated in those files. Giving us one convenient location to make changes in one file that will be used on many different pages on our website.

20. (L3) Open file: base.html
Add the following code to it:
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>{{ title }}</title>
	</head>
	<body>
		{% block content %}{% endblock %}
	</body>
</html>
```
There are two placeholders here `{{ title }}` and `{% block content %}{% endblock %}` that will be used to insert data for the *title* and `HTML` code for the *content* from each individual webpage file into this base.html file. This will happen when Eleventy processes the the contents of the src folder and puts the resulting files into the dist folder.

21. (L3) Create a file in folder src/_includes/layouts: home.html
This will create a template for our home page.

22. (L3) Open file: home.html
Add the following HTML to it:
```html
{% extends "layouts/base.html" %}
{% block content %}
<article>
	<h1>{{ title }}</h1>
	{{ content | safe }}
</article>
{% endblock %}
```

23. (L3-Assign Template to Page) Open file: /src/index.md
Delete all its content and add this code to it:
```yml
---
title: 'Hello, world'
layout: 'layouts/home.html'
---
This is pretty _rad_, right?
```

24. (L4-Add Front Matter to Home Page) Open file: src/index.md
Add the following to the front matter after the layout variable.
```yaml
intro:
  eyebrow: 'Digital Marketing is our'
  main: 'Bread & Butter'
  summary: 'Let us help you create the perfect campaign with our multi-faceted team of talented creatives.'
  buttonText: 'See our work'
  buttonUrl: '/work'
  image: '/images/bg/toast.jpg'
  imageAlt: 'Buttered toasted white bread'
```

25. (L4) Open file: src/_includes/layouts/home.html
Replace the existing `<article>` tags with this code:
```html
<article class="intro">
	<div class="[ intro__header ] [ radius frame ]">
		<h1 class="[ intro__heading ] [ weight-normal text-400 md:text-600 ]">
			{{ intro.eyebrow }}
			<em class="text-800 md:text-900 lg:text-major weight-bold">{{ intro.main }}</em>
		</h1>
	</div>
	<div class="[ intro__content ] [ flow ]">
		<p class="intro__summary">{{ intro.summary }}</p>
		<a href="{{ intro.buttonUrl }}" class="button">{{ intro.buttonText }}</a>
	</div>
	<div class="[ intro__media ] [ radius dot-shadow ]">
		<img
			class="[ intro__image ] [ radius ]"
			src="{{ intro.image }}"
			alt="{{ intro.imageAlt }}"
		/>
	</div>
</article>
```

26. (L5-Add Passthrough) Open file: src/.eleventy.js
On the first line of the function right below ( `module.exports = (config) => {` ) add this line:
```js
// Set directories to pass through to the dist folder
config.addPassthroughCopy('./src/images/');
```
This tells Eleventy two things. Do *not* process the file in the images folder and copy the folder and all its contents to the dist folder.

27. (L6-Partials) Open file: src/_includes/layouts/base.html
Delete the `{% block content %}{% endblock %}` and replace it with this:
```html
<main tabindex="-1" id="main-content">
	{% block content %}{% endblock %}
</main>
```

28. (L6) Open file: src/_includes/layouts/base.html
Add this line **above** your <main> element:
```js
{% include "partials/site-head.html" %}
```
Whatever HTML code is in this file will be included on every webpage as the header block. One less chunck of code to update on every webpage when making a minor change or to fix an error.

29. (L6) Create a folder in the folder src/_includes: partials
This will hold snippets of code that can be inserted anywhere we need it.

30. (L6) Create a file in the folder src/_includes/partials: site-head.html
Add the following to it:
```html
<a class="[ skip-link ] [ button ]" href="#main-content">Skip to content</a>
<header role="banner" class="site-head">
	<div class="wrapper">
		<div class="site-head__inner">
			<a href="/" aria-label="Issue 33 - home" class="site-head__brand">
				{% include "partials/brand.svg" %}
			</a>
			<nav class="[ nav ] [ site-head__nav ] [ font-sans ]" aria-label="Primary">
				<ul class="nav__list">
					<li>
						<a href="/">Home</a>
					</li>
					<li>
						<a href="/work">Work</a>
					</li>
				</ul>
			</nav>
		</div>
	</div>
</header>
```
This will include any site titles, icons and naivigation links.

31. (L6) Create or add a site icon to the folder src/_includes/partials

32. (L7-Data Basics) Create folder in the folder src/: _data
This will hold any data contained in files such as JSON, JS or YAML.

33. (L7) Create a file in folder src/_data: site.json
A data file to hold any site wide data for our files. But *NO* comments.

34. (L7) Open the file site.json
Add to the following data to it:
```json
{
  "name": "Issue 33",
  "url": "https://issue33.com"
}
```
Now we have two pieces of data in our **site**.json file ("Issue 33" and "https://issue33.com") that are joined to two variables ("name" and "url") which now can be used by any of our files simply by combining the file name with the variable names ie. `{{ site.name }}` or `{{ site.url }}`. When Eleventy processes a file that has an object variable of `{{ site.name }}` it will replace it with the data `Issue 33`.

35. (L7) Open the file src/_includes/partials/site-head.html
Find the line of code `<a href="/" aria-label="Issue 33 - home" class="site-head__brand">` and replace it with this line of code: 
```html
<a href="/" aria-label="{{ site.name }} - home" class="site-head__brand">
```

36. (L7) Create file in folder src/_data: navigation.json
This will hold our navigation links for the site.

37. (L7) Open file src/_data/navigation.json
Add the following array of objects to it:
```json
{
	"items": [
		{
			"text": "Home",
			"url": "/"
		},
		{
			"text": "About",
			"url": "/about-us/"
		},
		{
			"text": "Work",
			"url": "/work/"
		},
		{
			"text": "Blog",
			"url": "/blog/"
		},
		{
			"text": "Contact",
			"url": "/contact/"
		}
	]
}
```
38. (L7) Open file src/_includes/partials/site-head.html
Delete the `<ul class="nav__list">` element, with all of its children, and add the following:
```html
<ul class="nav__list">
	{% for item in navigation.items %}
	<li>
		<a href="{{ item.url }}">{{ item.text }}</a>
	</li>
	{% endfor %}
</ul>
```

39. (L7) Create new file in folder src/_data: helpers.js
We will use this to highlight the navigation button for the current/active page we are on.

40. (L7) Open file src/_data/helpers.js
Add the following javascript code to it:
```js
module.exports = {
	/**
	 * Returns back some attributes based on whether the
	 * link is active or a parent of an active item
	 *
	 * @param {String} itemUrl The link in question
	 * @param {String} pageUrl The page context
	 * @returns {String} The attributes or empty
	 */
	getLinkActiveState(itemUrl, pageUrl) {
		let response = '';

		if (itemUrl === pageUrl) {
			response = ' aria-current="page"';
		}

		if (itemUrl.length > 1 && pageUrl.indexOf(itemUrl) === 0) {
			response += ' data-state="active"';
		}

		return response;
	},
};
```

41. (L7) Open file  src/_includes/partials/site-head.html
Delete the `<ul class="nav__list">` element and all of its children, and replace it with the following:
```html
<ul class="nav__list">
	{% for item in navigation.items %}
	<li>
		<a href="{{ item.url }}" {{ helpers.getLinkActiveState(item.url, page.url) | safe }}>
			{{ item.text }}
		</a>
	</li>
	{% endfor %}
</ul>
```

42. (L7-Cascading Data) This section of the lesson will be skipped for now.

43. (L8-Creating a Collection) This lesson will be skipped for now.

44. (L9-Adding Remote Data) This lesson will be skipped for now.

45. (L10-Home Page Complete) Open file src/index.md
Delete `title: 'Hellow World'` and add:
```yml
title: 'Issue 33'
```

46. (L10) Open file src/_includes/layouts/home.html
Add a HTML `<div class="wrapper"></div>` tag around the `<article class="intro"></article>` tag.

47. (L11-Blog Feeds, Tags and Pagination) Create file in src/_includes/partials: page-header.html
This will be an h1 tag for the page title and a page summary from our about.md file.
The rest of the lesson will be skipped for now.

**Note: See section on page-header.html**

48. (L11) Open file: page-header.html
Add the following code block to it:
```html
<div class="[ page-header ] [ bg-light-glare ]">
	<div class="[ wrapper ] [ flow ]">
		<h1 class="[ page-header__heading ] [ headline ]" data-highlight="primary">
			{{ pageHeaderTitle }}
		</h1>
		{% if pageHeaderSummary %}
		<div class="[ page-header__summary ] [ measure-long ]">
			{{ pageHeaderSummary | safe }}
		</div>
		{% endif %}
	</div>
</div>
```


49. (L12-Blog Post View, Directory Data and Fileter) In terminal run: npm install moment
The moment package is used to work with JavaScript dates. Skip the rest of the material for now.

**Note: Move this install to around Step 11.**

50. (L13-Recommended Content) This lesson will be skipped for now.

51. (L14-Add About Page Layout) Create file in src/_includes/layouts: about.html

52. (L14-Add About Page Layout) Don't use the layout presented here.
It is tied to closely with the site the author is building. Instead use this modified layout:
```html
{% extends "layouts/base.html" %} 

{% set pageHeaderTitle = title %} 
{% set pageHeaderSummary = content %} 

{% block content %}
  <article>
    {% include "partials/page-header.html" %} 
   </article>
{% endblock %}
```
**Note: Is a uniquie layout needed for each individual page?**

53. (L14-Add About Page) Create file in folder src/: about.md
This file will add content to the about.html layout.

54. (14-Add About Page) Open file: about.md
Add the following to it:
```yml
---
title: 'About Issue 33'
layout: 'layouts/about.html'
---

Wanna see our foosball table? Nah, only kidding. We’re a made-up
agency being used as an example for the Piccalilli course,
[Learn Eleventy From Scratch](https://learneleventyfromscratch.com).
```


55. (L15-Add a Work Landing Page) Skip this lesson for now.
56. (L16-Creating Work Item Page) Skip this lesson for now.
57. (L17-Meta Info and RSS Feeds) Skip this lesson for now.
58. (L18-Setting Up Gulp) Skip this lesson for now.

59. (L19-Setting Up Sass) Skip this lesson for now.
**Note:** The *CUBE CSS* framework/methodology is used in these lessons. 

***Note:** See Step-5 .gitignore for why `src/_includes/css` is there.*

**Note:** there is a CSS reset file shown and used. This is a slightly modified version of this CSS reset that I created. You can read about it in my article, A modern CSS reset (https://hankchizljaw.com/wrote/a-modern-css-reset/).

**TIP**
You might have noticed that the file is called _reset.scss, rather than reset.scss. This is because Sass will ignore files with a _ prefix unless they are imported with @import.

60. (L19-Update NPM Scripts) **Skip for now.** Open file: src/package.json
This is going to modify the *start* command and add the *productiion* command.
61. (L20-Setting Up Fonts) Skip this lesson for now.
62. (L21-Setting Up Images) Skip this lesson for now.
Uses Gulp to shrink any image in the src/images folder and place them in the dist/images folder.
63. (L22_Global CSS and Design Tokens) Skip this lesson for now.
64. (L23-Styling Global Blocks) Skip this lesson for now.
65. (L24-Styling The Skip Link) Skip this lesson for now.
Styles the sites buttons and hides the skip link at the very top of each page.
66. (L25-Home Page Intro) Skip this lesson for now.
67. (L26-Home Page Panels) Skip this lesson for now.
68. (L27-Styling The Blog) Skip this lesson for now.
69. (L28-Styling The About Page) Skip this lesson for now.

70. (L29-Add A Contact Page) Skip this lesson for now.
**Note:** Move this lesson up to the front where you create other pages.

71. (L30-Style The Work Section) Skip this lesson for now.
72. (L32-Wrapping Up) Skip this lesson for now.
- Adds social image and favicon.
- Minifies the HTML output files in production.
- Test your production build.
