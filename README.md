<p align="center" background="black"><img src="un_top.svg" width="450"></p>

<p align="center">
</p>

Python SDK

Ported from official Minter's <a href="https://github.com/MinterTeam/minter-php-sdk">php SDK</a>

Created by <a href="https://www.u-node.net">https://www.u-node.net</a>'s masternode co-founder Roman Matusevich 

You can support our project by sending any Minter's coins to our wallet Mx6e0cd64694b1e143adaa7d4914080e748837aec9

Feel free to delegate to our 3% masternode Mp02bc3c3f77d5ab9732ef9fc3801a6d72dc18f88328c14dc735648abfe551f50f


# Installation
`pip install minter-sdk`


# Using API
```python
from mintersdk.minterapi import MinterAPI

node_url = 'https://minter-node-1.testnet.minter.network:8841'  # Example of a node url
api = MinterAPI(api_url=node_url, **kwargs)
```
You can pass `requests` package attributes as `**kwargs`  
E.g. `api = MinterAPI(api_url=node_url, timeout(1, 3), **kwargs)`

## Methods
- `get_status()`  
  Returns node status info.
  
- `get_candidate(public_key, height=None)`  
  Returns candidate’s info by provided public_key. It will respond with 404 code if candidate is not found.
  
- `get_validators(height=None)`  
  Returns list of active validators.
  
- `get_balance(address, height=None)`  
  Returns coins list, balance and transaction count (for nonce) of an address.
  
- `get_nonce(address)`  
  Returns next transaction number (nonce) of an address.
  
- `send_transaction(tx)`  
  Returns the result of sending signed tx.
  
- `get_transaction(tx_hash)`  
  Returns transaction info.
  
- `get_block(height)`
  Returns block data at given height.
  
- `get_events(height)`  
  Returns events at given height.
  
- `get_candidates(height=None, include_stakes=False)`  
  Returns list of candidates.
  
- `get_coin_info(symbol, height=None)`  
  Returns information about coin. Note: this method does not return information about base coins (MNT and BIP).
  
- `estimate_coin_sell(coin_to_sell, value_to_sell, coin_to_buy, height=None)`  
  Return estimate of sell coin transaction.
  
- `estimate_coin_buy(coin_to_sell, value_to_buy, coin_to_buy, height=None)`  
  Return estimate of buy coin transaction.
  
- `estimate_tx_comission(tx)`  
  Return estimate of transaction.
  
- `get_transactions(query, page=None, limit=None)`  
  Return transactions by query.
  
- `get_unconfirmed_transactions(limit=None)`  
  Returns unconfirmed transactions.
  
- `get_max_gas_price(height=None)`  
  Returns current max gas price.
  
- `get_min_gas_price()`  
  Returns current min gas price.
  
- `get_missed_blocks(public_key, height=None)`  
  Returns missed blocks by validator public key.



# SDK use

You can create transaction by import transaction class and create object of this class.

## Create transactions
Each Minter transaction requires `nonce, gas_coin` to be passed.  Also you can pass `payload, chain_id, gas_price`.

`MiterTx(nonce, gas_coin, payload='', service_data='', chain_id=1, gas_price=1, **kwargs)`

To create Minter transaction you should use concrete transaction class.

All transaction values should be passed in BIP, you shouldn't convert them to PIP.

### Transactions
- MinterBuyCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterBuyCoinTx
  tx = MinterBuyCoinTx(coin_to_buy='SYMBOL', value_to_buy=1, coin_to_sell='SYMBOL', max_value_to_sell=2, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterCreateCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterCreateCoinTx
  tx = MinterCreateCoinTx(name='Coin description', symbol='SYMBOL', initial_amount=1.5, initial_reserve=10000, crr=50, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterDeclareCandidacyTx
  ```python
  from mintersdk.sdk.transactions import MinterDeclareCandidacyTx
  tx = MinterDeclareCandidacyTx(address='Mx...', pub_key='Mp...', commission=1, coin='SYMBOL', stake=100, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterDelegateTx
  ```python
  from mintersdk.sdk.transactions import MinterDelegateTx
  tx = MinterDelegateTx(pub_key='Mp...', coin='SYMBOL', stake=100, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterRedeemCheckTx
  ```python
  from mintersdk.sdk.transactions import MinterRedeemCheckTx
  tx = MinterRedeemCheckTx(check='check hash str', proof='proof hash str', nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterSellAllCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterSellAllCoinTx
  tx = MinterSellAllCoinTx(coin_to_sell='SYMBOL', coin_to_buy='SYMBOL', min_value_to_buy=100, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterSellCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterSellCoinTx
  tx = MinterSellCoinTx(coin_to_sell='SYMBOL', value_to_sell=1, coin_to_buy='SYMBOL', min_value_to_buy=2, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterSendCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterSendCoinTx
  tx = MinterSendCoinTx(coin='SYMBOL', to='Mx...', value=5, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterMultiSendCoinTx
  ```python
  from mintersdk.sdk.transactions import MinterMultiSendCoinTx
  
  txs = [
      {'coin': 'SYMBOL', 'to': 'Mx..1', 'value': 5},
      {'coin': 'SYMBOL', 'to': 'Mx..2', 'value': 1},
      {'coin': 'SYMBOL', 'to': 'Mx..3', 'value': 4}
  ]
  tx = MinterMultiSendCoinTx(txs=txs, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterSetCandidateOffTx
  ```python
  from mintersdk.sdk.transactions import MinterSetCandidateOffTx
  tx = MinterSetCandidateOffTx(pub_key='Mp...', nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterSetCandidateOnTx
  ```python
  from mintersdk.sdk.transactions import MinterSetCandidateOnTx
  tx = MinterSetCandidateOnTx(pub_key='Mp...', nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterUnbondTx
  ```python
  from mintersdk.sdk.transactions import MinterUnbondTx
  tx = MinterUnbondTx(pub_key='Mp...', coin='SYMBOL', value=100, nonce=1, gas_coin='SYMBOL')
  ```
  
- MinterEditCandidateTx
  ```python
  from mintersdk.sdk.transactions import MinterEditCandidateTx
  tx = MinterEditCandidateTx(pub_key='Mp...', reward_address='Mx...', owner_address='Mx...', nonce=1, gas_coin='SYMBOL')
  ```

### Sign tx

``
tx.sign('PRIVATE_KEY')
``

### You can get signed_tx from signed_tx attribute

``
tx.signed_tx
``

To get all required and optional arguments, look for source code.

### To create tx object from raw tx

```python
from mintersdk.sdk.transactions import MinterTx
tx = MinterTx.from_raw(raw_tx='...')
```
You will get tx object of tx type.


### Minter deeplink
Let's create a MinterSendCoinTx
```
from mintersdk.sdk.transactions import MinterSendCoinTx
tx = MinterSendCoinTx(coin='BIP', to='Mx18467bbb64a8edf890201d526c35957d82be3d95', value=1.23456789, nonce=1,
                      gas_coin='MNT', gas_price=1, payload='Hello World')
```

Now it's time to create deeplink
```
from mintersdk.sdk.deeplink import MinterDeeplink
dl = MinterDeeplink(tx=tx, data_only=False)

# Deeplink is generated by all tx params (nonce, gas_price, gas_coin, payload) by default.
# If you want to create deeplink only by tx data, set `data_only=True`
```

After deeplink object is created, you can override it's attributes, e.g.
```
dl = MinterDeeplink(tx=tx)
dl.nonce = ''
dl.gas_coin = 'MNT'
dl.gas_price = 10
```

When your deeplink object is ready, generate it
```
url_link = dl.generate()

# If password check is needed, pass password to generate method
url_link = dl.generate(password='mystrongpassword')
```

Then you might want to create QR-code from your deeplink
```
from mintersdk import MinterHelper
qr_code_filepath = MinterHelper.generate_qr(text=url_link)

# For additional params information for `MinterHelper.generate_qr()`, please see sourcecode for this method.
```
