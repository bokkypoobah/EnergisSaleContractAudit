# PrivatePreSale

Source file [../../contracts_flat/PrivatePreSale.sol](../../contracts_flat/PrivatePreSale.sol).

<br />

<hr />

```javascript
// BK Ok
pragma solidity ^0.4.21;

// File: zeppelin-solidity/contracts/ownership/Ownable.sol

/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
// BK Ok
contract Ownable {
  // BK Ok
  address public owner;


  // BK Ok - Event
  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  // BK Ok - Constructor
  function Ownable() public {
    // BK Ok
    owner = msg.sender;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  // BK Ok
  modifier onlyOwner() {
    // BK Ok
    require(msg.sender == owner);
    // BK Ok
    _;
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  // BK Ok - Only owner can execute. This function is overridden
  function transferOwnership(address newOwner) public onlyOwner {
    // BK Ok
    require(newOwner != address(0));
    // BK Ok - Log event
    emit OwnershipTransferred(owner, newOwner);
    // BK Ok
    owner = newOwner;
  }

}

// File: zeppelin-solidity/contracts/ownership/Claimable.sol

/**
 * @title Claimable
 * @dev Extension for the Ownable contract, where the ownership needs to be claimed.
 * This allows the new owner to accept the transfer.
 */
// BK Ok
contract Claimable is Ownable {
  // BK Ok
  address public pendingOwner;

  /**
   * @dev Modifier throws if called by any account other than the pendingOwner.
   */
  // BK Ok
  modifier onlyPendingOwner() {
    // BK Ok
    require(msg.sender == pendingOwner);
    // BK Ok
    _;
  }

  /**
   * @dev Allows the current owner to set the pendingOwner address.
   * @param newOwner The address to transfer ownership to.
   */
  // BK Ok - Only owner can execute
  function transferOwnership(address newOwner) onlyOwner public {
    // BK Ok
    pendingOwner = newOwner;
  }

  /**
   * @dev Allows the pendingOwner address to finalize the transfer.
   */
  // BK Ok - Only pending owner can execute
  function claimOwnership() onlyPendingOwner public {
    // BK Ok - Log event
    emit OwnershipTransferred(owner, pendingOwner);
    // BK Ok
    owner = pendingOwner;
    // BK Ok
    pendingOwner = address(0);
  }
}

// File: contracts/external/KYCWhitelist.sol

/**
 * @title KYCWhitelist
 * @dev Crowdsale in which only whitelisted users can contribute.
 */
// BK Ok
contract KYCWhitelist is Claimable {

   // BK Ok
   mapping(address => bool) public whitelist;

  /**
   * @dev Reverts if beneficiary is not whitelisted. Can be used when extending this contract.
   */
  // BK Ok
  modifier isWhitelisted(address _beneficiary) {
    // BK Ok
    require(whitelist[_beneficiary]);
    // BK Ok
    _;
  }

  /**
   * @dev Does a "require" check if _beneficiary address is approved
   * @param _beneficiary Token beneficiary
   */
  // BK Ok - Internal view function
  function validateWhitelisted(address _beneficiary) internal view {
    // BK Ok
    require(whitelist[_beneficiary]);
  }

  /**
   * @dev Adds single address to whitelist.
   * @param _beneficiary Address to be added to the whitelist
   */
  // BK Ok - Only owner can execute
  function addToWhitelist(address _beneficiary) external onlyOwner {
    // BK Ok
    whitelist[_beneficiary] = true;
  }
  
  /**
   * @dev Adds list of addresses to whitelist. Not overloaded due to limitations with truffle testing. 
   * @param _beneficiaries Addresses to be added to the whitelist
   */
  // BK Ok - Only owner can execute
  function addManyToWhitelist(address[] _beneficiaries) external onlyOwner {
    // BK Ok
    for (uint256 i = 0; i < _beneficiaries.length; i++) {
      // BK Ok
      whitelist[_beneficiaries[i]] = true;
    }
  }

  /**
   * @dev Removes single address from whitelist. 
   * @param _beneficiary Address to be removed to the whitelist
   */
  // BK Ok - Only owner can execute
  function removeFromWhitelist(address _beneficiary) external onlyOwner {
    // BK Ok
    whitelist[_beneficiary] = false;
  }

  
}

// File: contracts/external/Pausable.sol

/**
 * @title Pausable
 * @dev Base contract which allows children to implement an emergency stop mechanism.
 */
// BK Ok
contract Pausable is Claimable {
  // BK Next 2 Ok - Events
  event Pause();
  event Unpause();

  // BK Ok
  bool public paused = false;


  /**
   * @dev Modifier to make a function callable only when the contract is not paused.
   */
  // BK Ok
  modifier whenNotPaused() {
    // BK Ok
    require(!paused);
    // BK Ok
    _;
  }

  /**
   * @dev Modifier to make a function callable only when the contract is paused.
   */
  // BK Ok
  modifier whenPaused() {
    // BK Ok
    require(paused);
    // BK Ok
    _;
  }

  /**
   * @dev called by the owner to pause, triggers stopped state
   */
  // BK Ok - Only owner can execute, when not paused
  function pause() onlyOwner whenNotPaused public {
    // BK Ok
    paused = true;
    // BK Ok - Log event
    emit Pause();
  }

  /**
   * @dev called by the owner to unpause, returns to normal state
   */
  // BK Ok - Only owner can execute when paused
  function unpause() onlyOwner whenPaused public {
    // BK Ok
    paused = false;
    // BK Ok - Log event
    emit Unpause();
  }
}

// File: zeppelin-solidity/contracts/math/SafeMath.sol

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
// BK Ok
library SafeMath {

  /**
  * @dev Multiplies two numbers, throws on overflow.
  */
  // BK Ok - Internal pure function
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    // BK Ok
    if (a == 0) {
      // BK Ok
      return 0;
    }
    // BK Ok
    uint256 c = a * b;
    // BK Ok
    assert(c / a == b);
    // BK Ok
    return c;
  }

  /**
  * @dev Integer division of two numbers, truncating the quotient.
  */
  // BK Ok - Internal pure function
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    // BK Ok
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    // BK Ok
    return c;
  }

  /**
  * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
  */
  // BK Ok - Internal pure function
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    // BK Ok
    assert(b <= a);
    // BK Ok
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  // BK Ok - Internal pure function
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    // BK Ok
    uint256 c = a + b;
    // BK Ok
    assert(c >= a);
    // BK Ok
    return c;
  }
}

// File: zeppelin-solidity/contracts/token/ERC20/ERC20Basic.sol

/**
 * @title ERC20Basic
 * @dev Simpler version of ERC20 interface
 * @dev see https://github.com/ethereum/EIPs/issues/179
 */
// BK Ok - Interface
contract ERC20Basic {
  // BK Next 3 Ok
  function totalSupply() public view returns (uint256);
  function balanceOf(address who) public view returns (uint256);
  function transfer(address to, uint256 value) public returns (bool);
  // BK Ok - Event
  event Transfer(address indexed from, address indexed to, uint256 value);
}

// File: zeppelin-solidity/contracts/token/ERC20/ERC20.sol

/**
 * @title ERC20 interface
 * @dev see https://github.com/ethereum/EIPs/issues/20
 */
// BK Ok - Interface
contract ERC20 is ERC20Basic {
  // BK Next 3 Ok
  function allowance(address owner, address spender) public view returns (uint256);
  function transferFrom(address from, address to, uint256 value) public returns (bool);
  function approve(address spender, uint256 value) public returns (bool);
  // BK Ok - Event
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: contracts/PrivatePreSale.sol

/**
 * @title PrivatePreSale
 * 
 * Private Pre-sale contract for Energis tokens
 *
 * (c) Philip Louw / Zero Carbon Project 2018. The MIT Licence.
 */
// BK Ok
contract PrivatePreSale is Claimable, KYCWhitelist, Pausable {
  // BK Ok
  using SafeMath for uint256;

  
  // Wallet Address for funds
  // BK Ok
  address public constant FUNDS_WALLET = address(0);
  // Token Wallet Address
  // BK Ok
  address public constant TOKEN_WALLET = address(0);
  // Token adderss being sold
  // BK Ok
  address public constant TOKEN_ADDRESS = address(0);
  // Token being sold
  // BK Ok
  ERC20 public constant TOKEN = ERC20(TOKEN_ADDRESS);
  // Conversion Rate (Eth cost of 1 NRG) (Testing uses ETH price of $10 000)
  // BK Ok
  uint256 public constant TOKENS_PER_ETH = 100000;
  // Max NRG tokens to sell
  // BK Ok
  uint256 public constant MAX_TOKENS = 4000000 * (10**18);
  // Min investment in Tokens
  // BK Ok
  uint256 public constant MIN_TOKEN_INVEST = 300000 * (10**18);
  // Token sale start date
  // BK Ok
  uint256 public START_DATE = 1522749798;

  // -----------------------------------------
  // State Variables
  // -----------------------------------------

  // Amount of wei raised
  // BK Ok
  uint256 public weiRaised;
  // Amount of tokens issued
  // BK Ok
  uint256 public tokensIssued;
  // If the pre-sale has ended
  // BK Ok
  bool public closed;

  // -----------------------------------------
  // Events
  // -----------------------------------------

  /**
   * Event for token purchase logging
   * @param purchaser who paid for the tokens
   * @param beneficiary who got the tokens
   * @param value weis paid for purchase
   * @param amount amount of tokens purchased
   */
  // BK Ok - Event
  event TokenPurchase(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);


  // -----------------------------------------
  // Constructor
  // -----------------------------------------


  // BK Ok - Constructor
  function PrivatePreSale() public {
    // BK Next 6 Ok
    require(TOKENS_PER_ETH > 0);
    require(FUNDS_WALLET != address(0));
    require(TOKEN_WALLET != address(0));
    require(TOKEN_ADDRESS != address(0));
    require(MAX_TOKENS > 0);
    require(MIN_TOKEN_INVEST >= 0);
  }

  // -----------------------------------------
  // Private PreSale external Interface
  // -----------------------------------------

  /**
   * @dev Checks whether the cap has been reached. 
   * @return Whether the cap was reached
   */
  // BK Ok - View function
  function capReached() public view returns (bool) {
    // BK Ok
    return tokensIssued >= MAX_TOKENS;
  }

  /**
   * @dev Closes the sale, can only be called once. Once closed can not be opened again.
   */
  // BK Ok - Only owner can execute
  function closeSale() public onlyOwner {
    // BK Ok
    require(!closed);
    // BK Ok
    closed = true;
  }

  /**
   * @dev Returns the amount of tokens given for the amount in Wei
   * @param _weiAmount Value in wei
   */
  // BK Ok - Pure function
  function getTokenAmount(uint256 _weiAmount) public pure returns (uint256) {
    // Amount in wei (10**18 wei == 1 eth) and the token is 18 decimal places
    // BK Ok
    return _weiAmount.mul(TOKENS_PER_ETH);
  }

  /**
   * @dev fallback function ***DO NOT OVERRIDE***
   */
  // BK Ok - Only whitelisted accounts can call this fallback function that can receive ETH
  function () external payable {
    // BK Ok
    buyTokens(msg.sender);
  }

  // -----------------------------------------
  // Private PreSale internal
  // -----------------------------------------

   /**
   * @dev low level token purchase ***DO NOT OVERRIDE***
   * @param _beneficiary Address performing the token purchase
   */
  // BK Ok - Only whitelisted accounts can call this internal function via the fallback function, when not paused
  function buyTokens(address _beneficiary) internal whenNotPaused {
    
    // BK Ok
    uint256 weiAmount = msg.value;

    // calculate token amount to be created
    // BK Ok
    uint256 tokenAmount = getTokenAmount(weiAmount);

    // Validation Checks
    // BK Ok
    preValidateChecks(_beneficiary, weiAmount, tokenAmount);
    
    // update state
    // BK Ok
    tokensIssued = tokensIssued.add(tokenAmount);
    // BK Ok
    weiRaised = weiRaised.add(weiAmount);

    // Send tokens from token wallet
    // BK Ok
    TOKEN.transferFrom(TOKEN_WALLET, _beneficiary, tokenAmount);

    // Forward the funds to wallet
    // BK Ok
    FUNDS_WALLET.transfer(msg.value);

    // Event trigger
    // BK Ok - Log event
    emit TokenPurchase(msg.sender, _beneficiary, weiAmount, tokenAmount);
  }

  /**
   * @dev Validation of an incoming purchase. Use require statements to revert state when conditions are not met. Use super to concatenate validations.
   * @param _beneficiary Address performing the token purchase
   * @param _weiAmount Value in wei involved in the purchase
   * @param _tokenAmount Amount of token to purchase
   */
  // BK Ok - Internal view function
  function preValidateChecks(address _beneficiary, uint256 _weiAmount, uint256 _tokenAmount) internal view {
    // BK Next 3 Ok
    require(_beneficiary != address(0));
    require(_weiAmount != 0);
    require(now >= START_DATE);
    require(!closed);

    // KYC Check
    // BK Ok
    validateWhitelisted(_beneficiary);

    // Test Min Investment
    // BK Ok
    require(_tokenAmount >= MIN_TOKEN_INVEST);

    // Test hard cap
    // BK Ok
    require(tokensIssued.add(_tokenAmount) <= MAX_TOKENS);
  }
}

```
