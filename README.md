# Decentralized-Lottery-application

This is a decentralized lottery application built on the Ethereum blockchain. The application allows users to participate in a lottery by purchasing tickets. A random winner is selected at the end of each lottery cycle, and the prize pool is distributed accordingly.

## Features

- Users can purchase lottery tickets using Ether.
- The application ensures transparency and fairness through smart contracts.
- A secure random number generation mechanism is used to select the winner.
- The prize is automatically distributed to the winner.


## Prerequisites

- Node.js
- npm (Node Package Manager)
- MetaMask browser extension


## Getting Started
#### 1. Clone the Repository
```bash
   git clone https://github.com/yourusername/decentralized-lottery.git<br>
   cd decentralized-Lottery-Application/LotteryDApp
```

#### 2. Install Dependencies
```bash
   npm install
   ```

#### 3. Compile and Deploy Smart Contracts
    Use Remix ide to Compile and Deploy Smart Contracts

#### 4. Configure Frontend
Update the contract address and ABI in the frontend code. Open LotteryDApp/src/constants.js and update the following variables with your deployed contract details:
```bash
  const ContractAddress = 'YOUR_CONTRACT_ADDRESS';
  const ContractABI = YOUR_CONTRACT_ABI;
  ```

#### 5. Run the Application
Start the development server:
 ```bash
  npm run dev
  ```
Open `http://localhost:3000`in your browser to view the application.

## Smart Contract: Lottery.sol

The smart contract handles the core lottery logic:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Lottery {
    address public manager;
    address payable[] public players;
    address payable winner;
    bool public isComplete;
    bool public claimed;
    
    constructor() {
        manager = msg.sender;
        isComplete = false;
        claimed = false;
    }

    modifier restricted() {
        require(msg.sender == manager);
        _;
    }

    function getManager() public view returns (address) {
        return manager;
    }

    function getWinner() public view returns (address) {
        return winner;
    }

    function status() public view returns (bool) {
        return isComplete;
    }
    
    function enter() public payable {
        require(msg.value >= 0.001 ether);
        require(!isComplete);
        players.push(payable(msg.sender));
    }
    
    function pickWinner() public restricted {
        require(players.length > 0);
        require(!isComplete);
        winner = players[randomNumber() % players.length];
        isComplete = true;
    }
    
    function claimPrize() public {
        require(msg.sender == winner);
        require(isComplete);
        winner.transfer(address(this).balance);
        claimed = true;
    }
    
    function getPlayers() public view returns (address payable[] memory) {
        return players;
    }
    
    
    
    function randomNumber() private view returns (uint) {
        return uint(keccak256(abi.encodePacked(block.prevrandao, block.timestamp, players.length)));
    }
}
```


## Contributing
Feel free to submit issues and pull requests. Contributions are welcome!

## Acknowledgements

- Ethereum and the Ethereum community
- Truffle Suite
- MetaMask


## Contact
For any inquiries, please contact [saurabhjadhav9921@gmail.com.]