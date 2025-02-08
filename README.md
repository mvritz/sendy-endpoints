# Sendy Required Endpoints

## TokenAddress (CA) to PairAddress Endpoint
### Description
Returns the Pairaddress of a token based on its CA

### Request
- recommended Method: `GET`
- body: `tokenAddress`
- Example https url: 
```https://api.com/token_to_pair?tokenAddress=9Vhh3FaUVGgaRZE6BfW4eAKeV4ujU6s5aMPz1Dkmpump```
- Expected Response:
```json
{
    "pairAddress": "<pairAddress>"
}
```

## Value of Token Endpoint
### Description
Returns the value of a token **in sol** based on its Pairaddress

### Request 
- recommended Method: `GET`
- body: `pairAddress`
- Example https url: `https://api.com/pair_price?pairAddress=<pairAddress>`
- Expected Response:
```
{
    "value": <numberOfTokenValueInSol>
}
```

## Coin Info Data Endpoint
### Description
Endpoint to retreive data like name, CA, top 10 holders, ... of a coin based on its pairAddress

### Request
- recommended Method: `GET`
- body: `pairAddress`
- Example https url: `https://api.com/coin_info?pairAddress=<pairAddress>`
- Expected response:
```ts
 {
    "pair_address": string,
    "token_address": string,
    "token_name": string,
    "token_ticker": string,
    "token_image": string,
    "token_uri": string,
    "created_at": string,
    "website": string | null,
    "twitter": string | null,
    "telegram": string | null,
    "discord": string | null,
    "mint_authority": string | null,
    "deployer_address": string,
    "supply": number,
    "top_10_holders": number,
    "lp_burned": number,
    "freeze_authority": string | null,
}
```

## Coin Data Endpoint
### Description
This endpoint is for the chart data that we provide. It should return the value data of a coin over a specific given time. We should be able to change the amount of data that is returned for that time.

### Request
- recommended Method: `POST`
- body/data:
```json
{
    "pairAddress": "<pairAddressOfCoin>",
    "from": <fromTimestamp>,
    "to": <toTimestamp>,
    "interval": "<intervalForData>"
}
```
- - pairAddress: Address of the token pair to fetch data for.
- - from: Start of the time range (UNIX timestamp in ms).
- - to: End of the time range (UNIX timestamp in ms).
- - currency: The currency to use (either "USD" or "SOL").
- - interval: The interval for data (1s, 15s, 30s, 1m, 3m, 5m, 15m, 30m, 1h, 4h, 6h, 12h, 24h)

- exmaple https url: `POST https://api.com/coin_data`
- Expected Response:
```
  "bars": [
    {
        "time": <timestamp>,
        "open": <open>,
        "close": <close>,
        "high": <high>,
        "low": <low>
        "volume": <volume>,
    }
  ] 
  "noData": true | false
```

## Live Websocket Data
### Description
Live Websocket Data for the coin

### Websocket
- this is a websocket
- how we should be able to join: wss://api.com/live
- on join this will be sent:
```json
{
    "action": "join",
    "room": "<pairAddress>"
}
```
- expected response (everytime the chart udpates):
```
{
    "content": {
        "price_usd": <priceOfOneTokenInUsd>,
        "price_sol": <priceOfOneTokenInSol>
    },
    "pairAddress": <pairAddress>
}
```
