# EnergisToken

Source file [../contracts_flat/EnergisToken.sol](../contracts_flat/EnergisToken.sol).

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
  // BK Ok
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
  // BK Ok - Only owner can execute. This function is overriden
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
  // BK Ok - Pure function
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
  // BK Ok - Pure function
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
  // BK Ok - Pure function
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    // BK Ok
    assert(b <= a);
    // BK Ok
    return a - b;
  }

  /**
  * @dev Adds two numbers, throws on overflow.
  */
  // BK Ok - Pure function
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

// File: zeppelin-solidity/contracts/token/ERC20/BasicToken.sol

/**
 * @title Basic token
 * @dev Basic version of StandardToken, with no allowances.
 */
// BK Ok
contract BasicToken is ERC20Basic {
  // BK Ok
  using SafeMath for uint256;

  // BK Ok
  mapping(address => uint256) balances;

  // BK Ok
  uint256 totalSupply_;

  /**
  * @dev total number of tokens in existence
  */
  // BK Ok - View function
  function totalSupply() public view returns (uint256) {
    // BK Ok
    return totalSupply_;
  }

  /**
  * @dev transfer token for a specified address
  * @param _to The address to transfer to.
  * @param _value The amount to be transferred.
  */
  // BK Ok
  function transfer(address _to, uint256 _value) public returns (bool) {
    // BK Ok
    require(_to != address(0));
    // BK Ok
    require(_value <= balances[msg.sender]);

    // SafeMath.sub will throw if there is not enough balance.
    // BK Ok
    balances[msg.sender] = balances[msg.sender].sub(_value);
    // BK Ok
    balances[_to] = balances[_to].add(_value);
    // BK Ok - Log event
    emit Transfer(msg.sender, _to, _value);
    // BK Ok
    return true;
  }

  /**
  * @dev Gets the balance of the specified address.
  * @param _owner The address to query the the balance of.
  * @return An uint256 representing the amount owned by the passed address.
  */
  // BK Ok - View function
  function balanceOf(address _owner) public view returns (uint256 balance) {
    // BK Ok
    return balances[_owner];
  }

}

// File: zeppelin-solidity/contracts/token/ERC20/ERC20.sol

/**
 * @title ERC20 interface
 * @dev see https://github.com/ethereum/EIPs/issues/20
 */
// BK Ok
contract ERC20 is ERC20Basic {
  // BK Next 3 Ok
  function allowance(address owner, address spender) public view returns (uint256);
  function transferFrom(address from, address to, uint256 value) public returns (bool);
  function approve(address spender, uint256 value) public returns (bool);
  // BK Ok - Event
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: zeppelin-solidity/contracts/token/ERC20/StandardToken.sol

/**
 * @title Standard ERC20 token
 *
 * @dev Implementation of the basic standard token.
 * @dev https://github.com/ethereum/EIPs/issues/20
 * @dev Based on code by FirstBlood: https://github.com/Firstbloodio/token/blob/master/smart_contract/FirstBloodToken.sol
 */
// BK Ok
contract StandardToken is ERC20, BasicToken {

  // BK Ok
  mapping (address => mapping (address => uint256)) internal allowed;


  /**
   * @dev Transfer tokens from one address to another
   * @param _from address The address which you want to send tokens from
   * @param _to address The address which you want to transfer to
   * @param _value uint256 the amount of tokens to be transferred
   */
  // BK Ok
  function transferFrom(address _from, address _to, uint256 _value) public returns (bool) {
    // BK Next 3 Ok
    require(_to != address(0));
    require(_value <= balances[_from]);
    require(_value <= allowed[_from][msg.sender]);

    // BK Ok
    balances[_from] = balances[_from].sub(_value);
    // BK Ok
    balances[_to] = balances[_to].add(_value);
    // BK Ok
    allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_value);
    // BK Ok - Log event
    emit Transfer(_from, _to, _value);
    // BK Ok
    return true;
  }

  /**
   * @dev Approve the passed address to spend the specified amount of tokens on behalf of msg.sender.
   *
   * Beware that changing an allowance with this method brings the risk that someone may use both the old
   * and the new allowance by unfortunate transaction ordering. One possible solution to mitigate this
   * race condition is to first reduce the spender's allowance to 0 and set the desired value afterwards:
   * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
   * @param _spender The address which will spend the funds.
   * @param _value The amount of tokens to be spent.
   */
  // BK Ok
  function approve(address _spender, uint256 _value) public returns (bool) {
    // BK Ok
    allowed[msg.sender][_spender] = _value;
    // BK Ok - Log event
    emit Approval(msg.sender, _spender, _value);
    // BK Ok
    return true;
  }

  /**
   * @dev Function to check the amount of tokens that an owner allowed to a spender.
   * @param _owner address The address which owns the funds.
   * @param _spender address The address which will spend the funds.
   * @return A uint256 specifying the amount of tokens still available for the spender.
   */
  // BK Ok - View function
  function allowance(address _owner, address _spender) public view returns (uint256) {
    // BK Ok
    return allowed[_owner][_spender];
  }

  /**
   * @dev Increase the amount of tokens that an owner allowed to a spender.
   *
   * approve should be called when allowed[_spender] == 0. To increment
   * allowed value is better to use this function to avoid 2 calls (and wait until
   * the first transaction is mined)
   * From MonolithDAO Token.sol
   * @param _spender The address which will spend the funds.
   * @param _addedValue The amount of tokens to increase the allowance by.
   */
  // BK Ok
  function increaseApproval(address _spender, uint _addedValue) public returns (bool) {
    // BK Ok
    allowed[msg.sender][_spender] = allowed[msg.sender][_spender].add(_addedValue);
    // BK Ok - Log event
    emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
    // BK Ok
    return true;
  }

  /**
   * @dev Decrease the amount of tokens that an owner allowed to a spender.
   *
   * approve should be called when allowed[_spender] == 0. To decrement
   * allowed value is better to use this function to avoid 2 calls (and wait until
   * the first transaction is mined)
   * From MonolithDAO Token.sol
   * @param _spender The address which will spend the funds.
   * @param _subtractedValue The amount of tokens to decrease the allowance by.
   */
  // BK Ok
  function decreaseApproval(address _spender, uint _subtractedValue) public returns (bool) {
    // BK Ok
    uint oldValue = allowed[msg.sender][_spender];
    // BK Ok
    if (_subtractedValue > oldValue) {
      // BK Ok
      allowed[msg.sender][_spender] = 0;
    // BK Ok
    } else {
      // BK Ok
      allowed[msg.sender][_spender] = oldValue.sub(_subtractedValue);
    }
    // BK Ok - Log event
    emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
    // BK Ok
    return true;
  }

}

// File: contracts/EnergisToken.sol

/**
 * @title EnergisToken
 * 
 * Symbol      : ERG
 * Name        : Energis Token
 * Total supply: 240,000,000.000000000000000000
 * Decimals    : 18
 *
 * (c) Philip Louw / Zero Carbon Project 2018. The MIT Licence.
 */
// BK Ok
contract EnergisToken is StandardToken, Claimable {
  // BK Ok
  using SafeMath for uint256;

  // BK Ok
  string public constant name = "Energis Token"; // solium-disable-line uppercase
  // BK Ok
  string public constant symbol = "NRG"; // solium-disable-line uppercase
  // BK Ok
  uint8 public constant decimals = 18; // solium-disable-line uppercase

  // BK Ok
  uint256 public constant INITIAL_SUPPLY = 240000000 * (10 ** uint256(decimals));

  /**
   * @dev Constructor that gives msg.sender all of existing tokens.
   */
  // BK Ok - Constructor
  function EnergisToken() public {
    // BK Ok
    totalSupply_ = INITIAL_SUPPLY;
    // BK Ok
    balances[msg.sender] = INITIAL_SUPPLY;
    // BK Ok - Log event
    emit Transfer(0x0, msg.sender, INITIAL_SUPPLY);
  }

  /**
  * @dev Reject all ETH sent to token contract
  */
  // BK Ok - Revert transfers of ETH
  function () public payable {
    // Revert will return unused gass throw will not
    // BK Ok
    revert();
  }

  /**
   * @dev Owner can transfer out any accidentally sent ERC20 tokens
   */
  // BK Ok - Only owner can execute
  function transferAnyERC20Token(address tokenAddress, uint tokens) public onlyOwner returns (bool success) {
    // BK Ok
    return ERC20Basic(tokenAddress).transfer(owner, tokens);
  }  
}

```
