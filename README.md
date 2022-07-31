# Osmosis price feed

- Create ATOM/OSMO price candles in [analyse.ipynb](hackatom_2022/analyse.ipynb) and save as Parquet
- Read all Osmosis token swaps using hosted TheGraph API in [store_price.py](hackatom_2022/store_price.py) to CSV
- [Deployed Subgraph](https://thegraph.com/hosted-service/subgraph/miohtama/hackatom-2022)
- [Subgraph source code](https://github.com/miohtama/hackatom-2022)
- [CosmWasm smart contracts](./frontsie/counter-dapp/contracts)
- [Beaker based frontend](./frontsie/counter-dapp/frontend)

## Collect historical price data

Obtain raw Osmosis swap events using [subgraph](https://github.com/miohtama/hackatom-2022)

```shell
poetry install
python hackatom_2022/store_price.py
```

This will generate 600 MB `swaps.csv`.

## Create and examine OHLCV candle data

Use [OHLCV Jupyter Notebook](hackatom_2022/analyse.ipynb).

## Run backtests 

Use [backtest Jupyter Notebook](hackatom_2022/analyse.ipynb).

## Team

- Mikko Ohtamaa
- Mike Purvis
- Teddy Knox
- Mykhailo Donchenko

## Hackathon story

The idea grew and morphed with mostly completion, but also stretch goals partially complete.

The first step was to look into how to get an indexer working for Osmosis so Mikko's knowledge of investment strategies could align with the network and its historical data.

We were able to contact James Bayly from SubGraph and Adam Fuller from The Graph, and set up a good MVP system. To learn more about these two indexer solutions available on Cosmos blockchains, check out these links:

- https://thegraph.com/docs/en/supported-networks/cosmos (great examples at the bottom)
- https://subquery.medium.com/subquery-cosmos-juno-support-developer-deep-dive-e317969633ea

After reaching a level of satisfaction for a hackathon, moved to using the Osmosis tooling. We used `beaker`, `osmojs`, `telescope`, and `LocalOsmosis` to set up our system. The goal was to create a custom frontend that would call a smart contract on Osmosis we wrote (this is modified from the example and lives at `frontsie/counter-dapp/contracts/counter`) that would take OSMO and then do a swap using Osmosis bindings. The bindings part did not complete in time, but is quite doable in a day, we believe.

Along the way there were some excellent discussions with mentors and participants that we'd like to highlight.

We believe all of us learned some valuable information about installing beaker on different systems, and it did seem like upgrading Rust and remembering to run `cargo install cargo-wasm` was key to getting `beaker` to install. We hope those conversations helped that tool become more robust.

Since our goal was to eventually do a swap from a smart contract, we looked into how to create our own custom tokens and create a pool on Osmosis. This took to an interesting turn, eventually leading to great conversations with the Osmosis core team about an issue with the `tokenfactory`. Shoutout to all the folks who helped, especially Dan Lynch, who eventually created this issue [detailing the problem](https://github.com/osmosis-labs/osmosis/issues/2259). This was actually quite a fascinating deep dive into protobufs and amino signing, and well worth the detour.

<img src="./assets/tokenfactory-amino.png" width="600" />

In our efforts to try out the Osmosis bindings, we used an example from a custom branch recommended to us by Xiangan He:
https://github.com/osmosis-labs/cw-tpl-osmosis/blob/xiangan/example/src/contract.rs

This gave us enough knowledge to discover the `Swap` [functionality here](https://docs.rs/osmo-bindings/0.5.1/osmo_bindings/struct.Swap.html).

<img src="./assets/osmo-bindings-swap.png" width="600" />

(Note that for this project, we copied the version `0.6.0` instead of the published crate which looked a bit older.)

While we weren't able to wrap up the swap functionality, we have a head start in this pull request:
https://github.com/mikedotexe/hackatom-2022-osmosis-algorithmic-trading-solution/tree/add-basic-osmo-bindings

<img src="./assets/begin-osmo-bindings.png" width="600" />

We were hoping to be able to create a pool on testnet, but at the 11th hour found a small inconsistency where beaker **had** access to `create-pool` but couldn't seem to point to testnet (11.5th hour errata, yes you can!), and telescope **didn't** have knowledge about the "createPool" Message type for Osmosis yet.

<img src="./assets/osmo-bindings.png" width="600" />
<img src="./assets/osmo-bindings2.png" width="600" />

All in all, we learned a ton about the ecosystem and feel like we've all got a handle on the tooling and possibilities.

At the 11.75th hour, we have a live website!
https://hackatom-seoul-2022.onrender.com

With some luck, this domain will be live once it's propagated:
https://algosm.zone

…and this domain coming Soon™:

<img src="./assets/algosm-zone.png" width="600" />

Thank you, HackAtom Seoul 2022!