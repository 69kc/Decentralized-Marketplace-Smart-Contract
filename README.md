# Decentralized-Marketplace-Smart-Contract
A decentralized marketplace smart contract is a smart contract that can be used to create a decentralized marketplace where buyers and sellers can trade without intermediaries. 


pragma solidity ^0.8.0;

contract Marketplace {
    struct Product {
        uint256 id;
        string name;
        uint256 price;
        address payable seller;
        bool sold;
    }

    mapping(uint256 => Product) public products;
    uint256 public productIndex;

    function createProduct(string memory _name, uint256 _price) public {
        require(_price > 0);
        productIndex++;
        products[productIndex] = Product(productIndex, _name, _price, payable(msg.sender), false);
    }

    function buyProduct(uint256 _id) public payable {
        require(products[_id].id != 0, "Product does not exist.");
        require(!products[_id].sold, "Product already sold.");
        require(msg.value >= products[_id].price, "Insufficient payment.");

        products[_id].seller.transfer(msg.value);
        products[_id].sold = true;
    }
}
