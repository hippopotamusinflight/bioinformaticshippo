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
    - `single.html` table of content on sidebar for .md blog posts<br>
- in /R/ dir
    - `build.R` r script to tell blogdown to build whatever is in static dir<br>
- in /themes/hugo-academic/ dir (shouldn't really touch this)
    - `assets/scss/academic/_docs.scss` looks interesting, might have something to do with setting docs content like toc?<br>
    - `data/page_sharer.toml` enabling disabling share in blog posts
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

**

---
## things to figure out
- create alternative to "home" scrolling page
- breadcrumbs navigation
- floating toc for .Rmd
- getting rid of spacing between intro sentence and subsequent bullet lists
- 


