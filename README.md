# saksh-eth-wallet

A package to generate Ethereum addresses, monitor payments, send ETH, get transaction reports, and check balances in ETH and USD.

## Installation

To install the package, use npm:

```bash
npm install saksh-eth-wallet
```



 

### Usage
Here's an example of how to use the package:

```

const {
    sakshSetConfig,
    sakshGenerateMnemonic,
    sakshCreateUser,
    sakshGenerateAddressForUser,
    sakshGetUserAddresses,
    sakshGetETHBalance,
    sakshGetETHBalanceInUSD,
    sakshGetUserBalances,
    sakshGetUsersWithBalance,
    sakshMonitorAddresses,
    sakshSetGlobalCallback
} = require('saksh-eth-wallet'); // Replace with the actual file name

// Example usage
async function exampleUsage() {
    // Set the configuration
    sakshSetConfig(() => ({
        MONGODB_URI: 'your_mongodb_connection_string',
        ALCHEMY_API_KEY: 'your_alchemy_api_key',
        COINLAYER_API_KEY: 'your_coinlayer_api_key'
    }));

    // Generate a new mnemonic
    const mnemonic = sakshGenerateMnemonic();
    console.log('Generated Mnemonic:', mnemonic);

    // Create a new user
    const userId = 'user123';
    const user = await sakshCreateUser(userId);
    console.log('Created User:', user);

    // Define a callback function
    const callback = function (transaction) {
        console.log('Payment received:', transaction);
        // Add your notification logic here (e.g., send an email or SMS)
    };

    // Generate an address for the user with a callback function
    const addressData = await sakshGenerateAddressForUser(userId, "m/44'/60'/0'/0/0", callback);
    console.log('Generated Address:', addressData);

    // Set a global callback function
    sakshSetGlobalCallback(function (transaction) {
        console.log('Global callback - Payment received:', transaction);
        // Add your global notification logic here
    });

    // Get all addresses for the user
    const addresses = await sakshGetUserAddresses(userId);
    console.log('User Addresses:', addresses);

    // Get the ETH balance of an address
    const ethBalance = await sakshGetETHBalance(addressData.address);
    console.log('ETH Balance:', ethBalance);

    // Get the ETH balance in USD for an address
    await sakshGetETHBalanceInUSD(addressData.address, (balance) => {
        console.log('ETH Balance in USD:', balance);
    });

    // Get the balances for all addresses of the user
    await sakshGetUserBalances(userId, (balances) => {
        console.log('User Balances:', balances);
    });

    // Get user IDs with at least one address having a balance greater than zero
    const usersWithBalance = await sakshGetUsersWithBalance();
    console.log('Users with Balance:', usersWithBalance);

    // Monitor addresses and send notifications
    sakshMonitorAddresses();
}

exampleUsage().catch(console.error);

```


#### Functions
 
 
## sakshSetConfig(callback)

Sets the configuration for the package. The callback function should return an object containing the necessary configuration data.

### Parameters:
- **callback** (Function): A function that returns an object with the following properties:
  - **MONGODB_URI** (String): MongoDB connection string.
  - **ALCHEMY_API_KEY** (String): Alchemy API key.
  - **COINLAYER_API_KEY** (String): Coinlayer API key.

### Example:
```javascript
sakshSetConfig(() => ({
  MONGODB_URI: 'your_mongodb_connection_string',
  ALCHEMY_API_KEY: 'your_alchemy_api_key',
  COINLAYER_API_KEY: 'your_coinlayer_api_key'
}));
```

---

## sakshGenerateMnemonic()

Generates a new mnemonic phrase.

### Returns:
- **String**: A new mnemonic phrase.

### Example:
```javascript
const mnemonic = sakshGenerateMnemonic();
console.log('Generated Mnemonic:', mnemonic);
```

---

## sakshCreateUser(userId)

Creates a new user with a mnemonic.

### Parameters:
- **userId** (String): The user ID.

### Returns:
- **Object**: The created user object.

### Example:
```javascript
const user = await sakshCreateUser('user123');
console.log('Created User:', user);
```

---

## sakshGenerateAddressForUser(userId, path, callback)

Generates an address for a given user ID.

### Parameters:
- **userId** (String): The user ID.
- **path** (String, optional): The derivation path (default: "m/44'/60'/0'/0/0").
- **callback** (Function, optional): A callback function to be called when a payment is received.

### Returns:
- **Object**: The generated address data.

### Example:
```javascript
const addressData = await sakshGenerateAddressForUser('user123', "m/44'/60'/0'/0/0", function(transaction) {
  console.log('Payment received:', transaction);
});
console.log('Generated Address:', addressData);
```

---

## sakshGetUserAddresses(userId)

Gets all addresses for a given user ID.

### Parameters:
- **userId** (String): The user ID.

### Returns:
- **Array**: An array of address objects.

### Example:
```javascript
const addresses = await sakshGetUserAddresses('user123');
console.log('User Addresses:', addresses);
```

---

## sakshGetETHBalance(address)

Gets the ETH balance of an address.

### Parameters:
- **address** (String): The Ethereum address.

### Returns:
- **String**: The ETH balance.

### Example:
```javascript
const ethBalance = await sakshGetETHBalance('0xYourAddress');
console.log('ETH Balance:', ethBalance);
```

---

## sakshGetETHBalanceInUSD(address, callback)

Gets the ETH balance in USD for an address.

### Parameters:
- **address** (String): The Ethereum address.
- **callback** (Function): A callback function to handle the balance data.

### Example:
```javascript
await sakshGetETHBalanceInUSD('0xYourAddress', (balance) => {
  console.log('ETH Balance in USD:', balance);
});
```

---

## sakshGetUserBalances(userId, callback)

Gets the balances for all addresses of a user.

### Parameters:
- **userId** (String): The user ID.
- **callback** (Function): A callback function to handle the balances data.

### Example:
```javascript
await sakshGetUserBalances('user123', (balances) => {
  console.log('User Balances:', balances);
});
```

---

## sakshGetUsersWithBalance()

Gets user IDs with at least one address having a balance greater than zero.

### Returns:
- **Array**: An array of user IDs.

### Example:
```javascript
const usersWithBalance = await sakshGetUsersWithBalance();
console.log('Users with Balance:', usersWithBalance);
```

---

## sakshMonitorAddresses()

Monitors addresses and sends notifications when a payment is received. Calls both the address-specific and global callback functions.

### Example:
```javascript
sakshMonitorAddresses();
```

---

## sakshSetGlobalCallback(callback)

Sets a global callback function that will be called whenever a payment is received.

### Parameters:
- **callback** (Function): A callback function to handle the transaction data.

### Example:
```javascript
sakshSetGlobalCallback(function(transaction) {
  console.log('Global callback - Payment received:', transaction);
});
```
 

This formatting uses headings, lists, and code blocks to improve readability and organization, making it suitable for GitHub documentation.






### License
This project is licensed under the MIT License.


### Support
Susheel2339 at gmail.com
