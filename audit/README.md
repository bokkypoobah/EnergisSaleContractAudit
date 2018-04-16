# Zero Carbon Project / Energis Private PreSale Contract Audit

## Summary

[Zero Carbon Project](https://www.zerocarbonproject.com/) intends to run a private presale in Apr/May 2018.

Bok Consulting Pty Ltd was commissioned to perform an audit on the Ethereum smart contracts for Zero Carbon Project's private presale.

This audit has been conducted on Zero Carbon Project's private presale contract source code in commit
[9b956af](https://github.com/zerocarbonproject/energis-sale/commit/9b956aff9064628ad688ade8d3df7d2078cf1dc0).

This contract also depends on the token contract in commit [https://github.com/zerocarbonproject/energis-token/blob/fe6b8c45025cd2b99333fa0c29189975476ca8ed/contracts_flat/EnergisToken.sol](https://github.com/zerocarbonproject/energis-token/blob/fe6b8c45025cd2b99333fa0c29189975476ca8ed/contracts_flat/EnergisToken.sol) and this was audited at the same time. This audit has been updated to include
the latest commit [https://github.com/zerocarbonproject/energis-token/blob/05dae6c74cf55e8f77abf5366657a39ae114be41/contracts_flat/EnergisToken.sol](https://github.com/zerocarbonproject/energis-token/blob/05dae6c74cf55e8f77abf5366657a39ae114be41/contracts_flat/EnergisToken.sol) of the token contract.

No potential vulnerabilities have been identified in the private presale and token contracts.

<br />

<hr />

## Table Of Contents

* [Summary](#summary)
* [Recommendations](#recommendations)
* [Potential Vulnerabilities](#potential-vulnerabilities)
* [Scope](#scope)
* [Limitations](#limitations)
* [Due Diligence](#due-diligence)
* [Risks](#risks)
* [Testing](#testing)
* [Code Review](#code-review)

<br />

<hr />

## Recommendations

No recommendations were raised during this audit.

<br />

<hr />

## Potential Vulnerabilities

No potential vulnerabilities have been identified in the private presale and token contracts.

<br />

<hr />

## Scope

This audit is into the technical aspects of the private presale and token contracts. The primary aim of this audit is to ensure that funds
contributed to these contracts are not easily attacked or stolen by third parties. The secondary aim of this audit is to
ensure the coded algorithms work as expected. This audit does not guarantee that that the code is bugfree, but intends to
highlight any areas of weaknesses.

<br />

<hr />

## Limitations

This audit makes no statements or warranties about the viability of Zero Carbon Projects's business proposition, the individuals
involved in this business or the regulatory regime for the business model.

<br />

<hr />

## Due Diligence

As always, potential participants in any crowdsale are encouraged to perform their due diligence on the business proposition
before funding any crowdsales.

Potential participants are also encouraged to only send their funds to the official crowdsale Ethereum address, published on
the crowdsale beneficiary's official communication channel.

Scammers have been publishing phishing address in the forums, twitter and other communication channels, and some go as far as
duplicating crowdsale websites. Potential participants should NOT just click on any links received through these messages.
Scammers have also hacked the crowdsale website to replace the crowdsale contract address with their scam address.
 
Potential participants should also confirm that the verified source code on EtherScan.io for the published crowdsale address
matches the audited source code, and that the deployment parameters are correctly set, including the constant parameters.

<br />

<hr />

## Risks

Ethers (ETH) contributed to the private presale contract is transferred directly into the project wallet, and tokens are generated for the
contributing account. This reduces the severity of any attacks on this private presale contract.

<br />

<hr />

## Testing

Details of the testing environment can be found in [test](test).

The following functions were tested using the script [test/01_test1.sh](test/01_test1.sh) with the summary results saved
in [test/test1results.txt](test/test1results.txt) and the detailed output saved in [test/test1output.txt](test/test1output.txt):

* [x] Deploy token contract
* [x] Deploy private presale contract
* [x] Approve tokens to be sold by the private presale contract
* [x] Whitelist contributing addresses
* [x] Contribute to the private presale contract
* [x] Close sale
* [x] `transfer(...)`, `approve(...)` and `transferFrom(...)`

<br />

<hr />

## Code Review

From [https://github.com/zerocarbonproject/energis-token/blob/05dae6c74cf55e8f77abf5366657a39ae114be41/contracts_flat/EnergisToken.sol](https://github.com/zerocarbonproject/energis-token/blob/05dae6c74cf55e8f77abf5366657a39ae114be41/contracts_flat/EnergisToken.sol):

* [x] [code-review/EnergisToken.md](code-review/EnergisToken.md)
  * [x] contract Ownable
  * [x] contract Claimable is Ownable
  * [x] library SafeMath
  * [x] contract ERC20Basic
  * [x] contract BasicToken is ERC20Basic
    * [x] using SafeMath for uint256;
  * [x] contract BurnableToken is BasicToken
  * [x] contract ERC20 is ERC20Basic
  * [x] contract StandardToken is ERC20, BasicToken
  * [x] contract EnergisToken is StandardToken, Claimable, BurnableToken
    * [x] using SafeMath for uint256;

<br />

From this repository:

* [x] [code-review/PrivatePreSale.md](code-review/PrivatePreSale.md)
  * [x] contract Ownable
  * [x] contract Claimable is Ownable
  * [x] contract KYCWhitelist is Claimable
  * [x] contract Pausable is Claimable
  * [x] library SafeMath
  * [x] contract ERC20Basic
  * [x] contract ERC20 is ERC20Basic
  * [x] contract PrivatePreSale is Claimable, KYCWhitelist, Pausable
    * [x] using SafeMath for uint256;

<br />

<br />

(c) BokkyPooBah / Bok Consulting Pty Ltd for Zero Carbon Project - Apr 17 2018. The MIT Licence.