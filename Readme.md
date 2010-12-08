
# Tobi

  Expressive server-side functional testing with jQuery and jsdom.

  Tobi allows you to test your web application as if it were a browser. Interactions with your app are performed via jsdom, htmlparser, and jQuery, providing a natural JavaScript API for traversing, manipulating and asserting the DOM.

## Example

In the example below, we have an http server or express app `require()`ed, and we simply create new tobi `Browser` for that app to test against. Then we `GET /login`, receiving a response to assert headers, status codes etc, and the `$` jQuery context.

We can then use regular css selectors to grab the form, we use tobi's `.fill()` method to fill some inputs (supports textareas, checkboxes, radios, etc), then we proceed to submitting the form, again receiving a response and the jQuery context.

    var tobi = require('tobi')
      , app = require('./my/app)
      , browser = tobi.createBrowser(app);

    browser.get('/login', function(res, $){
      $('form')
        .fill({ username: 'tj', password: 'tobi' })
        .submit(function(res, $){
          res.should.have.status(200);
          res.should.have.header('Content-Length');
          res.should.have.header('Content-Type', 'text/html; charset=utf-8');
          $('ul.messages').should.have.one('li', 'Successfully authenticated');
          browser.get('/login', function(res, $){
            res.should.have.status(200);
            $('ul.messages').should.have.one('li', 'Already authenticated');
            // We are finished testing, close the server
            app.close();
          });
        });
    });

## Assertions

Tobi extends the [Should.js](http://github.com/visionmedia/should.js) assertion library to provide you with DOM and response related assertion methods.

### Assertion#text(str|regexp)

  Assert element text via regexp or string:
  
      elem.should.have.text('foo bar');
      elem.should.have.text(/^foo/);
      elem.should.not.have.text('rawr');

### Assertion#many(selector)

  Assert that one or more of the given selector is present:
  
      ul.should.have.many('li');

### Assertion#one(selector)

  Assert that one of the given selector is present:
  
      p.should.have.one('input');

### Assertion#attr(key[, val])

  Assert that the given _key_ exists, with optional _val_:
  
      p.should.not.have.attr('href');
      a.should.have.attr('href');
      a.should.have.attr('href', 'http://learnboost.com');

  Shortcuts are also provided:
  
    - id()
    - title()
    - href()
    - alt()
    - src()
    - rel()
    - media()
    - name()
    - action()
    - method()
    - value()
    - enabled
    - disabled
    - checked
    - selected

For example:

      form.should.have.id('user-edit');
      form.should.have.action('/login');
      form.should.have.method('post');
      checkbox.should.be.enabled;
      checkbox.should.be.disabled;
      option.should.be.selected;
      option.should.not.be.selected;

### Assertion#class(name)

  Assert that the element has the given class _name_.

      form.should.have.class('user-edit');

### Assertion#status(code)

  Assert response status code:
  
      res.should.have.status(200);
      res.should.not.have.status(500);

### Assertion#header(field[, val])

  Assert presence of response header _field_ and optional _val_:
  
      res.should.have.header('Content-Length');
      res.should.have.header('Content-Type', 'text/html');

## Testing

Update submodules:

    $ git submodule update --init

and execute:

    $ make test

## License 

(The MIT License)

Copyright (c) 2010 LearnBoost &lt;dev@learnboost.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation te rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.