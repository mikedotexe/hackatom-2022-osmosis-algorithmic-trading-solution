# Osmosis price feed

- Create ATOM/OSMO price candles in [analyse.ipynb](hackatom_2022/analyse.ipynb) and save as Parquet
- Read all Osmosis token swaps using hosted TheGraph API in [store_price.py](hackatom_2022/store_price.py) to CSV
- [Deployed Subgraph](https://thegraph.com/hosted-service/subgraph/miohtama/hackatom-2022)
- [Subgraph source code](https://github.com/miohtama/hackatom-2022)

# Install

```shell
poetry install
python hackatom_2022/store_price.py
```