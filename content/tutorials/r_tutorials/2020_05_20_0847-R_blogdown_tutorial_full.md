---
date: "2020-05-20T08:47:00+01:00"
draft: false
linktitle: Full Tutorial
menu:
  r_tutorials:
    parent: R Blogdown
    weight: 2
title: Full Tutorial
toc: true
type: docs
weight: 2019052002
---

My experience going through R blogdown install.
https://bookdown.org/yihui/blogdown/software-info.html

Blogdown = static website generator in R, based on Hugo
<br>

## 1.1 installation

<pre>
install.packages('blogdown')		      # install blogdown

blogdown::install_hugo()              # install Hugo
	macOS installs using homebrew
	The latest Hugo version is 0.71.0
	Updated 1 tap (homebrew/core).
	==> New Formulae
	...
	...		
	Bash completion has been installed to:
	  /usr/local/etc/bash_completion.d
	==> Summary
	🍺  /usr/local/Cellar/hugo/0.71.0: 42 files, 74.4MB		

blogdown::update_hugo()
blogdown::hugo_version()
</pre>
<br>

## 1.2 quick start

<pre>
File > New Project > New Directory > Website using Blogdown > Create Project
specify directory name (e.g. blogdown_test)
creates blogdown_test/
                       .Rproj.user/
                       blogdown_test.Rproj
                       config.toml
                       content/
                       index.Rmd
                       resources/
                       static/
                       themes/

or type blogdown::new_site() in project directory
</pre>

### LiveReload
- website auto-rebuild when any source file modified and saved
- use blogdown::serve_site()

### config.toml
- basic configuration settings

### content/
- put R Markdown or Markdown source files for posts/ pages
- can have any directory structure

### public/
- publishing directory
- website automatically generated to this directory
- \*.html, \*.css, \*.js files and images
- upload this to github pages

## 1.3 RStudio add-ins

<pre>
Addins >
Serve Site		calls blogdown::serve_site()
New Post		calls blogdown::new_post()
				opens dialog box to enter metadata for blog post
				can also generate normal page
				scans categories and tags of existing posts
				opens markdown file for creating new content
Update Metadata	update YAML metadata of currently opened post
				edit metadata if you closed it
Insert Image	copies image into project directory
				adds markdown/HTML code to embed the image
</pre>

### check project = website

<pre>
Tool > Project Options > Build Tools > Project build tools = Website

! uncheck "Preview site after building"
	  and "Re-knit current preview when supporting files change"
</pre>

### conclusion:
if publishing manually,
- do NOT use serve_site(),
- instead use build_site() before uploading public/ to github

if just previewing before publishing,
- then use serve_site()

### build website

RStudio > Build tab > Build Website (calls blogdown::build_site())

if publishing public/ files manually (nah, better to use Netlify)
<br>

every time before publishing website
- restart R session
- click Build Website
- (instead of publishing public/ continuously with blogdown::serve_site(),
    - blogdown::serve_site() calls blogdown::build_site(local = TRUE),
    - we want blogdown::build_site(local = FALSE))

<pre>
			==> rmarkdown::render_site(encoding = 'UTF-8')

			Rendering content/post/2015-07-23-r-rmarkdown.Rmd
			Building sites … 
							   | EN  
			-------------------+-----
			  Pages            | 20  
			  Paginator pages  |  0  
			  Non-page files   |  0  
			  Static files     | 12  
			  Processed images |  0  
			  Aliases          |  0  
			  Sitemaps         |  1  
			  Cleaned          |  0  

			Total in 26 ms

			Output created: public/index.html
</pre>

public/ is default
- can change it with publishDir = "myNewDirectory" in config.toml
<br>

set "relativeurls" to true in config.toml to view in browser
- or else website can only be previewed in RStudio viewer
- (CSS and Javascript links won't work)

## 1.4 global options that affect blogdown

|Option             |name         |Default	Meaning               |
|-------------------|-------------|-------------------------------|
|servr.daemon		    |interactive()|Use a daemonized server?       |
|blogdown.author		|				      |The default author of new posts|
|blogdown.ext		    |.md				  |Default extension of new posts |
|blogdown.subdir		|post			    |A subdirectory under content/  |
|blogdown.yaml.empty|TRUE			    |Preserve empty fields in YAML? |

- set global options with options(name = value)
- sets some defaults for e.g. author, ext
- so when you do New Post, some fields auto populate

<pre>
file.edit('/Users/minghan/Google Drive/RBlogDown/blogdown_test/.Rprofile')
	
	options(servr.daemon = 'interactive()',
			blogdown.author = 'Ming Han',
			blogdown.ext = '.Rmd',
			blogdown.subdir = 'post',
			blogdown.yaml.empty = TRUE)
</pre>

- or create .Rprofile at home dir (~/)
- R reads project .Rprofile first, but eventually settles with what's in ~/.Rprofile
<br>

## 1.5 .rmd vs .Rmarkdown vs .md

|Feature			|.Rmd	  |.Rmarkdown	|.md  |
|-------------|-------|-----------|-----|
|Run R code		|yes		|yes			  |no   |
|Bibliography	|yes		|no			    |no   |
|Task list		|maybe  |	yes			  |yes  |
|MathJax			|yes		|maybe		  |maybe|
|HTML widgets	|yes		|no			    |no   |

- .Rmd most complete, but heavier
- more info at https://bookdown.org/yihui/blogdown/output-format.html
<br>

- .md rendered to HTML by Blackfriday
- .RMarkdown compiled using rmarkdown and Pandoc
	- see Pandoc and bookdown doc for all features (https://blogdown-demo.rbind.io)
- blogdown supports LaTex render to HTML, but needs `$math$` (backticks)


### r markdown output format

	...
	output:
		blogdown:html_page:
			toc: true
			fig_width: 6
			dev: "svg"				<-- device for plots
	...

- blogdown:html_page inherits from bookdown::html_document2 inherits from rmarkdown::html_document
- so check ?markdown::html_document for details

### setting blogdown:html_page() options globally

- create _output.yml file in root directory

<pre>
	blogdown::html_page:
	  toc: true
	  fig_width: 6
	  dev: "svg"
</pre>

### make blogdown ignore a .Rmd file when compiling

- rename it to an unknown extension e.g. .Rmdignore

### 1.6 other Hugo themes

http://themes.gohugo.io
- not all themes work well with blogdown

### install theme

<pre>
blogdown::install_theme() with github username and repo name
	
	OR
	
create new site, pass github repo name to theme in new_site() <-- preferred
- automatically creates config.toml file

	blogdown::new_site(theme = 'gcushen/hugo-academic')
</pre>

### themes that work well with blogdown

	Simple/minimal themes: 	XMin, https://github.com/yihui/hugo-xmin
	no navbars...			Tanka, https://github.com/nanxstats/hugo-tanka
							simple-a, https://github.com/AlexFinn/simple-a
							ghostwriter, https://github.com/roryg/ghostwriter
	
									 ____ pretty good
									|
	Sophisticated themes: 	hugo-academic (strongly recommended for users in academia), https://github.com/gcushen/hugo-academic
							hugo-tranquilpeak-theme, https://github.com/kakawait/hugo-tranquilpeak-theme
							hugo-creative-portfolio-theme, https://github.com/kishaningithub/hugo-creative-portfolio-theme
							hugo-universal-theme, https://github.com/devcows/hugo-universal-theme

## 1.7 recommended workflow

1. pick a theme

	- pick a theme from http://themes.gohugo.io
	- find link to theme repo on github (https://github.com/user/repo)
	
	- create new project in RStudio
		blogdown::new_site(theme = 'user/repo')
	
	- edit options of config.toml to customize
	
2. edit website

	- preview site with Addins > Serve Site
	- (never click knit button)
	
	- upload public/ folder to your github repo
	- easier with Netlify, continuous deployment instead of manually uploading to github

## 2.1 static sites with Hugo

static sites
- HTML files
- same content no matter who visits the page

dynamic sites
- server side language that sends different content to different users
- e.g. PHP
- e.g. web forum, user login, server fetches user data from a database
	renders different content for different users

static site generators
- Hugo, Jekyll, Hexo

advantage of Hugo
- easy install
- fast rendering
- large community

blogdown = Hugo with Rmarkdown


### blogdown default folder structure
	
	config.toml
	content/
	static/
	themes/
	layouts/

## 2.2 configuration
- config.toml
- sets global configuration for entire site

Hugo
- first searches for config.toml (Tom's Obvious Minimal Language), 
- then config.yaml (Yet Another Markup Language)
- Hugo prefers .toml, but can also use .yaml

for blogdown
- use TOML for config file
- use YAML for data format for metadata

## 2.2.1 TOML syntax

|argument   |style                 |
|-----------|----------------------|
|key = value|char strings in quotes|
|boolean    |in bare lowercase     |

  e.g.  title = "My Awesome Site"
        relativeURLs = true

table in TOML
  e.g.	[social]
        github = "https://github.com/rstudio/blogdown"
        twitter = "https://twitter.com/rstudio"

### table used to fill variables in site's template

array of tables in TOML

  e.g.	[[menu.main]]
  			name = "Blog"
  			url = "/blog/"
  		
  		[[menu.main]]
  			name = "Categories"
  			url = "/categories/"
  
  		[[menu.main]]
  			name = "About"
  			url = "/about/"

- Hugo interprets array of tables as MENU
- if in config.toml, will generate links to Blog, Categories and About in site's main menu
- menu names defined here, location and style of menu specified elsewhere

## 2.2.2 options

- defaults at https://gohugo.io/overview/configuration/
- all can be changed except contentDir, hard-coded to "content"

  baseURL				URL for your site
  enableEmoji			true or false, e.g. :smile: in markdown

### permalinks
rules to generate permanent link

  e.g.	
  [permalinks]
		posts = "/:year/:month/:day/:title/"
	
	blogdown recommend using :slug instead of :title
	
	[permalinks]
		posts = "/:year/:month/:day/:slug/"

		SLUG:
			- simply an unique character string used to identify a specific post
			- slug does not change even if post title changes
			
		- post title may change, but want link to post to stay permanent
		- :slug falls back to :title if field name "slug" not set in YAML metadata of the post
		- set a fixed :slug, so link to post is fixed, and can update title
		- e.g. 	slug = "blogdown-love"
				old title = "I love blogdown"
				new title = "why blogdown is the best"
		
	e.g.
	[permalinks]
		post = "/:year/:month/:day/:slug/"

		- contents under posts will have new URL structure
		- content/posts/sample-entry.md with date: 2017-02-27T19:20:00-05:00
			will render to public/2017/02/sample-entry/index.html at build time
		- { more for organizing blogs }

	https://gohugo.io/content-management/urls/
	permalink config values

		:year			the 4-digit year
		:month			the 2-digit month
		:monthname		the name of the month
		:day			the 2-digit day
		:weekday		the 1-digit day of the week (Sunday = 0)
		:weekdayname	the name of the day of the week
		:yearday		the 1- to 3-digit day of the year
		:section		the content’s section
		:sections		the content’s sections hierarchy
		:title			the content’s title
		:slug			the content’s slug (or title if no slug is provided in the front matter)
		:filename		the content’s filename (without extension)

<pre>
publishDir      can set other than public/
theme           directory name under themes/
ignoreFiles			list of filename pattern (regex) for Hugo to ignore
                specify at least these
						["\\.Rmd$",				blogdown will render .Rmd files into .html files, no need for Hugo to build .Rmd files
						 "\\.Rmarkdown$",		
						 "_files$",				_files and _cache only contains temp files during blogdown .Rmd compilation
						 "_cache$"]

uglyURLs			Hugo generates "clean" URLs by default
						for foo.md, generates foo/index.html
					enable uglyURLs to have strict mapping from foo.md to http://www.example.com/foo/foo.html

hasCJKLanguage		Chinese, Japanese, Korean
					set to true if using those langugages

arbitrary options

e.g.	[params]
			author = "Ming Han"
			dateFormat = "2018_04_24"

	- avoids hard coding Hugo themes,
	- user change theme by editing config file
</pre>


## 2.3 content

- can have arbitrary directory structure in content/
- common one is:

	├── _index.md
	├── about.md
	├── vitae.md
	├── post/
	│   ├── 2017-01-01-foo.md
	│   ├── 2017-01-02-bar.md
	│   └── ...
	└── ...


## 2.3.1 YAML metadata

- each page should start with YAML metadata

	title
	date
	author
	categories
	tags
	etc...
	
	draft = true		posts not rendered with build_site() or hugo_build(), only renders in preview with Addins > Serve Site
	publishdate	=		specify future publish date, again still renders in preview
	weight = 			if 2 posts have same date, use weight to order them
	slug = 				char string as tail of URL


## 2.3.2 body

## 2.3.3 shortcode

- Hugo automatically generates full HTML code based on a shortcode
- e.g. embed twitter card

	<blockquote class="twitter-tweet">
	  <p lang="en" dir="ltr">Anyone know of an R package for
		interfacing with Alexa Skills?
		<a href="https://twitter.com/thosjleeper">@thosjleeper</a>
		...
	  </p>
	  &mdash; Jeff Leek (@jtleek)
	  <a href="https://twitter.com/jtleek/status/852205086956818432">
		April 12, 2017
	  </a>
	</blockquote>
	<script async src="//platform.twitter.com/widgets.js" charset="utf-8">
	</script>

	becomes
	
	{{< tweet 852205086956818432 >}}						in .md
	blogdown::shortcode('tweet', '852205086956818432')		in .Rmd

- see https://gohugo.io/extras/shortcodes/ for more



## 2.4 themes

https://themes.gohugo.io

e.g. hugo-lithium

config.toml

	baseurl = "/"												get from github, don't forget trailing /
	relativeurls = false										optional, true only if viewing website locally (blogdown::serve_site() provides local server to preview site locally)
	languageCode = "en-us"										
	title = "A Hugo website"									
	theme = "hugo-lithium"										directory name of theme
	googleAnalytics = ""										can sign up at https://analytics.google.com to obtain a tracking ID (tracks visits etc...)
	disqusShortname = ""										https://disqus.com, to enable commenting on your site
	ignoreFiles = ["\\.Rmd$", 
				   "\\.Rmarkdown", 
				   "_files$", 
				   "_cache$"]

	[permalinks]
		post = "/:year/:month/:day/:slug/"

	[[menu.main]]
		name = "Home"
		url = "/"
		weight = 1											set weight to order menu items
	[[menu.main]]
		name = "About"
		url = "/about/"
		weight = 2
	[[menu.main]]
		name = "GitHub"
		url = "https://github.com/rstudio/blogdown"
		weight = 3
	[[menu.main]]
		name = "CV"
		url = "/vitae/"										need vitae.md in content/ to generate /vitae/index.html
		weight = 4
	[[menu.main]]
		name = "Twitter"
		url = "https://twitter.com/rstudio"
		weight = 5

	[params]												misc parameters
		description = "A website built through 
					   Hugo and blogdown."

		highlightjsVersion = "9.12.0"						https://highlightjs.org/static/demo/
		highlightjsCDN = "//cdnjs.cloudflare.com/ajax/libs"
		highlightjsLang = ["r", "yaml"]
		highlightjsTheme = "github"

		MathJaxCDN = "//cdnjs.cloudflare.com/ajax/libs"
		MathJaxVersion = "2.7.5"

	[params.logo]											default image logo.png in static/ is used
		url = "logo.png"
		width = 50
		height = 50
		alt = "Logo"



## 2.5 templates

theme = templates + web assets

web assets = CSS, JS files, images, videos
- assets determine appearance + functionality of website + embedded content


## 2.5.1 XMin Hugo theme example

- tree view

		hugo-xmin/
		├── LICENSE.md
		├── README.md
		├── archetypes
		│   └── default.md
		├── layouts								HTML templates
		│   ├── 404.html
		│   ├── _default						
		│   │   ├── list.html					renders a list of pages e.g. blogs
		│   │   ├── single.html					render a single page
		
					{{ partial "header.html" . }}										{{ }} Hugo's variable and functions
						<div class="article-meta">
							<h1><span class="title">{{ .Title }}</span></h1>			title, author, date all provided in YAML metadata header of a page

							{{ with .Params.author }}
							<h2 class="author">{{ . }}</h2>
							{{ end }}

							{{ if .Params.date }}
							<h2 class="date">{{ .Date.Format "2006/01/02" }}</h2>
							{{ end }}
						</div>

					<main>
						{{ .Content }}
					</main>

					{{ partial "footer.html" . }}		
		
		│   │   └── terms.html					used to generate full list of categories or tags
		│   └── partials						HTML fragments reused by other templates via "partial" function
		│       ├── foot_custom.html
		│       ├── footer.html
		│       ├── head_custom.html
		│       └── header.html
		
					<!DOCTYPE html>
					<html lang="{{ .Site.LanguageCode }}">
						<head>
							<meta charset="utf-8">
							<title>{{ .Title }} | {{ .Site.Title }}</title>
							<link href='{{ "/css/style.css" | relURL }}'				link to .css files
								rel="stylesheet" />
							<link href='{{ "/css/fonts.css" | relURL }}'
								rel="stylesheet" />
							{{ partial "head_custom.html" . }}
						</head>

						<body>
							<nav>
								<ul class="menu">
									{{ range .Site.Menus.main }}
									<li><a href="{{ .URL | relURL }}">{{ .Name }}</a></li>
									{{ end }}
								</ul>
								<hr/>
							</nav>
						</body>
		
					- user defines menu in config.toml
															[[menu.main]]
																name = "Home"
																url = "/"
															[[menu.main]]
																name = "About"
																url = "/about/"
					- menu generated using header.html
															<ul class="menu">
																<li><a href="/">Home</a></li>
																<li><a href="/about/">About</a></li>
															</ul>					
		
		├── static								assets
		│   └── css
		│       ├── fonts.css
		│       └── style.css
		└── exampleSite
			├── config.toml
			├── content
			│   ├── _index.md
			│   ├── about.md
			│   ├── note
			│   │   ├── 2017-06-13-a-quick-note.md
			│   │   └── 2017-06-14-another-note.md
			│   └── post
			│       ├── 2015-07-23-lorem-ipsum.md
			│       └── 2016-02-14-hello-markdown.md
			├── layouts
			│   └── partials
			│       └── foot_custom.html
			└── public
				└── ...


## 2.5.2 implement more features

1. google analytics

	{{ template "_internal/google_analytics.html" . }}
	
	- can add to layouts/partials/foot_custom.html
	- configure googleAnalytics in config.toml
	- preview at https://deploy-preview-3--hugo-xmin.netlify.com
	- (https://github.com/yihui/hugo-xmin/pull/3 for details)

2. disqus comments

	{{ template "_internal/disqus.html" . }}
	
	- add to foot_custom.html
	- configure disqus shortname in config.toml
	- preview at https://deploy-preview-4--hugo-xmin.netlify.com
	- (https://github.com/yihui/hugo-xmin/pull/4 for details)

3. syntax highlighting via highlight.js
...
4. math expressions through MathJax
...

5. table of contents

	- if .Rmd, just add YAML metadata to top of .Rmd file
	
	output:
		blogdown:html_page:
			toc: true
			
	- if .md, have to modify single.html template, before content
	
		{{ if .Params.toc }}
		{{ .TableOfContents }}
		{{ end }}
	
	- https://github.com/yihui/hugo-xmin/pull/7 for an implementation with examples
	
6. display categories and tags in post
...
7. add pagination
...
8. add github edit button
...



## 2.6 custom layouts

		your-website/
		├── config.toml
		├── ...
		├── themes/
		│   └── hugo-xmin/
		│       ├── ...
		│       ├── layouts/
		│       |   ├── ...
		│       |   └── partials
		│       |       ├── foot_custom.html
		│       |       ├── footer.html
		│       |       ├── head_custom.html
		│       |       └── header.html
		|		└── static/
		|			├── style.css
		|			└── ...
		|
		├── layouts
		|	└── partials
		|		├── foot_custom.html
		|		└── head_custom.html
		└── static/
			├── style.css
			└── ...

- if customize a theme, future author updates may conflict with your tweaks


## customize in 2 ways

1. in config.toml (without touching template files)
2. use layouts/ to override templates in themes/

	e.g., the file layouts/partials/foot_custom.html, when provided, 
		  will override themes/hugo-xmin/layouts/partials/foot_custom.html



## 2.7 static files

- static/ stores web assets (images, CSS, JS files)
- all files in static/ copied to public/ when Hugo renders website		  


## 2 applications

1. make small changes to .css main file

e.g. static/style.css will override themes/static/style.css

		body {
		  font-family: "Comic Sans MS", cursive, sans-serif;
		}
		code {
		  font-family: "Courier New", Courier, monospace;
		}


2. render documents that are not blogdown::html_page()

e.g. .Rmd file generates PDF, 
- putting the .Rmd file in static/, blogdown will generate PDF,
- and Hugo will just copy PDF file into public/
- need to write custom "build" script R/build.R

	blogdown::build_dir("static")

- more info https://bookdown.org/yihui/blogdown/methods.html#methods	



## 3 deployment



## 3.1 netlify

- login to Netlify with github username to link Netlify with Github


## dealing with site updates
- host all source files (not just public/) in github repo
- Netlify create new sites from github repo
- Netlify supports Jekyll and Hugo (need to disable Jekyll only if hosting with github)

- Netlify
	Deploy settings
	repository			https://github.com/username/repo
	branch				master
	build command			hugo
	publish directory		public
	
- may need to create environment variable HUGO_VERSION and set to 0.20
	Build environment variables
	HUGO_VERSION		0.24.1	(or latest)	
	
- may take few minutes to deploy on Netlify for first time
- but later updates are incremental

auto-update
- now update github source repository and
- Netlify calls Hugo to render website automatically


## domain name
- custom domain name, need to configure some DNS records, point it to Netlify server
