# Message NFT

This project is a website that allows users to send a message via an NFT to an address on the Base Blockchain.

## Table of Contents

- Introduction
- Features
- Installation
- Usage
- Smart Contract
- Contributing
- License

## Introduction

Message NFT is a decentralized application (dApp) that enables users to mint NFTs containing personalized messages and send them to specified addresses on the Base Blockchain. The application leverages IPFS for storing metadata and images, and uses the Ethereum blockchain for minting and transferring NFTs.

## Features

- Mint NFTs with personalized messages.
- Store metadata and images on IPFS.
- Send NFTs to any address on the Base Blockchain.
- Simple and intuitive user interface.

## Installation

To run this project locally, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/your-username/message-nft.git
    cd message-nft
    ```

2. Open the `index.html` file in your preferred web browser.

## Usage

1. Open the website in your browser.
2. Enter your message in the "Enter your message" field.
3. Enter the recipient's address in the "Recipient Address" field.
4. Click the "Send NFT" button.
5. Wait for the transaction to be confirmed. The result will be displayed on the page.

## Smart Contract

The smart contract for this project is written in Solidity and uses OpenZeppelin libraries. It includes functionalities for minting, pausing, and burning NFTs.

### Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.22;

import {ERC721} from "@openzeppelin/contracts@5.1.0/token/ERC721/ERC721.sol";
import {ERC721Burnable} from "@openzeppelin/contracts@5.1.0/token/ERC721/extensions/ERC721Burnable.sol";
import {ERC721Pausable} from "@openzeppelin/contracts@5.1.0/token/ERC721/extensions/ERC721Pausable.sol";
import {ERC721URIStorage} from "@openzeppelin/contracts@5.1.0/token/ERC721/extensions/ERC721URIStorage.sol";
import {Ownable} from "@openzeppelin/contracts@5.1.0/access/Ownable.sol";

contract MessageNFT is ERC721, ERC721URIStorage, ERC721Pausable, Ownable, ERC721Burnable {
    uint256 private _nextTokenId;

    constructor(address initialOwner)
        ERC721("MessageNFT", "MNFT")
        Ownable(initialOwner)
    {}

    function setOwner(address newOwner) public onlyOwner {
        transferOwnership(newOwner);
    }

    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    function _update(address to, uint256 tokenId, address auth)
        internal
        override(ERC721, ERC721Pausable)
        returns (address)
    {
        return super._update(to, tokenId, auth);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the LICENSE file for details.