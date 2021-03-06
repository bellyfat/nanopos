# nanopos

[![npm release](https://img.shields.io/npm/v/nanopos.svg)](https://www.npmjs.com/package/nanopos)
[![MIT license](https://img.shields.io/github/license/ElementsProject/paypercall.svg)](https://github.com/ElementsProject/paypercall/blob/master/LICENSE)
[![Pull Requests Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![IRC](https://img.shields.io/badge/chat-on%20freenode-brightgreen.svg)](https://webchat.freenode.net/?channels=lightning-charge)

A simple Lightning :zap: point-of-sale system with a clean & minimal web UI.

   * Optimized for places selling fixed-price items (like coffee shops, falafel stands or barber shops), but also has an option for billing custom amounts.
   * Small codebase (~60 server-side LoC + ~70 client-side LoC), great starting point for developing your own Lightning Charge apps!

Powered by :zap: [Lightning Charge](https://github.com/ElementsProject/lightning-charge).

<img src="https://i.imgur.com/puslYKb.png" width="45%"></img>
<img src="https://i.imgur.com/kScuEjG.png" width="45%"></img>

[See demo video here ➤](https://www.youtube.com/watch?v=ckYGyhbovrg)

## Setup

```bash
$ npm install -g nanopos

$ edit items.yaml # see file format below

$ nanopos --items-path items.yaml --charge-token mySecretToken --currency USD
HTTP server running on localhost:9116
```

## Running with Docker

Nanopos includes a Dockerfile to allow for fast setup using a docker container based on node:carbon. To run from the container with port 9116 exposed, first setup [Lightning Charge](https://github.com/ElementsProject/lightning-charge), then build the image with:

```bash
$ docker build -t elements_project/nanopos .
```

and then run with:

```bash
$ docker run -p9116:9116 -e CHARGE_URL=http://[charge-url]/ -e CHARGE_TOKEN=[access-token] elements_project/nanopos
```

That's it! The web server should now be running on port 9116 and ready to accept payments.

## Example `items.yaml` file

```
tea:
  price: 0.02 # denominated in the currency specified by --currency
  title: Green Tea # title is optional, defaults to the key

coffee:
  price: 1

bamba:
  price: 3

beer:
  price: 7

hat:
  price: 15

tshirt (S):
  price: 25
  metadata:
    shirt_size: S
```

The `metadata` object will be added into the metadata of invoices for this item.

## CLI options

```bash
$ nanopos --help

  A simple Lightning point-of-sale system, powered by Lightning Charge.

  Usage
    $ nanopos [options]

  Options
    -c, --charge-url <url>      lightning charge server url [default: http://localhost:9112]
    -t, --charge-token <token>  lightning charge access token [required]

    -y, --items-path <path>     path to yaml file with item config [default: ./items.yaml, file is required]
    -x, --currency <currency>   currency to use for item prices [default: BTC]
    -m, --theme <name>          pick theme from bootswatch.com [default: yeti]
    -l, --title <name>          website title [default: Lightning Nano POS]
    --no-custom                 disable custom amount field [default: false]
    --show-bolt11               display bolt11 as text and button [default: false]

    -p, --port <port>           http server port [default: 9115]
    -i, --host <host>           http server listen address [default: 127.0.0.1]
    -h, --help                  output usage information
    -v, --version               output version number

  Example
    $ nanopos -t chargeSecretToken -x EUR -y items.yaml
```

## License

MIT

