# Dead-Man-s-Switch-
![images](https://github.com/GPGPgauravpunetha12/Dead-Man-s-Switch-/assets/73377793/6196c2d0-73ac-471f-8d26-5607dee365aa)

## Problem
Dead Man's Switch 
A dead man's switch is a term used in many cases. The idea is pretty simple: if someone should become incapacitated, there is a way to detect it and act on it.

For our particular case we'll create a mechanism where the owner of a contract will need to ping or notify a contract every 52 weeks. If there is no activity during this time period, the recipient will be able to withdraw the funds. The assumption here is that the owner is no longer able to do so.

 We use 52 weeks as opposed to a year since weeks are an available global time unit in solidity: 52 weeks is valid code. Years are not globally available units due to leap years. Learn more here.

 Your Goal: Implement the Switch
Create three functions on the Switch contract:

A public, payable constructor which takes a single argument: the address for the eventual recipient of the funds.
An external withdraw function which will transfer all of the contract funds to the recipient address.
An external ping function which restarts the period of inactivity.
 Contract Security
Ensure that only the owner can call ping. No other address should be able to delay the switch's withdrawal execution. Revert the transaction if the caller is any other address.

Ensure that withdraw can only be called after 52 weeks of inactivity. If the owner has called ping or deployed the contract more recently than that, the withdrawal should be reverted.
## Solution
// SPDX-License-Identifier: MIT
pragma solidity 0.8.4;

contract Switch {
	uint lastAction;
	address recipient;
	address owner;

	constructor(address _recipient) payable {
		recipient = _recipient;
		owner = msg.sender;
		lastAction = block.timestamp;
	}

	function withdraw() external {
		require((block.timestamp - lastAction) >= 52 weeks);
		(bool success, ) = recipient.call{ value: address(this).balance }("");
		require(success);
	}

	function ping() external {
		require(owner == msg.sender);
		lastAction = block.timestamp;
	}
}
