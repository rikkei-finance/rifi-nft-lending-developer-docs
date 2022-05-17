<p align="center">
  <img src="assets/logo/rifi.png" width="200" alt="web3.js" />
</p>

# RiFi NFT Rental Marketplace API

When user lists, delists and rents nft on NFT Rental Marketplace, smartcontract fires 3 corresponding events, game server needs to listen to these 3 events to process them appropriately for the game. You can use library [web3.js](https://github.com/ChainSafe/web3.js) for listing these events. In this document, we have some sample code using [web3.js](https://github.com/ChainSafe/web3.js)

## Contract overview

### ABI

Please see file `abi.json`

### Address

BSC Testnet: `0xe9D5B350b77D80f6CBe4D2638FA067FEDdB15Ec6`

## Contract Events

### `NFTListRental`

```solidity
    event NFTListRental(
        address indexed nftAddress,
        uint256 indexed tokenId,
        address indexed ownerAddress,
        uint256 minTime,
        uint256 maxTime,
        uint256 pricePerHour,
        uint256 createdAt
    )   
```
When the user list 1 nft goes to the market, they will receive this event, this event returns the parameters as
   - `tokenId` : id of nft
   - `ownerAddress` : the wallet address of the person listed
   - `minTime` : minimum rental time (in seconds)
   - `maxTime` : maximum rental time (in seconds)
   - `pricePerHour` : rental fee over 1 hour

Sample code for get this event:

```js
// In Node.js
const Web3 = require('web3');
const web3 = new Web3('http://localhost:8546');
const lendingNFTContract = new web3.eth.Contract(LENDING_NFT_CONTRACT_ABI, LENDING_NFT_CONTRACT_ADDRESS);
// contract abi and address see above

async function getContractEvent() {
    const pastLentEvents = await lendingNFTContract.getPastEvents('NFTListRental', {
            filter: {
                nftAddress: // your game nft 
            },
			fromBlock: blockNumber,
			toBlock: lastBlockNumber
		});
    for (let i = 0; i < pastLentEvents.length; i++) {
			const event = pastLentEvents[i];
			const { tokenId, ownerAddress, minTime, maxTime, pricePerHour } = event.returnValues;
            // your logic code
	}
}

```
### `RentalCompleted`

```solidity
    event RentalCompleted(
        address indexed nftAddress,
        uint256 indexed tokenId,
        address indexed renterAddress,
        uint256 rentDuration,
        uint256 rentedAt,
        uint256 price,
        uint256 createdAt
    )   
```
When a user rents nft to the market, he will receive this event, this event returns the parameters as
   - `tokenId` : id of nft
   - `renterAddress` : tenant's wallet address
   - `rentDuration` : rental time (in seconds)
   - `rentedAt` : transaction time (timestamp)

Sample code for get this event:

```js
// In Node.js
const Web3 = require('web3');
const web3 = new Web3('http://localhost:8546');
const lendingNFTContract = new web3.eth.Contract(LENDING_NFT_CONTRACT_ABI, LENDING_NFT_CONTRACT_ADDRESS);
// contract abi and address see above

async function getContractEvent() {
    const pastRentedEvents = await lendingNFTContract.getPastEvents('RentalCompleted', {
            filter: {
                nftAddress: // your game nft 
            },
			fromBlock: blockNumber,
			toBlock: lastBlockNumber
		});
    for (let i = 0; i < pastRentedEvents.length; i++) {
			const event = pastRentedEvents[i];
			const { tokenId, renterAddress, rentDuration, rentedAt } = event.returnValues;
            // your logic code
	}
}

```

### `DelistRetal`

```solidity
    event DelistRetal(
        address indexed nftAddress,
        uint256 indexed tokenId,
        uint256 createdAt
    )   
```
When the user cancels the nft rental, he will receive this event, this event returns the parameters as
   - `tokenId` : id of nft

Sample code for get this event:

```js
// In Node.js
const Web3 = require('web3');
const web3 = new Web3('http://localhost:8546');
const lendingNFTContract = new web3.eth.Contract(LENDING_NFT_CONTRACT_ABI, LENDING_NFT_CONTRACT_ADDRESS);
// contract abi and address see above

async function getContractEvent() {
    const pastStoppedEvents = await lendingNFTContract.getPastEvents('DelistRetal', {
            filter: {
                nftAddress: // your game nft 
            },
			fromBlock: blockNumber,
			toBlock: lastBlockNumber
		});
    for (let i = 0; i < pastRentedEvents.length; i++) {
			const event = pastRentedEvents[i];
			const { tokenId } = event.returnValues;
            // your logic code
	}
}

```

