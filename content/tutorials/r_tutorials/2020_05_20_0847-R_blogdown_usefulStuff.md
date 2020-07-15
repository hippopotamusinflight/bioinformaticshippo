---
date: "2020-05-20T08:47:00+01:00"
draft: false
linktitle: Useful Stuff
menu:
  r_tutorials:
    parent: R Blogdown
    weight: 1
title: Useful Stuff
toc: true
type: docs
weight: 2020052001
---

- some useful commands for building and maintaining a blogdown site
- also some markdown quirks I'm finding along the way

## useful commands
`blogdown::serve_site()` to serve site on localhost<br>
`servr::daemon_stop(1)` to stop server (sometimes serve_site() doesn't refresh)
`Addins > New Post`<br>
`Addins > Insert Image`<br>



---
## useful customization files
- in root dir
    - `/.gitignore` <br>
    - `/.Rprofile` defaults for Addins > New Post<br>
    - `/_output.yml` site wide blogdown:html_page settings<br>
    - `/config.toml` title, baseurl, copyright, relativeurls, theme, enableEmoji, ignoreFiles, permalinks, markup, etc<br>
- in /assets/ dir
    - `/scss/custom.scss` top navbar font size, heading font size, body font size, smooth scrolling<br>
- in /config/_default/ dir
    - `menus.toml` links for top navbar<br>
    - `params.toml` theme within academic, day_night, font="my_font_set", font_size, site_type, math=true, diagram=true, contact details, social, region settings, reading_time, disqus comments, search, maps, google analytics<br>
- in /content/ dir 
    - `authors/johndoe/_index` authors: johndoe, bio, email, etc<br>
    - `home/...` all sorts of widget md files for scrolling homepage<br>
- in /data/ dir
    - `fonts/my_font_set` set custom font, font size, etc<br>
- in /layouts/_default/ dir
	- **do not do below**, blogdown stopped rendering new blog posts after
    - `single.html` table of content on sidebar for .md blog posts<br>
- in /R/ dir
    - `build.R` r script to tell blogdown to build whatever is in static dir<br>
- in /themes/hugo-academic/ dir (shouldn't really touch this)
    - `assets/scss/academic/_docs.scss` looks interesting, might have something to do with setting docs content like toc?<br>
    - `data/page_sharer.toml` enabling disabling share in blog posts
	- `archetypes/docs.md` change docs template when using `blogdown::new_content(path, kind = "docs", ...)`
- in // dir
    - `` <br>


---
## academic theme quirks
- docs: docs page with parent/child will always appear after docs without parent/child
- docs: without assigning weight, left navbar will put them alphabetically, after those with weights



---
## markdown quirks

**lining up content in quote blocks**
- use space (•) instead of tabs (\t)
- e.g.
```
this is some code•••••••••••••••••••••# comment1••••more text
this is some more code••••••••••••••••# comment2••••more text
this is some more and more code•••••••# comment3•••••••more text
```

**insert image to .Rmd page files in /static/ dir**
- has to do it manually (unlike in docs or posts)
- image has to be in same dir as .Rmd file
- `<img src="image1.png" alt="some text" width="70%"/>`
- `![some text](image1.png){width=200px}` no quotes, must have extension
- these can be inline with bullet points to indent images

**insert image to .md page files in /content/ dir**
- image URL is messed up in Hugo or Academic theme
- [https://github.com/gcushen/hugo-academic/issues/14](https://github.com/gcushen/hugo-academic/issues/14)
- after using Addin > Insert Image, have to manually edit image URL
- image URL is absolute path without /static
- `![sample_image](/resources/docker_resources/_index_files/sample_image.jpeg)`
- more quirks
	- need front / before subdir of /static (e.g. /resources)
	- image filename cannot have space (e.g. ~~sample image.jpeg~~)



---
## things to figure out
- create alternative to "home" scrolling page 
	- (see https://sourcethemes.com/academic/docs/managing-content/#create-a-widget-page)
	- `blogdown::new_content(path, kind = “home”, open = interactive())`
- page bundle archetype (blog post as folder with index.html in it) vs uglyUrl
- breadcrumbs navigation
- floating toc for .Rmd
- getting rid of spacing between intro sentence and subsequent bullet lists in .md files (not an issue with .Rmd)
- 


