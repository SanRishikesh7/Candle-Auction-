# Candle Auction Smart Contract

This smart contract implements a candle auction on the blockchain, ensuring fairness and unpredictability by using randomness to select the winner.

## Functions Description

- **createAuction(uint256 duration)**: Creates a new auction with the specified duration.
- **makeBid(uint256 auctionId)**: Allows participants to place bids.
- **returnPreviousBid(uint256 auctionId)**: Allows participants to withdraw their previous bids.
- **getWinner(uint256 auctionId)**: Finalizes the auction and transfers the highest bid to the auctioneer.
- **getRandomWinner(uint256 auctionId)**: Uses randomness to determine if the highest bid should be accepted or not.

# How Randomness is Used to Select the Winner

The `getRandomWinner(uint256 auctionId)` function generates a random value using the `keccak256` hash function, combining the current block timestamp and the hash of the previous block:
```solidity
uint256 randomValue = uint256(keccak256(abi.encodePacked(block.timestamp, blockhash(block.number - 1)))) % auction.highestBid;
```

# I have another Smart Contract Using Chainlink's VRF (Verifiable Random Function) 

The `CandleAuction` contract uses Chainlink's VRF (Verifiable Random Function) to ensure fair and unpredictable winner selection:

1. **Requesting Randomness**:
   - When an auction ends, the `endAuction(uint256 auctionId)` function requests a random number from Chainlink VRF using `requestRandomness(keyHash, fee)`.

2. **Receiving Randomness**:
   - Chainlink VRF provides a random number and calls `fulfillRandomness(bytes32 requestId, uint256 randomNumber)` in the contract.

3. **Selecting the Winner**:
   - In `fulfillRandomness`, the contract checks if the random number is even. If so, the highest bid is transferred to the auctioneer, and the highest bidder is declared the winner. This ensures a fair and unpredictable outcome.

This process leverages Chainlink VRF to prevent manipulation and ensure the auction results are random and fair.

# Prerequisites

- Node.js
- Truffel
- ganache
- MetaMask


## Installation

1. Clone this repository

2. Install the dependencies:
   ```bash
   npm install
   npm install -g truffle
   truffle init
   ```
   
3. Compile and depoly the smart contract:
   ```bash
    npx truffle compile
    npx truffle migrate --network development
   ```
   
4. Update the contractAddress from the terminal after deploying the contract and the contractABI from the CandleAuction.json file in the index.html file with the deployed contract address and ABI.

5. Start a local blockchain using Ganache.

6. Add Ganache Account to MetaMask [see here](https://dapp-world.com/smartbook/how-to-connect-ganache-with-metamask-and-deploy-smart-contracts-on-remix-without-1619847868947) 
