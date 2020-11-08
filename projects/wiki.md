## Todo
- styles:
	- article
	- fancy article
	- code highlighting
- sitenavigation:
	-wiki -> subwiki -> article, wiki -> article

## Tags -> template configurations and layouts
[tag] wiki-nav-collection: a collection of pages that should be grouped together under a single moniker and be included in the root wiki navigation. May or may not include an index for the collection.
- [tag] collection-title: unique title used for display purposes
- [tag] index-url: url for the index used as a root node for the collection

## Search
*ElasticLunr*: lightweight javascript library; add documents to search into an index object. 
```javascript
let index = elasticlunr(function(){
	this.addFiel("title")
	this.addField("body")
	this.setRef("id")
})

var doc1 = {
	"id": 1,
	"title": "This is a title...",
	"body": "this is body content..."
}

index.addDoc(doc1)

index.search("is body")

// or, with field boosting
index.search("is", {
	fields: {
		title: {boost: 2},
		body: {boost: 1}
	}
})
```
## ideas
### tabbed notecards
collection of markdown notecards, separately each in a different file, displayed as a large bootstrap tabbed navbar
```html
<ul class="nav nav-tabs" id="myTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">Home</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">Profile</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">Contact</a>
  </li>
</ul>
<div class="tab-content" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">...</div>
  <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">...</div>
  <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">...</div>
</div>	
```

### drag and drop cards
- into lists, grids, etc. from a dropdown menu.

### use pandoc instead of markdown it...?
**node-pandoc**: node package that interfaces with host's pandoc with a library of commands. We can call pandoc as a javascript function, with any options we want, and recieve either file or string output.
**Pandoc Filters**: extending the pandoc compilation pipeline:

```
INPUT --reader--> AST --filter--> AST --writer--> OUTPUT
```
### Sitemap and Navigation
General Schema reachable from site navbar or sidebar:
- Homepage
	- SubDomain (such as knowledge wiki)
		- landing pages for subdomains

Any deeper nesting can be handled on subdomain landing pages, though I want to keep the sitemap relatively normalized. Horizontal scaling within 3 nested levels is preferable.

```
knowledge {foldable}
- list of sub wikis or pages here
rpgs {foldable}
- list of sub pages/wikis here...
```

### Eleventy Notes
#### Collections (using tags)
used by templates:
`for post in collections.post` 

or in eleventy javascript configuration:
``` javascript
(collections) => {
	return (collections.post.map => `<li>${ post.data.title }<li>`).join("\n")
}
```

- set per file in front matter `tags: post` or `tags: ['cat', 'dog']`
- set per directory subtree in ascending parent node order (i.e. deepest takes precidence)

given: `posts/subdir/mypost.md`, these are applied, with first first given precidence.
- `posts/subdir/mypost.json`
- `posts/subdir/subdir.json`
- `posts/posts.json`
