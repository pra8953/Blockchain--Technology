# 🪙 Blockchain Technology

## 🧠 What is Blockchain?

Blockchain is a **distributed immutable ledger** that ensures complete transparency.  
The main purpose of blockchain is **decentralization**.  
In **January 2009**, *Satoshi Nakamoto* created the first cryptocurrency — **Bitcoin**.

---

## 🦊 MetaMask

**MetaMask** is a software cryptocurrency wallet used to interact with the **Ethereum blockchain**.

![MetaMask](https://github.com/pra8953/Blockchain--Technology/assets/154659571/e5032d64-9b35-4f94-944e-07e2d76198b4)

---

## 📜 Smart Contract

**Smart contracts** are applications that run on a blockchain and automatically execute actions when predefined conditions are met.  
For example, a smart contract can initiate a fund transfer once both parties sign an agreement.

---

## ⚙️ How to Make a Smart Contract in Blockchain?

### 🧩 Step 1: Open Remix IDE
To create a smart contract, open the **Remix IDE** → [https://remix.ethereum.org/](https://remix.ethereum.org/)

### 🧩 Step 2: Create a Contract File
In the **contracts** folder, create a new file with a `.sol` extension (for example, `EventContract.sol`).

### 🧩 Step 3: Write Your Smart Contract
Below is an example of a simple **Event Management Smart Contract** written in Solidity.

---

### 💻 Code Example

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.5.0 <0.9.0;

contract EventContract {
    struct Event {
        address organizer;
        string name;
        uint date;
        uint price;
        uint ticketCount;
        uint ticketRemain;
    }

    mapping(uint => Event) public events;
    mapping(address => mapping(uint => uint)) public tickets;
    uint public nextId;

    function createEvent(string memory name, uint date, uint price, uint ticketCount) external {
        require(date > block.timestamp, "You can organise event for future date");
        require(ticketCount > 0, "You can organise event only if tickets > 0");

        events[nextId] = Event(msg.sender, name, date, price, ticketCount, ticketCount);
        nextId++;
    }

    function buyTicket(uint id, uint quantity) external payable {
        require(events[id].date != 0, "Event does not exist");
        require(events[id].date > block.timestamp, "Event already occurred");

        Event storage _event = events[id];
        require(msg.value == (_event.price * quantity), "Ether value is incorrect");
        require(_event.ticketRemain >= quantity, "Not enough tickets");

        _event.ticketRemain -= quantity;
        tickets[msg.sender][id] += quantity;
    }

    function transferTicket(uint id, uint quantity, address to) external {
        require(events[id].date != 0, "Event does not exist");
        require(events[id].date > block.timestamp, "Event already occurred");
        require(tickets[msg.sender][id] >= quantity, "Not enough tickets to transfer");

        tickets[msg.sender][id] -= quantity;
        tickets[to][id] += quantity;
    }
}
