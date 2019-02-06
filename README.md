# How to host content on IPFS:
This DEX frontend needs you!

This web3page content isn't accesible without other people (that means you!) altruistically hosting it on ipfs.


Help DAI grow by using an old computer

#### Step 1: Download v1.6 content from github
```
# -L to follow redirects
curl -L -o build.tgz https://github.com/Leibniz137/oasis-direct/releases/download/v1.6.0/build.tgz
tar xvfz build.tgz
```

### Step 2: Install & Configure IPFS
```
# prerequisite: install ipfs with a package manager like brew or apt-get

# in another terminal window
# your ipfs daemon needs to be running for your computer to service requests for the DEX frontend
# ie you need a server for this.

ipfs init
ipfs daemon
```

### Step 3: Add Content to IPFS
```
ipfs add --recursive build
```


# Developer Notes:
```
# modifications to fix install:

# to fix this timeout:
# $ git ls-remote --tags --heads git://github.com/frozeman/WebSocket-Node.git
# fatal: unable to connect to github.com:
# github.com[0: 192.30.253.112]: errno=Operation timed out
# github.com[1: 192.30.253.113]: errno=Operation timed out
git config url."https://".insteadOf git://

# to fix this error:
TODO what was the error?

yarn config set ignore-engines true
```

(original Oasis Direct README begins here)

# Oasis Direct  
Oasis.Direct is a convenient and user friendly fully-decentralized way to change tokens, especially DAI/ETH and DAI/ERC-20 token pairs.   

You don’t need to think about wrapping/unwrapping ETH (and other non ERC-20 compliant tokens), you don’t need to worry about the current liquidity on the exchange and you are always guaranteed the best price you can get from Oasis.DEX exchange.

## Deployments
- [production](https://oasis.direct/)
- [stage](https://stage-oasis-direct.surge.sh) - auto deploy branch `release/simpson`

## Sections in Oasis Direct Web App

 1. **Exchange**
    *  Exchange tokens in matters of seconds. Just provide the amount you want to sell or the amount of the token you want to get. The application will automatically check the liquidity pool for orders that will fulfill yours. You  receive information how much is the rate and the gas cost for the transaction in `ETH`.
    * In order to select token, just click on the square box where the token icon is and a list with the supported tokens will appear. If you select token that is part of the trade pair, tokens will just swap.
    * Another way to swap tokens is to click the arrows icon.
    * To proceed with transaction, please read the _**Terms of Service**_ and accept them by marking the check box.
    * If given account doesn't have insufficient funds to trade an error message will be displayed.
    * If given order can't be fulfilled because there aren't enough corresponding orders, there will be notification.
    * Trading _ETH for ERC20 Token_ wil require only a single transaction. You can follow the status of the transaction on _Etherscan_ by clicking the transaction box.
    * Trading _ERC20 Token for ETH_  or _ERC20 Token for ERC20 Token_ will require 2 transaction initially. Once you go through those 2 transaction every other time you will only a single transaction. See **FAQ section** for more details.

 2. **Export Trades**
	* Exports trade history for the given addresses in the form of a `csv` file.
	* As many address as one want can be added.
	* Only OasisDEX is supported right now but in the future we plan to support many more exchanges


## FAQ

**What is Oasis.Direct ?**

Oasis.Direct is the most convenient, fully-decentralized way to change tokens. You don’t need to think about wrapping/unwrapping Ether or any other Ethereum token, you don’t need to worry about the current liquidity and you are always guaranteed the best available price on [Oasis.DEX](www.oasisdex.com). Simply specify the amount that you want to trade and confirm the transaction.


**Are you going to support more trading pairs in the future ?**


Yes, more trading pairs will be supported in the future. In the beginning the following pair is available: DAI/ETH.

**Why do I sometimes need to confirm two transactions, and sometimes only one ?**

Each trading pair requires a one-time transaction per Ether address to be enabled for trading. Once that is done, all subsequent trades will require confirming just one Ethereum transaction.
You just need to confirm two Ethereum transactions if it is the first time you are trading a particular pair from the new Ethereum address.

**What does “Order estimation may vary” mean ?**

The slippage limit value is used to create a Limit Order for your transaction. If this Limit Order cannot be immediately completely filled on Oasis.DEX, the transaction will fail. It’s impossible to guarantee that your order will succeed, since it is being matched with other orders on Oasis.DEX in real time. The fact that Oasis.Direct uses Limit Orders, rather than Market Orders, ensures that a user won’t experience unexpected slippage.

**What happens if the transaction price exceeds the given threshold ? Will I still pay gas price for the transaction ?**

Yes, you will still pay for gas. Bear in mind that the likelihood that your order can’t be filled is quite low, and will only occur if the price has moved in excess of the slippage limit in the time between you receiving the quote and your transaction getting mined.

**Why is the “order estimation vary indicator” (threshold) different for some pairs ?**

The slippage limits were decided by analyzing the volatility and book depth historically on the Oasis.Dex for the given pairs. The limit might be adjusted accordingly if the market on Oasis.DEX changes. In the future, you will be able to set the slippage limit manually according to your personal preference.

**Can you list order estimation variations (thresholds) for all assets/tokens ?**

Right now there is  a fixed threshold of  2% for DAI/ETH pair.

**What is the maximum value I can trade through Oasis.Direct ?**

There is no maximum size limit. However, larger orders impact the market more, and so may be quoted poorer prices. They are also more likely to fail by exceeding the slippage limit

**I want to submit a big Sell/Buy Order. Can I see how likely my order will fail due to exceeding threshold limit ?**

This functionality will be included in the future release of Oasis.Direct

**Why there is a minimum trading limit ?**

As the gas cost for a transaction is fixed and doesn’t  depend on the amount you want to trade, the bigger amount, the less (percentage-wise) you pay for gas. Minimum trade sizes were introduced to make sure that the total transaction cost is minimal with respect to the traded amount. Currently the minimum trade sizes are:
 * 30 DAI
 * 0.03 ETH

Note that both buy and sell amounts of a transaction need to be above the minimum trade size.


**You said Oasis.Direct is fee-less. Do I still need to cover gas cost ?**


Yes, even though there are no fees, you still need to cover the gas cost of the transaction. The gas price is estimated according to the current transaction details. To learn more about gas and why it is needed for Ethereum transactions, check this excellent [guide](https://myetherwallet.github.io/knowledge-base/gas/what-is-gas-ethereum.html).

**Why does the transaction confirmation takes so long? What can I do to speed it up ?**


Oasis.Direct is powered by fully decentralized, on-chain Smart Contracts. The transaction confirmation time depends on the current state of the Ethereum blockchain. You can increase the speed of confirmation by increasing the gas price for your transaction - please refer to this [guide](https://myetherwallet.github.io/knowledge-base/gas/what-is-gas-ethereum.html) that explains all the mechanics of the gas price and its effect on the speed of transactions.


## Development

### Starting the app
```
yarn
yarn start
```

### E2E tests

First, make sure you have app and oasis-localnode already running. Then run all cypress tests:

```
yarn cypress:run
```

To develop:

```
yarn cypress:dev
```

If you're trying to debug test failure on CI visit [Cypress Dashboard](https://dashboard.cypress.io/#/projects/noiqfs/runs)

#### CI

Sometimes, you might want to update localnode version used in tests. There is a `LOCALNODE_SHA` in `.circleci/config.yml`. This SHA should match top of the master branch in localnode repository.

## Env variables

```sh
OASIS_GANACHE_COMPATIBILITY=1                # optional, if enabled we are gonna adjust gas estimations
OASIS_HIDE_MKR=1                             # optional
```
