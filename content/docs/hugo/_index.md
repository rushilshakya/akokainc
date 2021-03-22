---
title: Hugo Tutorial
excerpt: >-
  In this tutorial, we will look at the basic building blocks of a hugo website.


seo:
  type: page_meta
  template: page_meta
  title: Hugo
  description: A Hugo tutorial
  extra:
    - name: "og:type"
      value: website
      keyName: property
    - name: "og:title"
      value: Hugo
      keyName: property
    - name: "og:description"
      value: A Hugo tutorial
      keyName: property
    - name: "twitter:card"
      value: Hugo
    - name: "twitter:title"
      value: Hugo
    - name: "twitter:description"
      value: A Hugo tutorial
layout: docs
---

HUGO TUTORIAL

Lets start with a new Hugo site

```
> mkdir projectDir
> cd projectDir
```

Now lets create the Hugo base

```
> hugo new site .
```

Git stuff

```
> git init
> touch .gitignore
```

content of .gitignore

```
/resources/
/public/
```

then commit to git

```
> git commit
```

Hugo theme (https://pakstech.com/blog/create-hugo-theme/)

```
> hugo new theme themename
```

BLOCKS {{- block "main" . }}{{- end }} This enables to define placeholders in the templates which can then be used in the other pages to fill the desired data by using: {{ define "main"}} {{ end }}

INDEX PAGES \_index.md is used when the page has child pages index.md can be used for leaf pages which contains no child pages

HEAD portion checklist: https://frontendchecklist.io/

TWO types of contents

- Single page
- List page

List page

- Created up to 1 level down from content, e.g. content/dir1
- Not created for 2 level down, e.g. content/dir1/dir2 will not get list page. You will have to create custom list at content/dir1/dir2/\_index.md
  - This does not need to be programmed. Hugo will automatically iterate the pages
- \_index.md can have content which will show up along with the listing, based on how theme is programmed

Archetype

- Determines how ‘Hugo new a.md’ is created: it determines the front matter
- default.md archetype is the default
- Dir1.md will be archetype for all content/dir1 markdowns

Shortcode

- Template of html
- There are some defaults, e.g. for YouTube: {{< youtube youtubeid >}}
- Or create your own shortcake
- Hugo/shortcodes give default shortcodes

Taxonomy

- 2 defaults
  - Tags & categories
    - In content frontmatter
    - tags: [“tag1”, “tag2”]
    - categories: [“cat1”, “cat2”]
- They can be programmatically accessed
- They give list pages even without the directory name
- Adding a new taxonomy, say ‘mood’
  - In frontmatter, add it, e.g. mood: [“happy”, “upbeat”]
  - In config.toml, add (to give the list page)
    - [taxonomies] (singular = plural form)
      - Tag = “tags” (need to add the defaults also)
      - Category = “categories” (need to add the defaults also)
      - Mood = “moods”

TEMPLATE

- Need for list, single
  - And homepage. Not required
- directories
  - \_default is the main default
    - Can have ‘list’, and ‘single’
  - partials
  - page (page is the default type. Basically, to specify custom layout, we can specify type and layout. Type is the folder, and layout is template filename)
  - content directory named templates (section template)
  - Taxonomy
- If no index.html, then default list.html will take over

SINGLE TEMPLATE

- Special variables
  - {{.Content}} is the content of markdown
  - {{.Title}} is from frontmatter
- Custom variables
  - Requires additional thing
  - .Params.variablename

HOMEPAGE TEMPLATE

- index.html on root layout directory

BASE TEMPLATES & BLOCKS

- baseof.html
  - Base can be same for all pages
- Block
  - {{ block “main” . }} (why the “.”?)
  - {{ end }}
- To use Block
  - {{ define “main” }}
  - {{ end }}

PROGRAMMING

- Iterating through
- {{ range .Pages }}
  - <a href=“{{.URL}}>{{ .Title }}</a>
  - Inside the range, the context changes to the current iteration instead of the outside container page the ranging is called from
- {{ end }}
- VARIABLES
  - NOT AVAILABLE inside content; only available in layouts
  - .Content - content on the markdown file
  - .Kind - is the kind of page, e.g. Home / List / Single
  - {{$section := .Section}} | then $section variable is now available to use throughout the site
    - .Title
- There is a way to use variable only if not null
  - with .., instead of first checking with ‘if’
- Functions (Hugo/functions)
  - truncate size string = truncates string to size
  - add 1 5 = 6
  - singularize words = word
  - range parameter
    - .Pages = all pages in current section and inside
    - .Site.Pages = all pages in website
- Conditionals
  - if [and | or ] [not] operator var1 var2
    - eq, lt, le, gt, ge, not
  - else

DATA

- Inside the data/ folder
- Can be accessed from layout
- {{ range .Site.Data.filename }}

PARTIALS

- Only has access to whatever scope is being passed to it
- {{ partial “partialFilename” . }} = getting passed the scope “.” which is the entire scope available in the passing layout; so whatever is accessed as .Title in the passing layout can now be accessed as .Title in the partial
- Can also pass dictionary, e.g.
  - {{ partial “header” (dict “myTitle” “custom title” “myDate” “custom date”)  }}

SHORTCODES

- Are partials for the content markdowns
- Are inside layouts/shortcodes directory
- Needs to be html file
- Called as \{\{< shortcodefilename >\}\}
- Variables can be passed as \{\{< shortcodefilename color=“blue” >\}\}
- Variables can be read as
  - {{ .Get \`color\` }}
- Or more complex as
- \{\{< shortcodefilename >\}\}
- CONTENT CONTENT CONTENT
- \{\{< /shortcodefilename >\}\}
- This is read as {{ .Inner }}
- To preserve markdown
- \{\{% shortcodefilename %\}\}
- ** BOLD STUFF **
- \{\{% /shortcodefilename %\}\}

TAXONOMIES

- tags and categories are automatically configured
- To add additional taxonomy, the following has to be done in the config.toml [taxonomies] category = "categories" tag = "tags" borough = "boroughs"
- For >categories = [‘cat1’, ‘cat2’], Hugo creates
  - /categories, /categories/cat1, /categories/cat2
  - Metadata for /categories/cat1 can be added in markdown file at /content/categories/cat1/\_index.md

BUILDING SITE

- > hugo
- Puts into /public directory
- Delete /public folder before running >hugo command because it doesn’t cleanup

NOTES

- Section
  - basically means directory inside the content directory
  - Or, the category / tag etc. basically whatever grouping context the list is in, is the section
-

QUESTIONS

- What’s the difference between scratch and custom variables?
  - e.g. $.Scratch.Set “sayHello “hello”
  - Vs $sayHello := “hello

INCORPORATING SEARCH INDEX

- search.js, search-data.js
- search.js is called from head as {{- $searchJSFile := printf "%s.search.js" .Language.Lang }} {{- $searchJS := resources.Get "search.js" | resources.ExecuteAsTemplate $searchJSFile . | resources.Minify | resources.Fingerprint }}
  <script defer src="{{ $searchJS.RelPermalink }}" integrity="{{ $searchJS.Data.Integrity }}"></script>
- In search.js
  - This might need flexsearch.min.js?
  - Calls search-data.js
  - Search-data.js creates index by calling some templates?
    - Template is docs/title
    - Creates the index. This is where it is ranging through the data it wants to index
- In the search html page
  - Only search.min.js is required. search.min.js internally calls
    - Search-data
    - flexsearch.min.js

## Lorem ipsum dolor sit amet, consectetur adipiscing elit?

Praesent tristique sem nec lacus laoreet laoreet. Phasellus commodo consectetur viverra. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Praesent mattis rhoncus lacus, vel maximus eros aliquet sit amet. Suspendisse condimentum non elit ac laoreet. Etiam molestie nulla nec ligula ultricies tempor. Nulla facilisi. Cras egestas accumsan nisi. Praesent elementum vehicula turpis sed imperdiet. Curabitur vel justo vitae tellus egestas auctor convallis vel ante. Mauris rhoncus finibus nulla, id rhoncus sapien hendrerit sed. Nullam malesuada mi at erat maximus, in gravida eros aliquet. Proin in enim nec enim tristique lobortis. Nulla finibus consectetur lectus eu porttitor. Vivamus venenatis, mi sit amet laoreet venenatis, augue enim efficitur justo, nec gravida lectus libero vel augue. Integer accumsan nisi lectus, eget imperdiet sem condimentum ut.

<hr />

## Pellentesque id elit mollis, pharetra libero in?

Suspendisse ligula massa, convallis vel turpis at, laoreet venenatis felis. Sed erat tortor, volutpat venenatis efficitur at, mollis ut arcu. Suspendisse potenti. Nulla facilisi. Suspendisse vestibulum quis mi in ullamcorper. Donec rhoncus vitae velit iaculis lobortis. Aliquam eleifend dignissim orci.

<hr />

## Maecenas placerat fermentum libero?

Curabitur quam neque, pulvinar at sagittis viverra, suscipit sed libero. Sed quis lorem feugiat, tempus metus et, tincidunt orci. Vestibulum eget justo venenatis, mollis lectus a, dictum ante. Sed nec metus mi. Fusce pretium quam vel varius aliquam. Nunc vulputate dictum ipsum, sit amet blandit dui ullamcorper sed. Pellentesque ornare arcu at consectetur pharetra. Suspendisse pellentesque euismod diam. Vivamus cursus eget tortor non ultricies.

<hr />

## Integer massa ante, bibendum sed tellus?

Nunc fringilla, est vel mattis malesuada, mauris velit tincidunt est, quis fringilla mauris felis ut sapien. Sed id sapien ex. Vivamus elementum et lectus pharetra accumsan. Curabitur viverra ante vitae velit euismod, in commodo dolor sollicitudin. Vivamus sit amet rhoncus dui, auctor malesuada ante. Phasellus lobortis fringilla magna at placerat. Maecenas nulla magna, condimentum porta tempor eu, tristique vitae nibh. Curabitur imperdiet eros quis justo luctus dapibus. Vestibulum eu ultrices felis, et lobortis urna. Aenean mollis tempus risus, at iaculis nisi rhoncus at. Phasellus eget massa pulvinar, dignissim velit aliquet, sollicitudin erat. Lorem ipsum dolor sit amet, consectetur adipiscing elit.

<hr />

## Nulla a interdum turpis, vitae tempus risus?

Vestibulum non sem eu dui facilisis maximus sed a diam. Proin et nulla vitae urna malesuada sollicitudin. Fusce vel enim nisi. Pellentesque tincidunt enim nec quam ullamcorper egestas. Aliquam erat volutpat. Cras in neque pulvinar augue suscipit feugiat. Vestibulum semper ipsum eu ligula imperdiet ullamcorper.

Donec vulputate nibh tellus, quis commodo lectus aliquet vestibulum. Morbi lobortis sem ex, in accumsan elit consectetur at. Ut dolor dui, eleifend a condimentum a, venenatis sit amet nunc. Aenean suscipit, metus sed ornare ultrices, neque felis ultrices mi, vitae gravida turpis dolor ut quam. Vivamus pulvinar eros id neque tincidunt elementum. Praesent at tristique nisl. Duis semper placerat risus, ac congue lectus varius nec.

<hr />

## Integer convallis urna consequat luctus commodo?

Aenean vitae pulvinar est, et egestas nunc. Sed pharetra mollis felis eleifend pulvinar. Suspendisse feugiat metus ex, ac gravida enim accumsan eu. Curabitur placerat leo ut urna laoreet, quis varius arcu euismod. Maecenas et pretium velit. In egestas libero sed ornare luctus. Ut ac finibus odio.

<hr />

## Nam consequat convallis purus, at eleifend tellus scelerisque et?

Cras eu erat et ligula dictum feugiat at eget sem. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Praesent et justo interdum, iaculis sapien sit amet, ornare est. Duis aliquam a elit et euismod. Sed convallis vestibulum sem sit amet interdum. Sed nec lobortis justo. Mauris fringilla porta risus, et volutpat diam congue at. Nullam commodo fringilla felis ut molestie. Nam eu elit risus. Nulla ornare posuere odio, nec tincidunt nulla elementum ac. Ut viverra eros fringilla aliquet porta. Nulla venenatis enim non blandit tincidunt.

<hr />

## Suspendisse blandit lobortis mi?

Cras vel congue augue. Integer sodales massa quis justo consectetur, id varius quam sagittis. Fusce molestie et diam ac faucibus. Morbi ut eleifend nibh. Quisque maximus velit eget leo aliquam mollis. Sed cursus nunc vitae dignissim consectetur. Vivamus eget venenatis leo, et aliquet odio. Nam mollis lectus ante, vel volutpat nulla elementum ac. Cras ut purus leo. Integer lacus ligula, vehicula vitae felis vel, iaculis tincidunt lorem. Etiam vestibulum venenatis neque sit amet semper. Fusce suscipit ante at mi blandit, a semper urna euismod.
