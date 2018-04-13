## Energis Sale Audit

Commit [9b956af](https://github.com/zerocarbonproject/energis-sale/commit/9b956aff9064628ad688ade8d3df7d2078cf1dc0)

<br />

<hr />

## Table Of Contents

* [Recommendations](#recommendations)
* [Code Review](#code-review)

<br />

<hr />

## Recommendations

<br />

<hr />

## Code Review

From [https://github.com/zerocarbonproject/energis-token/blob/fe6b8c45025cd2b99333fa0c29189975476ca8ed/contracts_flat/EnergisToken.sol](https://github.com/zerocarbonproject/energis-token/blob/fe6b8c45025cd2b99333fa0c29189975476ca8ed/contracts_flat/EnergisToken.sol):

* [ ] [code-review/EnergisToken.md](code-review/EnergisToken.md)
  * [ ] contract Ownable
  * [ ] contract Claimable is Ownable
  * [ ] library SafeMath
  * [ ] contract ERC20Basic
  * [ ] contract BasicToken is ERC20Basic
  * [ ]   using SafeMath for uint256;
  * [ ] contract ERC20 is ERC20Basic
  * [ ] contract StandardToken is ERC20, BasicToken
  * [ ] contract EnergisToken is StandardToken, Claimable
  * [ ]   using SafeMath for uint256;


From this repository:

* [ ] [code-review/PrivatePreSale.md](code-review/PrivatePreSale.md)
  * [ ] contract Ownable
  * [ ] contract Claimable is Ownable
  * [ ] contract KYCWhitelist is Claimable
  * [ ] contract Pausable is Claimable
  * [ ] library SafeMath
  * [ ] contract ERC20Basic
  * [ ] contract ERC20 is ERC20Basic
  * [ ] contract PrivatePreSale is Claimable, KYCWhitelist, Pausable
  * [ ]   using SafeMath for uint256;
