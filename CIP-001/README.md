# CIP-001
Implementation of CIP-001 https://github.com/Canto-Improvement-Proposals/CIPs/blob/main/CIP-001.md

## Setup
`forge install`

## Deploy
Create `.env` file and set Ethereum RPC at `ETHEREUM_RPC_URL` and deployer private key at `PRIVATE_KEY`.

Run
```
source .env
forge script script/Turnstile.s.sol:TurnstileScript --rpc-url $ETHEREUM_RPC_URL --broadcast -vvvv
```

## Test
`forge test -vvv`

## Gas report
`forge test --gas-report`

## Test coverage
`forge coverage`

```
+------------------------+-----------------+-----------------+-----------------+---------------+
| File                   | % Lines         | % Statements    | % Branches      | % Funcs       |
+==============================================================================================+
| src/Turnstile.sol      | 100.00% (26/26) | 100.00% (32/32) | 100.00% (12/12) | 100.00% (7/7) |
|------------------------+-----------------+-----------------+-----------------+---------------|
| Total                  | 100.00% (26/26) | 100.00% (32/32) | 100.00% (12/12) | 100.00% (7/7) |
+------------------------+-----------------+-----------------+-----------------+---------------+
```


## Gas Optimisation
Turnstiles implements ERC721Enumerable, which holds the totalSupply of the ERC721 contract. Additionally, Turnstiles implements Counters to keep a state variable `_tokenIdTracker` which additionally tracks this. The counter contract and the `_tokenIdTracker` saved an average of 1340 gas at mint `register` time, as well as 15621 gas at deployment

Test result: ok. 8 passed; 0 failed; finished in 141.67ms
| src/Turnstile.sol:Turnstile contract |                 |        |        |        |         |
|--------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                      | Deployment Size |        |        |        |         |
| 1729845                              | 8915            |        |        |        |         |
| Function Name                        | min             | avg    | median | max    | # calls |
| assign                               | 558             | 44884  | 46564  | 46564  | 152     |
| balanceOf                            | 629             | 642    | 629    | 2629   | 150     |
| balances                             | 528             | 1028   | 528    | 2528   | 12      |
| currentCounterId                     | 374             | 400    | 374    | 2374   | 300     |
| distributeFees                       | 513             | 12242  | 12122  | 24072  | 6       |
| getTokenId                           | 879             | 896    | 879    | 2671   | 303     |
| isRegistered                         | 661             | 661    | 661    | 661    | 300     |
| owner                                | 2421            | 2421   | 2421   | 2421   | 1       |
| ownerOf                              | 624             | 637    | 624    | 2649   | 150     |
| register                             | 656             | 157540 | 161436 | 161436 | 154     |
| transferFrom                         | 26095           | 26095  | 26095  | 26095  | 1       |
| withdraw                             | 760             | 5354   | 969    | 37427  | 11      |

Test result: ok. 8 passed; 0 failed; finished in 223.35ms

| src/TurnstileSupply.sol:Turnstile contract |                 |        |        |        |         |
|--------------------------------------------|-----------------|--------|--------|--------|---------|
| Deployment Cost                            | Deployment Size |        |        |        |         |
| 1714224                                    | 8837            |        |        |        |         |
| Function Name                              | min             | avg    | median | max    | # calls |
| assign                                     | 580             | 44906  | 46586  | 46586  | 152     |
| balanceOf                                  | 651             | 664    | 651    | 2651   | 150     |
| balances                                   | 483             | 983    | 483    | 2483   | 12      |
| distributeFees                             | 535             | 12264  | 12144  | 24094  | 6       |
| getTokenId                                 | 879             | 896    | 879    | 2671   | 303     |
| isRegistered                               | 595             | 595    | 595    | 595    | 300     |
| owner                                      | 2376            | 2376   | 2376   | 2376   | 1       |
| ownerOf                                    | 559             | 572    | 559    | 2584   | 150     |
| register                                   | 678             | 156200 | 161231 | 161231 | 154     |
| totalSupply                                | 393             | 419    | 393    | 2393   | 300     |
| transferFrom                               | 26130           | 26130  | 26130  | 26130  | 1       |
| withdraw                                   | 715             | 5309   | 924    | 37382  | 11      |

