//// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import "@openzeppelin/contracts/token/ERC721/IERC721Receiver.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

contract Marketplace is IERC721Receiver {
    using SafeMath for uint256;
    using SafeERC20 for IERC20;

    struct Listing {
        address seller;
        uint256 price;
    }

    IERC20 public token;
    IERC721 public nft;
    mapping (uint256 => Listing) public listings;

    constructor(address _token, address _nft) {
        token = IERC20(_token);
        nft = IERC721(_nft);
    }

    function onERC721Received(address, address, uint256, bytes memory) public virtual override returns (bytes4) {
        return this.onERC721Received.selector;
    }

    function createListing(uint256 _tokenId, uint256 _price) public {
        require(nft.ownerOf(_tokenId) == msg.sender, "Marketplace: Only token owner can create listing");
        require(_price > 0, "Marketplace: Price must be greater than 0");
        nft.safeTransferFrom(msg.sender, address(this), _tokenId);
        listings[_tokenId] = Listing(msg.sender, _price);
    }

    function buyListing(uint256 _tokenId) public {
        Listing memory listing = listings[_tokenId];
        require(listing.seller != address(0), "Marketplace: Listing does not exist");
        require(token.balanceOf(msg.sender) >= listing.price, "Marketplace: Insufficient balance");
        token.safeTransferFrom(msg.sender, listing.seller, listing.price);
        nft.safeTransferFrom(address(this), msg.sender, _tokenId);
        delete listings[_tokenId];
    }

    function withdrawListing(uint256 _tokenId) public {
        require(listings[_tokenId].seller == msg.sender, "Marketplace: Only seller can withdraw listing");
        nft.safeTransferFrom(address(this), msg.sender, _tokenId);
        delete listings[_tokenId];
    }
}
