---
date: "2020-05-20T08:47:00+01:00"
draft: false
linktitle: Blogdown Tutorial
menu:
  r_tutorials:
    parent: R Blogdown
    weight: 3
title: Blogdown Tutorial
toc: true
type: docs
weight: 2020052003
---

My experience going through R blogdown install by following Yihui's instructions at:<br>
[https://bookdown.org/yihui/blogdown/](https://bookdown.org/yihui/blogdown/)<br>
- the documentation there is superb, 
- but I had only beginner knowledge of HTML, CSS, and HUGO,
- took me a week to get my site published and start blogging,
- customizations took the longest, documented in **Highlights** section, hope you find some of it helpful

Blogdown = static website generator in R, based on Hugo<br>


## 1.1 installation

```
install.packages('blogdown')  # install blogdown

blogdown::install_hugo()  # install Hugo
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
blogdown::hugo_version()  # [1] ‘0.71.0’
```


## 1.2 a quick example 
(start new Blogdown R project/site)

### RStudio GUI way
```
File > New Project > New Directory > Website using Blogdown > |Create Project|
```

<img src="/img/tutorials/r_tutorials/image1.png" alt="create new Blogdown project" style="width:70%"/>

```
creates bioinfohippo/
                       .Rproj.user/
                       bioinfohippo.Rproj
                       config.toml
                       content/
                       index.Rmd
                       resources/
                       static/
                       themes/
```

### RStudio Console way
```
in project directory:
blogdown::new_site(theme = 'gcushen/hugo-academic') 
```

LiveReload
- use `blogdown::serve_site()`
- website auto-rebuild when any source file modified and saved

/config.toml
- basic configuration settings

/content/
- put R Markdown or Markdown source files for posts/ pages
- can have any directory structure

/public/
- publishing directory
- website automatically generated to this directory
- \*.html, \*.css, \*.js files and images
- if using github pages instead of Netlify -> upload /public/ to github pages


## 1.3 RStudio Addins
(GUI for various blogdown functions)

| Addins |  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Serve Site | calls blogdown::serve_site() |
| New Post | calls blogdown::new_post()<br>opens dialog box to enter metadata for blog post<br>can also generate normal page<br>scans categories and tags of existing posts<br>opens markdown file for creating new content |
| Update Metadata | update YAML metadata of currently opened post<br>edit metadata if you closed it |
| Insert Image | copies image into project directory<br>adds markdown/HTML code to embed the image |


### check project = website

```
Tool > Project Options > Build Tools > Project build tools = Website

! uncheck "Preview site after building"
	  and "Re-knit current preview when supporting files change"
```

### conclusion
if publishing manually on e.g. github pages,
- do NOT use `serve_site()`,
- instead use `build_site()` before uploading `public/` to github

if just previewing before publishing,
- then use `serve_site()`

## build_site()
- generating public/ with Blogdown
- for publishing public/ files manually on e.g. github pages (better to use Netlify - continuous deployment)

```
RStudio > Build tab > Build Website (calls blogdown::build_site())
```

every time before publishing website
- restart R session
- click Build Website

`build_site()` vs `serve_site()`
- `blogdown::serve_site()`
    - for previewing your site as you build it
    - builds public/ every time you make change to any file
    - calls `blogdown::build_site(local = TRUE)`
- `blogdown::build_site()`
    - builds public/ once
    - calls `blogdown::build_site(local = FALSE))`

```r
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
```

### `public/` is default
- can change it with publishDir = "myNewDirectory" in `config.toml`

### set "relativeurls" = true in `config.toml`
- to preview in browser
- or else website can only be previewed in RStudio viewer
- (CSS and Javascript links won't work without relativeurls)


## 1.4 global options
- sets some defaults for New Posts (e.g. author, extension)
- so when you do New Post, some fields auto populate

|Option             |name         |Default	Meaning               |
|-------------------|-------------|-------------------------------|
|servr.daemon		    |interactive()|Use a daemonized server?       |
|blogdown.author		|John Doe     |The default author of new posts|
|blogdown.ext		    |.md				  |Default extension of new posts |
|blogdown.subdir		|post			    |A subdirectory under content/  |
|blogdown.yaml.empty|TRUE			    |Preserve empty fields in YAML? |

project specific .Rprofile
```r
file.edit("/Users/minghan/Google Drive/RBlogDown/bioinfohippo/.Rprofile")
	
	options(servr.daemon = 'interactive()',
			blogdown.author = 'Ming Han',
			blogdown.ext = '.Rmd',
			blogdown.subdir = 'post',
			blogdown.yaml.empty = TRUE)
```

system wide .Rprofile
- create .Rprofile in home dir (~/)
- R reads project .Rprofile first, but eventually settles with what's in system wide ~/.Rprofile


## 1.5 .Rmd vs .Rmarkdown vs .md

|Feature			|.Rmd	  |.Rmarkdown	|.md  |
|-------------|-------|-----------|-----|
|Run R code		|yes		|yes			  |no   |
|Bibliography	|yes		|no			    |no   |
|Task list		|maybe  |	yes			  |yes  |
|MathJax			|yes		|maybe		  |maybe|
|HTML widgets	|yes		|no			    |no   |

.Rmd most complete, but takes a little longer to render compared to .md
- more info at [https://bookdown.org/yihui/blogdown/output-format.html](https://bookdown.org/yihui/blogdown/output-format.html)

rendering/ compilation
- .md rendered to HTML by Blackfriday (as of HUGO v0.62.0, renders .md with Goldmark)
- .RMarkdown compiled using rmarkdown and Pandoc
    - see Pandoc and bookdown doc for all features [https://blogdown-demo.rbind.io](https://blogdown-demo.rbind.io)
- blogdown supports LaTex render to HTML, but needs `$math$` (backticks)

personal comment:
- some Hugo themes really don't play nice with .Rmd (e.g. Academic)
- getting floating toc to work in Academic theme was a struggle
- need to learn more about Hugo to come up with custom template (similar to docs but for .Rmd) to fully utilize Academic theme features with .Rmd

### setting blogdown rendering options globally
- instead of setting blogdown rendering options in every .Rmd front matters,
- can set it globally by
- creating _output.yml file in project root directory and type:

```
blogdown::html_page:
  toc: true
  fig_width: 6
  dev: "svg" # device for plots
```

- blogdown:html_page inherits from bookdown::html_document2 inherits from rmarkdown::html_document
- so check ?markdown::html_document for details

personal comments:
- toc cannot float, can only appear at beginning of document

### make blogdown ignore a .Rmd file when compiling
- rename it to an unknown extension e.g. .Rmdignore


## 1.6 other Hugo themes
- [http://themes.gohugo.io](http://themes.gohugo.io)
- not all themes work well with blogdown

### install theme
- `blogdown::install_theme('gcushen/hugo-academic')`
- OR
- `blogdown::new_site(theme = 'gcushen/hugo-academic')`	
    - this is preferred
    - automatically creates `config.toml` file in root dir

### themes that work well with blogdown

| Simple/minimal themes: |  |
|------------------------|-----------------------------------------|
| XMin | https://github.com/yihui/hugo-xmin |
| Tanka | https://github.com/nanxstats/hugo-tanka |
| simple-a | https://github.com/AlexFinn/simple-a |
| ghostwriter | https://github.com/roryg/ghostwriter |
	
| Sophisticated themes |  |
|-------------------------------|-----------------------------------------------------------------|
| hugo-academic | https://github.com/gcushen/hugo-academic |
| hugo-tranquilpeak-theme | https://github.com/kakawait/hugo-tranquilpeak-theme |
| hugo-creative-portfolio-theme | https://github.com/kishaningithub/hugo-creative-portfolio-theme |
| hugo-universal-theme | https://github.com/devcows/hugo-universal-theme |

personal comments:
- wish I played around with XMin first, would have been a less steep learning curve
- but I needed a navbar and side panes for toc
- minimal themes are good for blogs
- I eventually settled with hugo-academic, looks clean, has a lot of users so there's a lot of helps in forums out there


## 1.7 recommended workflow

1. pick a theme
	- pick a theme from [http://themes.gohugo.io](http://themes.gohugo.io)
	- find link to theme repo on github [https://github.com/user/repo](https://github.com/user/repo)
	- create new project in RStudio `blogdown::new_site(theme = 'user/repo')`
	- edit options of `config.toml` to customize
	
2. preview and upload website
	- preview site with Addins > Serve Site (never click knit button)
	- upload public/ folder to your github repo, OR
	- easier with Netlify (continuous deployment instead of manually uploading to github after each update)


## 2.1 Hugo

static sites
- HTML files
- same content no matter who visits the page

dynamic sites
- server side language (e.g. PHP) that sends different content to different users
- e.g. web forum, user login, server fetches user data from a database
- renders different content for different users

static site generators
- Hugo, Jekyll, Hexo

advantage of Hugo
- easy install
- fast rendering
- large community

blogdown = Hugo with Rmarkdown

### blogdown default folder structure
```
	config.toml
	content/
	static/
	themes/
	layouts/
```


## 2.2 config file
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

table in TOML with [ ]
```
    [social]
    github = "https://github.com/rstudio/blogdown"
    twitter = "https://twitter.com/rstudio"
```

array of tables with [[ ]]
```
        [[menu.main]]
		name = "Blog"
		url = "/blog/"
	
	[[menu.main]]
		name = "Categories"
		url = "/categories/"

	[[menu.main]]
		name = "About"
		url = "/about/"
```

array of tables in TOML
- Hugo interprets array of tables as MENU
- if in `/config/_default/menu.toml`, will generate menu links
- menu names defined here, location and style of menu specified elsewhere


## 2.2.2 options
- stuff in `/config/` directory
- defaults see [https://gohugo.io/overview/configuration/](https://gohugo.io/overview/configuration/)
- all can be changed except contentDir, hard-coded to "content"

### `/config.toml`
```
title = "BioinfoHippo"
baseurl = "/" (URL for your site, default set to "/")
copyright = "John Doe &copy; {year}"
relativeurls = true
theme = "hugo-academic"
hasCJKLanguage = false  # Set `true` for Chinese/Japanese/Korean languages.
enableEmoji	= true or false, e.g. :smile: in markdown
etc etc
```

ignoreFiles
- list of filename pattern (regex) for Hugo to ignore and not render
- specify at least these

```
["\\.ign$", "\\.ipynb$", ".ipynb_checkpoints$", "\\.Rmd$", "\\.Rmarkdown$", "_files$", "_cache$"]
```

permalinks
- rules to generate permanent link

```
[permalinks]
  authors = "/author/:slug/"
  tags = "/tag/:slug/"
  categories = "/category/:slug/"
  publication_types = "/publication-type/:slug/"
```

- blogdown recommend using :slug instead of :title
```
  [permalinks]
posts = "/:year/:month/:day/:title/"
		
[permalinks]
posts = "/:year/:month/:day/:slug/"
```

slugs:
- simply an unique character string used to identify a specific post
- slug does not change even if post title changes
- post title may change, but want link to post to stay permanent
- :slug falls back to :title if field name "slug" not set in YAML metadata of the post
- set a fixed :slug, so link to post is fixed, and can update title
- e.g. slug = "blogdown-love"
    - old title = "I love blogdown"
    - new title = "why blogdown is the best"
    - doesn't matter, slug link is still "blogdown-love"

### `/config/_default/params.toml`
```
theme = "minimal"
font = "my_font_set"
font_size = "M"
highlight = true
math = true
diagram = true
email = "mhan@unb.ca"
date_format = "Jan 2, 2006"
etc etc
```

not sure where to find these...
- author (edit: found it in /content/home/about.md)
- publishDir (can set other than public/)
- theme (directory name under themes/)
- uglyURLs
    - Hugo generates "clean" URLs by default
    - for foo.md, generates foo/index.html
    - enable uglyURLs to have strict mapping from foo.md to http://www.example.com/foo/foo.html


## 2.3 `/content/` dir
- can have arbitrary directory structure in content/
- common one is:
```
	├── _index.md
	├── about.md
	├── vitae.md
	├── post/
	│   ├── 2017-01-01-foo.md
	│   ├── 2017-01-02-bar.md
	│   └── ...
	└── ...
```

## 2.3.1 YAML metadata
- "front matters"
- each page should start with YAML metadata

```
	title
	date
	author
	categories
	tags
	etc...

	draft = true (posts not rendered with build_site() or hugo_build(), only renders in preview with Addins > Serve Site)
	publishdate	=	2019, May 5 (specify future publish date, again still renders in preview)
	weight = 100 (if 2 posts have same date, use weight to order them)
	slug = uniqueID	(char string as tail of URL)
```

## 2.3.2 body
- check out markdown or Rmarkdown syntax

## 2.3.3 shortcode
- Hugo automatically generates full HTML code based on a shortcode
- see https://gohugo.io/extras/shortcodes/ for more
- e.g. embed twitter card

{{< tweet 852205086956818432 >}}

```
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
```

becomes

```
in .md
{\{< tweet 852205086956818432 >\}} backslash only to show code

in .Rmd
blogdown::shortcode('tweet', '852205086956818432')
```


## 2.4 themes
- [https://themes.gohugo.io](https://themes.gohugo.io)
- e.g. hugo-lithium


## 2.5 templates
- theme = templates (in `/themes/hugo-academic/layouts/`) + web assets (in `/themes/hugo-academic/assets/`)
- web assets = CSS, JS files, images, videos
- assets determine appearance + functionality of website + embedded content


## 2.5.1 XMin Hugo theme example
- dig into later


## 2.5.2 implement more features

1. google analytics
  - `{{ template "_internal/google_analytics.html" . }}`
  - can add to layouts/partials/foot_custom.html
  - configure googleAnalytics in config.toml
  - preview at https://deploy-preview-3--hugo-xmin.netlify.com
  - (https://github.com/yihui/hugo-xmin/pull/3 for details)

2. disqus comments
  - `{{ template "_internal/disqus.html" . }}`
  - add to foot_custom.html
  - configure disqus shortname in config.toml
  - preview at https://deploy-preview-4--hugo-xmin.netlify.com
  - (https://github.com/yihui/hugo-xmin/pull/4 for details)

3. syntax highlighting via highlight.js
4. math expressions through MathJax
5. table of contents
- https://github.com/yihui/hugo-xmin/pull/7 for an implementation with examples
- if .Rmd, just add YAML metadata to top of .Rmd file
```	
	output:
		blogdown:html_page:
			toc: true
```

- if .md, have to modify single.html template, before content
```	
		{{ if .Params.toc }}
		{{ .TableOfContents }}
		{{ end }}
```	

6. display categories and tags in post
7. add pagination
8. add github edit button


## 2.6 custom layouts
- if customize a theme, future author updates may conflict with your tweaks
```
your-website/
		├── config.toml
		├── ...
		├── themes/
		│      └── hugo-xmin/
		│             ├── ...
		│             ├── layouts/
		│             │      ├── ...
		│             │      └── partials
		│             │             ├── foot_custom.html
		│             │             ├── footer.html
		│             │             ├── head_custom.html
		│             │             └── header.html
		│	        └── static/
		│                    ├── style.css
		│		       └── ...
		│
		├── layouts
		│	└── partials
		│		├── foot_custom.html
		│		└── head_custom.html
		└── static/
			├── style.css
			└── ...
```

### customize in 2 ways
1\. in config.toml (without touching template files)<br>
2\. use layouts/ to override templates in themes/
- e.g., the file layouts/partials/foot_custom.html, when provided, will override themes/hugo-xmin/layouts/partials/foot_custom.html


## 2.7 static files
- static/ stores web assets (images, CSS, JS files)
- all files in static/ copied to public/ when Hugo renders website		  
- put local images in static


### applications of static files

style.css
- e.g. static/style.css will override themes/static/style.css
```
		body {
		  font-family: "Comic Sans MS", cursive, sans-serif;
		}
		code {
		  font-family: "Courier New", Courier, monospace;
		}
```

render .Rmd to any format
- .Rmd file with output NOT generated by blogdown::html_page()
- more info [https://bookdown.org/yihui/blogdown/methods.html#methods](https://bookdown.org/yihui/blogdown/methods.html#methods)
- e.g. .Rmd file generates PDF, 
    - putting the .Rmd file in static/, blogdown will generate PDF,
    - and Hugo will just copy PDF file into public/
    - need to write custom "build" script R/build.R with `blogdown::build_dir("static")`


## 3.1 netlify deployment
- login to Netlify with github username to link Netlify with Github


### Netlify continuous deployment with GitHub Repo
- host all source files (not just public/) in a github repo
- Netlify create new sites from github repo
- Netlify supports Hugo

```
	Deploy settings
	repository			https://github.com/username/repo
	branch				master
	build command			hugo
	publish directory		public
```

- may need to create environment variable HUGO_VERSION
- **need version 0.62.0 to work**
```
	Build environment variables
	HUGO_VERSION		0.24.1	(or latest)	
```

- may take few minutes to deploy on Netlify for first time
- but later updates are incremental

auto-update
- now git push to github repo and
- Netlify calls Hugo to render website automatically


### domain name
- custom domain name, need to configure some DNS records, point it to Netlify server