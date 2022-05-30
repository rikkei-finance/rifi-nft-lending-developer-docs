<p align="center">
  <img src="assets/logo/rifi.png" width="200" alt="web3.js" />
</p>

# RiFi NFT Rental Marketplace API

When an asset owner lists or de-lists or there is a rental transaction completed (i.e. event) on the NFT Rental Marketplace, smartcontract triggers an event emitter for the associated event. Game platform servers need to listen and handle the event emitter appropriately. You can use library [web3.js](https://github.com/ChainSafe/web3.js) for listing these events. In this document, we have some sample code using [web3.js](https://github.com/ChainSafe/web3.js)

## Contract overview

### ABI

Please see file `abi.json`

### Address

BSC Testnet: `0x07ac5567bcE0745f2B2c9cC72753E94Da5207708`

## Contract Events

### `NFTListRental`

```solidity
    event NFTListRental(
        address indexed nftAddress,
        uint256 indexed tokenId,
        address indexed ownerAddress,
        uint256 minTime,
        uint256 maxTime,
        uint256 pricePerDay,
        uint256 createdAt
    )   
```
When the asset owner lists the NFT on the marketplace, the event emitter returns these parameters:
   - `tokenId` : id of nft
   - `ownerAddress` : the wallet address of the asset owner
   - `minTime` : minimum rental time (in seconds)
   - `maxTime` : maximum rental time (in seconds)
   - `pricePerDay` : rental fee over 1 day

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
			const { tokenId, ownerAddress, minTime, maxTime, pricePerDay } = event.returnValues;
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
When a marketplace user (renter) rents the NFT from the marketplace, the event emitter returns these parameters:
   - `tokenId` : id of nft
   - `renterAddress` : renter's wallet address
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

### `RentalReturn`

```solidity
    event RentalReturn(
        address indexed nftAddress,
        uint256 indexed tokenId,
        address indexed ownerAddress,
        address renterAddress,
        uint256 createdAt
    )  
```
When the renter return the NFT to the marketplace, the event emitter returns these parameters:
   - `tokenId` : id of nft

Sample code for get this event:

```js
// In Node.js
const Web3 = require('web3');
const web3 = new Web3('http://localhost:8546');
const lendingNFTContract = new web3.eth.Contract(LENDING_NFT_CONTRACT_ABI, LENDING_NFT_CONTRACT_ADDRESS);
// contract abi and address see above

async function getContractEvent() {
    const pastReturnEvents = await lendingNFTContract.getPastEvents('RentalReturn', {
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

### `DelistRetal`

```solidity
    event DelistRetal(
        address indexed nftAddress,
        uint256 indexed tokenId,
        address indexed ownerAddress,
        uint256 createdAt
    )   
```
When the asset owner de-lists the NFT from the marketplace, the event emitter returns these parameters:
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

