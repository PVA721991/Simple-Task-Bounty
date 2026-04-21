# Simple-Task-Bounty
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TaskBounty {
    struct Bounty {
        string description;
        uint256 reward;
        address creator;
        address completer;
        bool completed;
    }

    Bounty[] public bounties;

    function createBounty(string memory description, uint256 reward) public payable {
        require(msg.value == reward, "Send exact reward");
        bounties.push(Bounty(description, reward, msg.sender, address(0), false));
    }

    function completeBounty(uint256 bountyId) public {
        Bounty storage b = bounties[bountyId];
        require(!b.completed, "Already completed");
        b.completer = msg.sender;
        b.completed = true;
        payable(msg.sender).transfer(b.reward);
    }

    function getBountyCount() public view returns (uint256) {
        return bounties.length;
    }
}
