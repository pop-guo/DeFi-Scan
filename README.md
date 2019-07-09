# DefiChain Explorer - Defiscan

[![Join the chat at https://gitter.im/defichain/defiscan](https://badges.gitter.im/defichain/defiscan.svg)](https://gitter.im/defichain/defiscan?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Defiscan is a BlockExplorer for **DefiChain**, built with VueJS, Nuxt and MongoDB. Defiscan allows you to explore and search the **DefiChain** for transactions, addresses, tokens, prices and other activities taking place on **DefiChain**.

A demo instance connected to the **DefiChain testnet** is available at [scan.testnet.defichain.com](https://scan.testnet.defichain.com/).

## Current Features
- Browse blocks, transactions, accounts and contracts
- View pending transactions
- Upload & verify contract sources
- Display the current state of verified contracts
- Responsive layout

Missing a feature? Please request it by creating a new [Issue](https://github.com/defichain/defiscan/issues).

## Usage notes

The explorer is still under heavy development, if you find any problems please create [an issue](https://github.com/defichain/defiscan/issues) or prepare [a pull request](https://github.com/defichain/defiscan/pulls).

## Getting started

### Requirements
- [NodeJS](https://nodejs.org/)
- [MongoDB](https://www.mongodb.com/)
- [Redis](https://redis.io/)

### Setup

Copy and modify your config
```bash
cp client/.env.example client/.env
cp server/config/default.json server/config/local.json
```

Install library
```bash
cd client/ && npm install
```

```bash
cd server/ && npm install
```

### Run
After modify your config & install library. Your environment is ready to start

- Run client to view in browser
```bash
cd client/ && npm run dev
```

- Run API server for client
```bash
cd server/ && npm run server-dev
```

- Run crawl data for API server
```bash
cd server/ && npm run crawl-dev
```

- Get transaction pending
```bash
cd server/ && npm run subscribe-pending-tx-dev
```
