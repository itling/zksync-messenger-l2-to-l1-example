# zkSync Messenger L2->L1 Example

> Send a message from L2 zkSync testnet to L1 Goerli and execute message on L1 Goerli after proving inclusion.

## Example

There's two contracts; `L2Contract.sol` and `L1Contract.sol`

The L2 contract has a method `sendGreetingMessageToL1` that sends a message to L1 contract to set a greeting message on L1 contract.

The L1 contract has a method `consumeMessageFromL2` that checks that the message was included in L2 block using zksync `proveL2MessageInclusion` and executes `setGreeting` call on itself which can only be called if the message was sent by the L2 contract.

### Files

- [`L2Contract.sol`](./contracts/L2Contract.sol)
- [`L1Contract.sol`](./contracts/L1Contract.sol)
- [`deployL2.js`](./deploy/deploy.js)
- [`deployL1.js`](./scripts/deployL1.js)
- [`sendL2ToL1Message.js`](./scripts/sendL2ToL1Message.js)
- [`waitForInclusion.js`](./scripts/waitForInclusion.js)
- [`executeMessageOnL1.js`](./scripts/executeMessageOnL1.js)
- [`getGreetingOnL1.js`](./scripts/getGreetingOnL1.js)

## Install

```sh
git clone https://github.com/itling/zksync-messenger-l2-to-l1-example.git
cd zksync-messenger-l2-to-l1-example
npm install
```

### Set Signer

Create `.env`

```sh
PRIVATE_KEY=123...
```

Make sure private key has funds on both zkSync testnet and Goerli.

### Compile Contracts

```sh
npx hardhat compile
```

### Deploy L2 Contract

Command

```sh
npx hardhat deploy-zksync --network zksync
```

Output

```sh
deployed to 0xc285B7A097F705EcCCF5eF808340133908a8743B
```

### Deploy L1 Contract

Command

```sh
L2_CONTRACT=0xc285B7A097F705EcCCF5eF808340133908a8743B \
npx hardhat run --network goerli scripts/deployL1.js
```

Output

```sh
deployed to 0xFf035A730B57eBbe1869b667f01595d4abDb723D
```

### Send L2->L1 Message

Command (replace env vars with your values)

```sh

```sh
GREETING="hello world" \
L2_CONTRACT=0xc285B7A097F705EcCCF5eF808340133908a8743B \
npx hardhat run --network zksync scripts/sendL2ToL1Message.js
```

Output

```sh
sent tx hash 0x57080336b1bb76019e470981c205442b051d97a8e028e958182672a0598bbd06
https://goerli.explorer.zksync.io/tx/0x57080336b1bb76019e470981c205442b051d97a8e028e958182672a0598bbd06
```

### Wait for L1 Block Inclusion

Command

```sh
GREETING="hello world" \
L2_CONTRACT=0xc285B7A097F705EcCCF5eF808340133908a8743B \
L2_TX_HASH=0x57080336b1bb76019e470981c205442b051d97a8e028e958182672a0598bbd06 \
npx hardhat run --network zksync scripts/waitForInclusion.js
```

Output

```sh
Waiting for L1 block inclusion (this may take up to 1 hour)...
L2 block: 11288458
L1 Index for Tx in block: 398
L1 Batch for block:  126516
Inclusion proof: [
  '0x5057d13f383c3657cbe4951e8089b3437f901fdabcaca7e3add7d56f02a93f38',
  '0xf7f829606141ede22423a4cc81a50baf1f37fb8e28586fddfd6942f58cfd5f54',
  '0x770ea634bad61bfc147abd7d497788cf219feef3f2b0344b0aeeb573b2af8e2f',
  '0x39edc8a4f784e0105e5a1e3ecc9779e422f5ec30677dbf8976226e460ad36673',
  '0x2facecc6ac6a614a91a2861ca92ac4f3f63e167ced81e73de94c3335a3c8489d',
  '0x1798a1fd9c8fbb818c98cff190daa7cc10b6e5ac9716b4a2649f7c2ebcef2272',
  '0x66d7c5983afe44cf15ea8cf565b34c6c31ff0cb4dd744524f7842b942d08770d',
  '0xb04e5ee349086985f74b73971ce9dfe76bbed95c84906c5dffd96504e1e5396c',
  '0xac506ecb5465659b3a927143f6d724f91d8d9c4bdb2463aee111d9aa869874db'
]
proveL2MessageInclusion: true
```

### Execute Message on L1

Command

```sh
GREETING="hello world" \
L1_CONTRACT=0xFf035A730B57eBbe1869b667f01595d4abDb723D \
L2_CONTRACT=0xc285B7A097F705EcCCF5eF808340133908a8743B \
L2_TX_HASH=0x57080336b1bb76019e470981c205442b051d97a8e028e958182672a0598bbd06 \
npx hardhat run --network goerli scripts/executeMessageOnL1.js
```

Output

```sh
sent tx hash 0xc3fd470b8ae495d8f491dbf4c8a4807c5c03edb704533c6bdd73377c4471f7a7
https://goerli.etherscan.io/tx/0xc3fd470b8ae495d8f491dbf4c8a4807c5c03edb704533c6bdd73377c4471f7a7
```

### Get Greeting on L1

Command

```sh
L1_CONTRACT=0xFf035A730B57eBbe1869b667f01595d4abDb723D \
npx hardhat run --network goerli scripts/getGreetingOnL1.js
```

Output

```sh
greeting: hello world
```

### verify contract
```
npx hardhat verify --network zksync 0xc285B7A097F705EcCCF5eF808340133908a8743B
```

## License

[MIT](./LICENSE) @ [Miguel Mota](https://github.com/miguelmota)
