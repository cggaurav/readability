# Readability

Turn any web page into a clean view. This module is based on arc90's readability project.

[![Build Status](https://travis-ci.org/luin/node-readability.png?branch=master)](https://travis-ci.org/luin/node-readability)

## Features
1. Optimized for more websites.
2. Supporting HTML5 tags(`article`, `section`) and Microdata API.
3. Focusing on both accuracy and performance. 4x times faster than arc90's version.
3. Supporting encodings such as GBK and GB2312.
4. Converting relative urls to absolute for images and links automatically(Thank [Guillermo Baigorria](https://github.com/gbaygon) & [Tom Sutton](https://github.com/tomsutton1984)).

## Example

[Before](https://raw.githubusercontent.com/luin/node-readability/master/examples/before.png) -> [After](https://raw.githubusercontent.com/luin/node-readability/master/examples/after.png)

## Install

    npm install node-readability

## Usage

`read(html [, options], callback)`

Where

  * **html** url or html code.
  * **options** is an optional options object
  * **callback** is the callback to run - `callback(error, article, meta)`

Example

    var read = require('node-readability');

    read('http://howtonode.org/really-simple-file-uploads', function(err, article, meta) {
      // Main Article
      console.log(article.content);
      // Title
      console.log(article.title);

      // HTML Source Code
      console.log(article.html);
      // DOM
      console.log(article.document);

      // Response Object from Request Lib
      console.log(meta);
    });

**NB** If the page has been marked with charset other than utf-8, it will be converted automatically. Charsets such as GBK, GB2312 is also supported.

## Options

node-readability will pass the options to [request](https://github.com/mikeal/request) directly.
See request lib to view all available options.

node-readability has two additional options:

- `cleanRulers` which allow set your own validation rule for tags.

If true rule is valid, otherwise no.
options.cleanRulers = [callback(obj, tagName)]
```javascript
read(url, {
        cleanRulers : [
          function(obj, tag) {
            if(tag === 'object') {
              if(obj.getAttribute('class') === 'BrightcoveExperience') {
                return true;
              }
            }
          }
        ]
      }, function(err, article, response) {});
```

- `preprocess` which should be a function to check or modify downloaded source before passing it to readability.

options.preprocess = callback(source, response, content_type, callback);
```javascript
read(url, {
  preprocess: function(source, response, content_type, callback) {
    if (source.length > maxBodySize) {
      return callback(new Error('too big'));
    }
    callback(null, source);
  }, function(err, article, response) {
    //...
  });

```


## article object

### content

The article content of the web page. Return `false` if failed.

### title

The article title of the web page. It's may not same to the text in the `<title>` tag.

### html

The original html of the web page.

### document
The document of the web page generated by jsdom. You can use it to access the DOM directly(for example, `article.document.getElementById('main')`).

## meta object

response object from request lib. If you need to get current url after all redirect or get some headers it can be useful.

## Why not Cheerio

This lib is using jsdom to parser HTML instead of cheerio because some data such as image size and element visibility isn't able to acquire when using cheerio, which will significantly affect the result. 

## Contributors

https://github.com/luin/node-readability/graphs/contributors

## License

This code is under the Apache License 2.0.  http://www.apache.org/licenses/LICENSE-2.0


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/luin/node-readability/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

