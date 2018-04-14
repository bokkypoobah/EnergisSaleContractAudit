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

* [x] [code-review/EnergisToken.md](code-review/EnergisToken.md)
  * [x] contract Ownable
  * [x] contract Claimable is Ownable
  * [x] library SafeMath
  * [x] contract ERC20Basic
  * [x] contract BasicToken is ERC20Basic
    * [x] using SafeMath for uint256;
  * [x] contract ERC20 is ERC20Basic
  * [x] contract StandardToken is ERC20, BasicToken
  * [x] contract EnergisToken is StandardToken, Claimable
    * [x] using SafeMath for uint256;


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
