*This repository is a mirror of the [component](http://component.io) module [matthewmueller/io](http://github.com/matthewmueller/io). It has been modified to work with NPM+Browserify. You can install it using the command `npm install npmcomponent/matthewmueller-io`. Please do not open issues or send pull requests against this repo. If you have issues with this repo, report it to [npmcomponent](https://github.com/airportyh/npmcomponent).*
# io

  higher-level [engine.io](http://github.com/learnboost/engine.io) client.

  [io-server](http://github.com/matthewmueller/io-server) is the recommended socket server, but any server that has `event` and `channel` events would work fine.

## Installation

    $ component install matthewmueller/io

## Example

```js
var io = IO('localhost:8080/news');

// Custom events
io.on('news', function(news) {...})

// Raw socket messages
io.socket.on('error', function(err) {...})

// Send a message
io.emit('news', news);
```

## API

### IO(uri, [options])

Initialize a new instance of `IO`. IO will pass these parameters into engine.io.

#### Pooling

IO supports url-based pooling or "rooms". If you connect with a pathname, IO will send the pathname to the server as a querystring. It's the responsibilty of the server to place these connections into rooms. [io-server](http://github.com/matthewmueller/io-server) supports this kind of pooling.

```js
IO('localhost:8080/news/today')
// internally: new EngineIO('localhost:8080/?pathname=news/today');

IO('localhost:8080/news/tomorrow') // different pool than `/news/today`
IO('localhost:8080/news') // gets updates from both `/news/today` and `/news/tomorrow`
```

**Why convert pathname to querystring?**

Engine.io squelches pathnames but maintains query parameters, so IO converts any pathname to a querystring to that it can be obtained on the server-side.

### io#on(event, fn)

Listen and respond to an `event`.

### io#socket

Access to the raw engine.io `socket`. Useful to listen to events such as `open`, `close`, etc.

### io#emit(event, message)

Send a `message` to all connected clients (including itself) with the given `event`.

```js
io.emit('reminder', data);
```

### io#channel([channel])

Split a single socket into multiple channels. In other words, `#channel()` creates a fresh socket  without another connection.

If no `channel` is given, a unique id is used.

```js
var io = IO('http://localhost:8080');
var cheerio = io.channel('cheerio');
var superagent = io.channel('superagent');

cheerio.emit('install');
superagent.emit('install');

cheerio.on('complete', fn);
superagent.on('complete', fn);
```

You can also split an already split socket:

```js
var script = io.channel('scriptID');
var cheerio = script.channel('cheerio');
console.log(cheerio.$channel) // scriptID:cheerio
```

### io#close()

Close the connection

## Tests

    make test

  > Note you'll need [serve](http://github.com/visionmedia/serve)

## TODO

* Fix pathname support on node client

## License

  MIT
