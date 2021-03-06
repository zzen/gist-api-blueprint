gist-api-blueprint
==================

Install
-------------
`npm install` - download all dependencies and generate a apiary.ast.json in your root folder
`npm run-script clean` - remove all install artefacts

Contents
--------
+ `apiary.apib` - [Gist API](http://developer.github.com/v3/gists/) rewritten as an API Blueprint, [rendered by Apiary here](http://docs.gistsample.apiary.io/).
+ `apiary.ast.json` - generated JSON serialization of the AST
+ `gist.api.js` - JS SDK implementation WIP


Opinionated API
---------------
The SDK makes a lot of magic happen, but for that, certain properties of the API are being assumed.

##### Collections
- there is a plural-named resource representing the collection & a corresponding singular-named resource representing the item
- collection accepts
     - `GET` for listing
     - `POST` for adding new item
     - *(optional)* `DELETE` for dropping all of the items in the collection
- item accepts
     - `GET` for retrieving
     - `POST`, `PATCH` or `PUT` for overwritting
     - `DELETE` for deleting
- item URL is a URL template with the last parameter being ID of the item

##### Actions
TBD `/gists/{id}/fork`
##### Properties
TBD `/gists/{id}/star`


JavaScript SDK (anticipated)
------------------------------

For the sake of simplicity, the SDK requires jQuery for the AJAX methods and promises.
All methods return a [jQuery promise](http://api.jquery.com/category/deferred-object/)

```js
// gists
$.gist.findAll({since: timestamp})
$.gist.add(properties)
$.gist(id).find()
$.gist(id).update(changedProperties)
$.gist(id).remove

// stars
$.gist(id).star.add()
$.gist(id).star.find()
$.gist(id).star.remove()

// forks
$.gist(id).fork.add()
```

Example: star all gists with *.js files

```js
$.gist.findAll()
  .then(filterGistsWithJsFiles)
  .then(favoriteGists)
  .then(function() {
    alert('you love all the JavaScriptz!')
  });

var hasJsFile = function(gist) {
  for (var filname in gist.files) {
    if(/\.js$/.test(filename)) {
      return true
    }
  }
}
var filterGistsWithJsFiles = function(gists) {
  return gists.filter(hasJsFile)
}
var favoriteGist = function(gist) {
  return $.gist(gist.id).star.add();
};
var favoriteGists = function(gists) {
  return gists.map(favoriteGist);
};
```
