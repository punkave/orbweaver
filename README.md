# orbweaver

<a href="http://apostrophenow.org/"><img src="https://raw.github.com/punkave/orbweaver/master/logos/logo-box-madefor.png" align="right" /></a>

A simple webcrawler. `orbweaver` prints out the URLs it finds on your site, crawling pages in parallel for speed. `orbweaver` can also simulate many separate website visitors at once, as a load-testing tool.

## Install

```
npm install -g orbweaver
```

## Basic usage

```
orbweaver http://mysite.example.com
```

Output:

```
http://mysite.example.com
http://mysite.example.com/favicon.ico
http://mysite.example.com/apple-touch-icon-72x72.png
http://mysite.example.com/about
http://mysite.example.com/products
... etc ...
```

Let's get rid of those static assets.

```
orbweaver http://mysite.example.com --ignore-static
```

Output:

```
http://mysite.example.com
http://mysite.example.com/about
http://mysite.example.com/products
http://mysite.example.com/services
http://mysite.example.com/blog
http://mysite.example.com/blog/2014/01/01/our-new-product-launch
... etc ...
```

This makes a great sitemap, but I want to know how fast the site is responding and see status codes.

```
orbweaver http://mysite.example.com --metrics --ignore-static
```

Output (notice the referring URLs on the left, useful if there are 404's):

```
* -> http://mysite.example.com: 200 (320ms)
http://mysite.example.com -> http://mysite.example.com/about: 200 (325ms)
http://mysite.example.com -> http://mysite.example.com/products: 200 (310ms)
http://mysite.example.com -> http://mysite.example.com/services: (200ms)
http://mysite.example.com -> http://mysite.example.com/blog: (1050ms)
http://mysite.example.com/blog -> http://mysite.example.com/blog/2014/01/01/our-new-product-launch: 200 (150ms)
```

Great, but I want to test just one page at a time, instead of six at once.

```
orbweaver http://mysite.example.com --parallel=1
```

Still too fast! I want a delay between requests. Half a second would be nice.

```
orbweaver http://mysite.example.com --parallel=1 --interval=500
```

Let's put a limit on the total number of pages explored.

```
orbweaver http://mysite.example.com --limit=1000
```

Let's simulate a real person. The delay between pages should vary. We only fetch one page at a time, and we don't stick around terribly long.

```
orbweaver http://mysite.example.com --interval=500-2000 --metrics --limit=20 --parallel=1
```

How about 100 people?

```
orbweaver http://mysite.example.com --interval=500-2000 --metrics --limit=20 --parallel=1 --users=100
```

## "parallel" versus "users"

Each "user" crawls the site separately, so two users may fetch the same page. Each user counts toward its own limit. For making sitemaps, use the default setting, `--parallel=6`. For simulating traffic load from actual people, use `--users=n`, and set `--parallel=1`.

## Other options

`--ignore-static` is based on our own list of file extensions. You can specify your own with `--ignore-extensions`:

`--ignore-extensions=gif,jpg,png,doc,docx`

Or specify a regular expression that tests the entire URL with `--ignore`:

`--ignore=/api|/private`

These options cannot be combined, so if you wish to use `--ignore` take care to match everything you don't want.

`--timeout` specifies the timeout in milliseconds for each request and defaults to `60000` (one minute).

## holodeck

Also check out [holodeck](http://github.com/punkave/holodeck), which replays the requests found in an existing web server log file.

## Limitations

* There is currently no support for automatically farming out requests to multiple machines to more accurately simulate traffic from many people.

* Only GET requests are simulated.

* The crawler is currently only interested in `href` attributes. It won't find images, JavaScript files, etc.

* The user agent is not configurable. Your webserver might treat it differently than a regular browser.

* `orbweaver` will never leave the site you point it to. An option to do so might be nice.

## Changelog

0.1.4: removed arachnophobia-triggering photo.

0.1.3: `--metrics` output includes referring URLs so you can do more with 404's. Also, fixed a bug in which entities in URLs were not decoded, leading to false 404's on some URLs with query strings.

0.1.2: set `main` to `lib/orbweaver.js` so the API can be used properly.

0.1.1: added `filterAttributes` option for those using the API who want to see what's going on in documents.

0.1.0: initial release.

## About P'unk Avenue and Apostrophe

`orbweaver` was created at [P'unk Avenue](http://punkave.com) for use in testing websites built with Apostrophe, an open-source content management system built on node.js. `orbweaver` isn't mandatory for Apostrophe and vice versa, but they play very well together. If you like `orbweaver` you should definitely [check out apostrophenow.org](http://apostrophenow.org). Also be sure to visit us on [github](http://github.com/punkave).

## Support

Feel free to open issues on [github](http://github.com/punkave/orbweaver).

<a href="http://punkave.com/"><img src="https://raw.github.com/punkave/orbweaver/master/logos/logo-box-builtby.png" /></a>
