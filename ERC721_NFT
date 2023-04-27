// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract MyNFT is ERC721 {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721("MyNFT", "MNFT") {}

    struct NFT {
        uint256 id;
        string name;
        string uri;
        uint256 price;
        address payable owner;
    }

    mapping(uint256 => NFT) public nfts;

    function mintNFT(string memory _name, string memory _uri, uint256 _price) public returns (uint256) {
        _tokenIds.increment();
        uint256 newItemId = _tokenIds.current();
        _mint(msg.sender, newItemId);
        nfts[newItemId] = NFT(newItemId, _name, _uri, _price, payable(msg.sender));
        return newItemId;
    }
}
