# [Riemann](http://aphyr.github.com/riemann/) Node.js Client

because you should be monitoring all of those [non-blocking buffet plates.](http://www.infinitelooper.com/?v=-sfZqL4Plxc&p=n#/242;267)


## Installation

Riemann uses [Google Protocol Buffers](http://code.google.com/p/protobuf/), so make sure thats installed beforehand, and available on your `PATH`.

```
npm install riemann
```

**NOTE:** unfortuantely, this module only works with node v0.6.x, __for the time being__. There is a problem with node's protocol buffers library support in node v0.8.x. [If you'd like to contribute to solving that problem](https://github.com/chrisdew/protobuf/issues/8), it'd be greatly appreciated!

## Getting Started

first things first, we'll want to establish a new client:

```js
var client = require('riemann').createClient({ host: 'some.riemann.server', port: 5555 });
client.on('connect', function() { console.log('connected!'); });
```

Just like [Riemann ruby client](https://github.com/aphyr/riemann-ruby-client), the client sends small events over UDP, by default. TCP is used for queries, and large events. There is no acknowledgement of UDP packets, but they are roughly an order of magnitude faster than TCP. We assume both TCP and UDP are listening to the same port.

sending events is easy (see [list of valid event properties](http://aphyr.github.com/riemann/concepts.html)):

```js
client.send(client.Event({
  service: 'buffet_plates',
  metric:  252.2,
  tags:    ['nonblocking']
}));
```

If you wanted to send that message over TCP and receive an acknowledgement, you can specify the transport, explicitly:

```js
client.on('data', function(ack) { console.log('got it!'); });

client.send(client.Event({
  service: 'buffet_plates',
  metric:  252.2,
  tags:    ['nonblocking']
}), client.tcp);
```

When you're done monitoring, disconnect:

```js
client.on('disconnect', function(){ console.log('disconnected!'); });
client.disconnect();
```


## Contributing

Contributing is easy, just send me a pull request. Please take a look at the project issues, to see how you can help. Here are some helpful tips:

- install the developer dependencies using `npm install --dev`
- please add tests. I'm using [Mocha](http://visionmedia.github.com/mocha/) as a test runner, you can run the tests using `npm test`
- please check your syntax with the included jshint configuration using `npm run-script lint`. It shouldn't report any errors.