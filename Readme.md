
# io

  event-emitter build on top of [engine.io](https://github.com/learnboost/engine.io)

  Requires the engine.io server to include the `event` key in their response.

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

Engine.io squelches pathnames but maintains query parameters, so IO converts any pathname to a querystring to that it can be obtained on the server-side.

```js
IO('localhost:8080/news/today')
// internally: new EngineIO('localhost:8080/?pathname=news/today');
```

### #on(event, fn)

Listen and respond to an `event`.

### #socket

Access to the raw engine.io `socket`. Useful to listen to events such as `open`, `close`, etc.

### #emit(event, message)

Send a `message` to all connected clients with the given `event`.

```js
io.emit('reminder', data);

{
  event : 'reminder',
  message : [{...}]
}
```

### #channel(channel)

Split a single socket into multiple channels, effectively creating a new socket without another connection.

```js
var io = IO('http://localhost:8080');
var cheerio = io.channel('cheerio');
var superagent = io.channel('superagent');

cheerio.emit('install');
superagent.emit('install');

cheerio.on('complete', fn);
superagent.on('complete', fn);
```

## Tests

    make test

  > Note you'll need [serve](http://github.com/visionmedia/serve)

## License

  MIT
