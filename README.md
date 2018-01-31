# bitfinex-api-events

A node module to connect to Bitfinex API and retrieve available symbols, ticker info, order books and stats of available cryptocurrencies

It supports events to get alerts on several data.

## Installation

```console
$ npm install bitfinex-api-events
```

## Usage Example
```js
var BitFinEx = require("bitfinex-api-events");
var bitfinex = new BitFinEx();
// If you want to check a single symbol, use getTicker() (You need to supply the bitfinex id of the cryptocurrency, not the symbol)
// If you want to use symbols instead of id, use multi.
bitfinex.getTicker("bitsymbol", symbol => {
  console.log(symbol.latest_price); // Prints the price in USD of BTC at the moment.
});
// If you want to check multiple symbols, use multi():
bitfinex.multi(symbols => {
  console.log(symbols.getTicker("BTC").latest_price); // Prints price of BTC in USD
  console.log(symbols.getTicker("ETH").latest_price); // Print price of ETH in USD
  console.log(symbols.getTicker("ETH").price_btc); // Print price of ETH in BTC
  console.log(symbols.getTop(10)); // Prints information about top 10 cryptocurrencies
});
```
## Usage Example with Events

```js
var BitFinEx = require("bitfinex-api-events");

var options = {
	events: true, // Enable event system
	refresh: 60, // Refresh time in seconds (Default: 60)
	convert: "EUR" // Convert price to different currencies. (Default USD)
}
var bitfinex = new BitFinEx(options);

// Trigger this event when BTC price is greater than 4000
bitfinex.onGreater("BTC", 4000, (symbol) => {
	console.log("BTC price is greater than 4000 of your defined currency");
});

// Trigger this event when BTC percent change is greater than 20
bitfinex.onPercentChange24h("BTC", 20, (symbol) => {
	console.log("BTC has a percent change above 20% in the last 24 hours");
});

// Trigger this event every 60 seconds with information about BTC
bitfinex.on("BTC", (symbol) => {
	console.log(symbol);
});
```
For a full list of examples with events, visit: https://github.com/hvmonteiro/bitfinex-api-events/blob/master/example.js