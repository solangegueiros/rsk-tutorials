# Developing smart contracts using TDD in Truffle 

Smart contracts are not a good fit for “move fast and break things” kind of mindset. Blockchain is immutable in the sense that a once approved transaction is going to stay.

Contracts are part of a public network and expose their vulnerabilities to any attacker, and this becomes even more critical when it is related to money. This is why the practice of TDD is one of our best friends when developing our smart contracts.

# What is TDD - Test Driven Development?

Test-Driven development is a process of developing and running automated test before developing the application itself.
Hence, TDD sometimes also called Test-First Development.

# How to perform TDD Test

1. Add a test
2. Run all tests and see if any new test fails
3. Write some code
4. Run tests and Refactor code
5. Repeat


Truffle comes with built in Mocha test framework and Chai assertion library.


# Create the election project

1. Create a new folder named `Election`;
2. Navigate to the folder in the terminal;
3. Initialize an empty Truffle project;
4. Initialize an npm project

```shell
mkdir Election
cd Election
truffle init
npm init -y
```

I am using Windows OS and I will do it in the `C:\RSK` folder:

**truffle init**

![truffle init](/../../images/truffle-tdd/image-01.png)

**npm init**
![npm init](/../../images/truffle-tdd/image-02.png)
 
### Truffle Assertions
 
[Truffle assertions](https://www.npmjs.com/package/truffle-assertions) include additional assertions which can be used to test Ethereum smart contracts inside Truffle tests. This npm package is very helpful! 
 
Install it using the terminal:
 
```shell
npm i truffle-assertions
```

Open the project in VS Code.

```shell
code .
```

# Configure Truffle to connect to RSK local node

Open `truffle-config.js` file in your Truffle project and overwrite it with the following code:

```javascript
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 4444,
      network_id: "*"
    },
  },
  compilers: {
    solc: {
      version: "0.5.16",
    }
  }
}
```

Check out the VS Code image:

![truffle-config](/../../images/truffle-tdd/image-03.png)


# Create testing script

We will start creating a testing script.

In the `test` folder, create a file named `electionTest.js`

Copy and paste this source code:

```javascript
const Election = artifacts.require('Election');
const truffleAssertions = require('truffle-assertions');

contract('Election', function(accounts) {

  it('should deploy contract', async () => {
    await Election.new();
  });

});
```

![truffle create test script](/../../images/truffle-tdd/image-04.png)

## Attempt 1 to run tests

In the terminal, run this command:

```
truffle test
```

## Test error

Oh! I've got an error!

![Attempt 1 test error](/../../images/truffle-tdd/image-05.png)

This test is not passing because we don't have the smart contract `Election.sol` created yet.
So, let's create it.

# Create the smart contract

In your terminal, inside the project folder, run this command:

```shell
truffle create contract Election
```

This command creates a 'skeleton' smart contract. Take a look in the results in the `contracts` folder:

![truffle create contract Election](/../../images/truffle-tdd/image-06.png)

## Attempt 2 to run tests

Try again:

```
truffle test
```

![Attempt 2 test ok](/../../images/truffle-tdd/image-07.png)


If you got it right, contracts should compile without any errors. 
But we still don’t have any tests. 

# Deploy on before each

I will extract deployment from test because I'd like to re-deploy the contract for each test. 
This is how we make it work as a unit test module.

```javascript
const Election = artifacts.require('Election');
const truffleAssertions = require('truffle-assertions');

contract('Election', function(accounts) {

  before('New instance before each test cases', async function() {
    instanceElection = await Election.new();
  })

  it('should deploy contract', async () => {    
  });

});
```

![deploy on before each](/../../images/truffle-tdd/image-08.png)

## Attempt 3 to run tests

Check it out:

```
truffle test
```

![Attempt 3 test ok](/../../images/truffle-tdd/image-09.png)


# The candidates

Rules for candidates:

- We have 2 candidates in this election, like `Biden` and `Trump`.
- Both must starts with 0 votes.

So let's create tests for these rules.

## Tests for constructor

Copy and paste in the file `test\electionTest.js`:

```javascript
const Election = artifacts.require('Election');
const truffleAssertions = require('truffle-assertions');

contract('Election', function(accounts) {

  before('New instance before each test cases', async function() {
    instanceElection = await Election.new();
  })

  describe('Constructor', function () {
    it('Candidate 1 is Biden', async function () {
      assert.equal(await instanceElection.candidate1(), "Biden");
    });

    it('Candidate 2 is Trump', async function () {
      assert.equal(await instanceElection.candidate2(), "Trump");
    });

    it('Candidate 1 starts with no votes', async function () {
      assert.equal(await instanceElection.votes1(), 0);
    });

    it('Candidate 2 starts with no votes', async function () {
      assert.equal(await instanceElection.votes2(), 0);
    });
  })

});
```

![Creating tests for constructor](/../../images/truffle-tdd/image-10.png)

## Test 4: test constructor

If you execute `truffle test` now, you will get an error because these rules are not implemented in the smart contract.

```
truffle test
```

![Test 4: test constructor error](/../../images/truffle-tdd/image-11.png)

## Adding the constructor

Let's add the constructor function in the smart contract `contracts\Election.sol`:

```javascript
//SPDX-License-Identifier: MIT
pragma solidity >= 0.5.0 < 0.7.0;

contract Election {
  string public candidate1;
  string public candidate2;
  uint public votes1;
  uint public votes2;

  constructor() public {
    candidate1 = "Biden";
    candidate2 = "Trump";
  }
}
```

![Add constructor](/../../images/truffle-tdd/image-12.png)

## Test 5: test constructor ok

Run the tests again and check out the result:

```
truffle test
```

![Test 5: test constructor ok](/../../images/truffle-tdd/image-13.png)

The tests passed :)

# Voting

Rules for voting:

- Each address can vote once.

So let's create tests for this rule.

## Tests for voting

In the file `test\electionTest.js`, add this part after end of last `describe`:

```javascript
  describe('Voting process', function () {
    it('Vote count should increase', async function () {
      let countBefore = (await instanceElection.votes1()).toNumber();
      await instanceElection.doVote("Biden", {from : accounts[0]});
      let countAfter = (await instanceElection.votes1()).toNumber();
      assert.isAbove(countAfter, countBefore);
    });

    it('Should not vote twice', async function () {
      await truffleAssertions.fails( instanceElection.doVote("Biden", {from : accounts[0]}) );
    });
  })
```

![Creating tests for voting](/../../images/truffle-tdd/image-14.png)

## Test 6: test voting error

Executing `truffle test` now, you will get an error because first we need to add the `doVote` functions and the stuffe related to this.

```
truffle test
```

![Test 6: test voting error](/../../images/truffle-tdd/image-15.png)

## Adding the voting rules

Let's add the voting rules in the smart contract `contracts\Election.sol`:

```javascript
pragma solidity >= 0.5.0 < 0.7.0;

contract Election {
  string public candidate1;
  string public candidate2;
  uint public votes1;
  uint public votes2;

  mapping (address => bool) public hasVoted;

  constructor() public {
    candidate1 = "Biden";
    candidate2 = "Trump";
  }

  modifier notVoted() {
    require(hasVoted[msg.sender] == false, "Already voted");
    _;
  }

  function doVote(string memory _candidate) public notVoted {
    if (compareStrings(_candidate, "Biden")) {
      votes1++;
    } else if (compareStrings(_candidate, "Trump")) {
      votes2++;
    }
    hasVoted[msg.sender] = true;
  }   

  function compareStrings(string memory a, string memory b) private pure returns (bool) {
    return (keccak256(abi.encodePacked((a))) == keccak256(abi.encodePacked((b))));
  }

}
```

![Add voting rules](/../../images/truffle-tdd/image-16.png)

## Test 7: test voting ok

Now the tests will run succesfuly:

```
truffle test
```

![Test 7: test voting ok](/../../images/truffle-tdd/image-17.png)









```javascript

```



# Final considerations

In this tutorial I started to present you how to develop smart contracts applying TDD techniques while using Truffle framework to run tests on smart contracts, connected to a RSK local node. 
There are more rules and tests to finalize our Election project that were not covered in this tutorial.

Our goal is to join forces and give options to people who believe in the power of smart contracts, and also believe in the security of Bitcoin, through RSK.

I hope this tutorial has been helpful and I’d appreciate your feedback. 
Happy with this tutorial? 
Share it if you like it :)










## Election.sol

This smart contract has:

* A variable `info` to store a string
* A function `getInfo()` to return the string stored at variable info
* A function `setInfo()` to change the string stored at variable info





