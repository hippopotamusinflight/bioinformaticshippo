---
date: "2020-05-20T08:47:00+01:00"
draft: false
linktitle: Academic Theme
menu:
  r_tutorials:
    parent: R Blogdown
    weight: 2
title: Academic Theme
toc: true
type: docs
weight: 2020052002
---

My implementation/customizations of R blogdown with **Academic theme** from gcushen
- Academic theme documentation [https://sourcethemes.com/academic/](https://sourcethemes.com/academic/)
- Academic theme GitHub repo [https://github.com/gcushen/hugo-academic](https://github.com/gcushen/hugo-academic)


## initialize project

New Project > New Directory > Website using Blogdown >
- Directory name = bioinfohippo
- Hugo theme = gcushen/hugo-academic > |Create Project|

## serve_site() and view in browser
- first edit config.toml
- (baseurl allows sharing to prepend baseurl to post to share)
```
    vim config.toml
     
        title = "bioinfohippo"
        baseurl = "url/to/your/site/"
        copyright = "John Doe &copy `{year}`"
        theme = "hugo-academic"
        relativeurls = true
        ignoreFiles = ["\\.ipynb$", ".ipynb_checkpoints$", "\\.Rmd$", "\\.Rmarkdown$", "_files$", "_cache$"]
```
- serve_site()
```
    blogdown::serve_site()
   
    	Rendering content/post/2015-07-23-r-rmarkdown.Rmd
    	Building sites … 
    					   | EN  
    	-------------------+-----
    	  Pages            | 91  
    	  Paginator pages  |  0  
    	  Non-page files   | 22  
    	  Static files     |  8  
    	  Processed images | 47  
    	  Aliases          | 20  
    	  Sitemaps         |  1  
    	  Cleaned          |  0  
   
    	Total in 692 ms
    	To stop the server, run servr::daemon_stop(1) or restart your R session
    	Serving the directory /Users/minghan/Google Drive/RBlogDown/bioinfohippo at      http://127.0.0.1:4321
```

## push repo to GitHub
- first setup GitHub repo
- then push repo to GitHub
```
      git init
      Initialized empty Git repository in /projectdir/.git
      
      git config --global user.name 'John Doe'
      git config --global user.email 'jd@email.com'
      
      vim .gitignore
      
           .Rhistory
           .Rprofile
           /.Rproj.user/
           projectdir.Rproj
      
      git add .
      git status
      git commit -m 'website initial commit'
      
      git remote add origin https://github.com/username/repo.git
      git push -u origin master
      ...
      ...
       * [new branch]      master -> master
      Branch 'master' set up to track remote branch 'master' from 'origin'.
```

## Netlify
- Netlify > Sign up with your GitHub account > authenticate > new site > GitHub > authenticate > Install Netlify (Only select repositories) > default settings
- e.g. https://app.netlify.com/sites/dazzling-shaw-b2244d/overview
- **need to set HUGO_VERSION = "0.69.2"**

## continuous deployment
- now everytime you `git push -u origin master`,
- Netlify will be notified and build your site using Hugo


---
## customizations

### add module to navbar
- e.g. Resources module
- aiming for this website structure

<img src="/tutorials/r_tutorials/2020_05_20_0847-R_blogdown_AcademicTheme_files/ss 2020-05-23 at 08.40.48.png" alt="aiming for this website structure" width="70%"/>

- courses module from Academic theme has the structure I need, so I just tweaked it
```
cp -R /content/courses /content/resources
vim /content/resources/_index.md
	
	title: Bioinformatics Resources

vim /config/_default/menus.toml

	[[main]]
	  name = "Resources"
	  url = "resources/"
	  weight = 25
```

### add sections into module
- e.g. R
- just copied `example` as `r` and edit its contents
```
cp -R /content/resources/example /content/resources/r
vim /content/resources/r/_index.md

	linktitle: r
	...
	weight: 2
```

### removed modules not needed
- e.g. Demo, Projects, Publications
- didn't delete their directories in case I want to use those modules later
- first edited menus.toml
```
vim /config/_default/menus.toml
  ...
  delete below ...
  [[main]]
    name = "Projects"
    url = "projects/"
    weight = ...
  ...
```

- then disable in their respective .md so they don't show up in scrolling main page
```
vim /content/home/demo.md        active = false
vim /content/home/experience.md  active = false
vim /content/home/skills.md      active = false
...
...
```

### edit Biography
- first copy the /admin dir and edit the copy
```
    cp -R /content/authors/admin/ /content/authors/johndoe/
```
- then edit about.md
```
    vim /content/home/about.md
    
    	author = "johndoe"
```
- then edit johndoe/_index.md
```
    vim /content/authors/johndoe/_index.md
    
    	(edited a bunch of stuff)
```
- then replace avatar.jpg with your own avatar.jpg
```
    /content/authors/johnjoe/avatar.jpg
```

### edit contact info
- all stored in `/config/_default/params.toml`
- remove phones by setting to ""
- remove address by setting to {}
- remove map by setting to []

### get posts on its own page
- changing from widget on homepage to its own page,
- by editing `menus.toml`
```
    vim /config/_default/menus.toml

	    changed from url = "#post" to url = "post/"
```

### first blog post
```
Addins > New Post

	Title = firstpost
	Author = John Doe
	Categories = test
	Tags = test
	Archetype = post
	Format = .Rmd			 > Done

  ## some heading
	### some heading
	```{r}
	library(tidyverse)
	iris %>% head()
	```
```
- set `authors: ["minghan"]` for author info to appear at end of blog
- set `toc: true` to display table of contents at beginning of blog

### setting share options
- each social platform can be enabled or disabled in `page_sharer.toml`
```
    vim /themes/hugo-academic/data/page_sharer.toml
    
        [[buttons]]
          id = "twitter"
          url = "https://twitter.com/intent/tweet?url={url}&text={title}"
          title = "Twitter"
          icon_pack = "fab"
          icon = "twitter"
          enable = true    > set to false to disable
```

### setting blogdown:html_page() options globally
- create _output.yml file in root directory
```
    vim /_output.yml
    
        blogdown::html_page:
          toc: true
          fig_width: 6
          dev: "svg"
```

### edit .Rprofile to set default blog settings
- this will give some default values when you click Addins > New Post
```
file.edit('/Users/minghan/Google Drive/RBlogDown/bioinfohippo/.Rprofile')
	
	options(servr.daemon = 'interactive()',
			blogdown.author = 'Ming Han',
			blogdown.ext = '.Rmd',
			blogdown.subdir = 'post',
			blogdown.yaml.empty = TRUE)
```

### customize font
- make /data/fonts/ dir if it doesn't exist
- copy minimal.toml to /data/fonts/ as my_font_set.toml
- changed font sizes 700 to 500
- set customized font in params.toml
```
    mkdir /data/fonts/
    cp /themes/hugo-academic/data/fonts/minimal.toml /data/fonts/my_font_set.toml
    vim /data/fonts/my_font_set.toml
    	
    	google_fonts = "Montserrat:400,500|Roboto:400,400italic,500|Roboto+Mono"
    	
    vim config/_default/params.toml
    	
    	font = "my_font_set"
```

### custom.scss
- make custom.scss if it doesn't exist
- change navbar font size, heading font size/weight etc
```
    vim /assets/scss/custom.scss
    
    	/* custom scss file */
    
    	https://github.com/gcushen/hugo-academic/issues/1151
    	/* changes navbar font size */
    	.dropdown-menu,
    	nav#navbar-main li.nav-item {
    	  font-size: 16px;
    	  font-weight: bold;
    	}
    
    	/* custom heading font size, weight
    	   from /themes/hugo-academic/assets/scss/academic/_root.scss */
    	h2 {
    	  font-weight: 500;
    	  margin-top: 1rem;
    	  font-size: 1.5rem;
    	}
    	h3 {
    	  font-weight: 500;
    	  margin-top: 1.5rem;
    	  font-size: 1.25rem;
    	}
    
    	/* body font size
    	   from /themes/hugo-academic/assets/scss/academic/_root.scss */
    	body {
    	  font-size: 0.9rem;
    	}
```

### open external link in new tab
- [https://discourse.gohugo.io/t/how-to-open-link-in-new-tab-with-hugos-new-goldmark-markdown-renderer-in-v0-62-0/22540](https://discourse.gohugo.io/t/how-to-open-link-in-new-tab-with-hugos-new-goldmark-markdown-renderer-in-v0-62-0/22540)
- [https://agrimprasad.com/post/hugo-goldmark-markdown/](https://agrimprasad.com/post/hugo-goldmark-markdown/)
- as of Hugo v0.62.0, need "render-hooks" to open external links in new tab
- create render-link.html if does not exist
```
    vim /themes/hugo-academic/layouts/_default/_markup/render-link.html (already existed in     Academic theme)
    
    	{{/* A Hugo Markdown render hook to parse links, opening external links in new tabs.     */}}
    	<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if     strings.HasPrefix .Destination "http" }} target="_blank" rel="noopener"{{ end }}>{{ .Text     | safeHTML }}</a>
```
- in .md file insert
```
    [https://some/website/](https://some/website/)
```
- internal links should still open in same tab,
- as long as it doesn't have "http" prefix

### Addins > Insert Image
- nice GUI to insert image into .md files
- Addins > Insert Image > Image/Browse > Width > Alternative text > Target file path > Done
- Target file path defaults to /static/dir/to/your/markdown/page/
- inserts HTML code into your .md or .Rmd file
    - img src path will ignore /static (this is what you want)
    - may need to change path manually, sometimes blogdown's Addins > Insert Image misses the top directory
    - example:
```
    blogdown will insert this HTML code:
    <img src="/to/your/markdown/page/exampleimage.png" alt="some text" width="70%"/>
    
    should be:
    <img src="/dir/to/your/markdown/page/exampleimage.png" alt="some text" width="70%"/>
```

### toc numbering in .Rmd
- [https://dadascience.design/post/r-some-tricks-when-working-with-blogdown-hugo-working-draft/#custom-toc-numbering-css](https://dadascience.design/post/r-some-tricks-when-working-with-blogdown-hugo-working-draft/#custom-toc-numbering-css)
```
    output:
        blogdown::html_page:
            number_sections: true
            toc: TRUE
```
- still have to figure out how to do this for .md files

### floating toc for blog posts
- .md files only, still can't for .Rmd files
- [http://www.ericfong.ca/post/floatingtoc/](http://www.ericfong.ca/post/floatingtoc/)
- first make /layouts/_default/single.html if does not exist
```
    mkdir /layouts/_default
    vim /layouts/_default/single.html
    	
    	<!-- custom sidebar nav for posts -->
    	{{- define "main" -}}
    	{{ if .Params.toc }}
    	<div class="container-fluid docs">
    		<div class="row flex-xl-nowrap">
    		  <div class="d-none d-xl-block col-xl-2 docs-toc">
    			<ul class="nav toc-top">
    			  <li><a href="#" id="back_to_top" class="docs-toc-title">{{ i18n "on_this_page"     }}</a></li>
    			</ul>
    			{{ .TableOfContents }}
    			{{ partial "docs_toc_foot" . }}
    		  </div>
    		  <main class="col-12 col-md-0 col-xl-10 py-md-3 pl-md-5 docs-content" role="main">
    	{{ end }}
    			<article class="article">
    				{{ partial "page_header" . }}
    				<div class="article-container">
    				  <div class="article-style">
    					{{ .Content }}
    				  </div>
    				  {{ partial "page_footer" . }}
    				</div>
    			</article>
    	  {{ if .Params.toc }}
    		  </main>
    		</div>
    	  </div>
    	  {{ end }}
    	{{- end -}}
```
- then edit custom.scss for smooth scrolling
```
    vim /assets/scss/custom.scss
    
    	html {
    		scroll-behavior: smooth;
    	}
```

### reference inline figure, images
<pre>
    See Figure \@ref(fig:pie) for example:
    
    ```{r pie, fig.cap='A fancy pie chart.', tidy=FALSE}
    some code...
    ```
</pre>

### share manual baseurl
- [https://github.com/gcushen/hugo-academic/issues/1116](https://github.com/gcushen/hugo-academic/issues/1116)
- baseurl not prepending {url} in `page_sharer.html`
- manually add baseurl before each {url} in `page_sharer.html`
```
    vim /themes/hugo-academic/data/page_sharer.html
    
        [[buttons]]
          id = "twitter"
          url = "https://twitter.com/intent/tweet?url=http://yourbaseurl.com/{url}&text={title}"
          title = "Twitter"
          icon_pack = "fab"
          icon = "twitter"
          enable = true
```

### reference internal pages

### collapsible docs sidebar nav

