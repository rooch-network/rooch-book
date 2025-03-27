# Transaction Hash Explained
## What is a Transaction Hash?

A Transaction Hash is a unique identifier used to identify a specific transaction on the blockchain. It is a fixed-length string obtained by hashing the transaction data.

## Components

A typical transaction hash is the result of hashing the following information:

1. Basic Transaction Information

- Sender address
- Receiver address
- Transaction amount
- Gas fee
- Nonce (transaction sequence number)

2. Contract-related Information (if it is a contract transaction)

- Contract address
- Function call data
- Parameters

## Actual Example

```move
# A typical transaction hash
0x7d013a39fbf123e54b00f3a33c3f7f4d33057fb28d05e707aa39aeea83367ad0
```

## Uses

1. Transaction Tracking

- Query transaction status
- Confirm whether the transaction was successful
- View transaction details

2. Transaction Verification

- Ensure transaction integrity
- Prevent transaction tampering

3. Transaction Reference

- Reference specific transactions in applications
- Associate related operations

## Using in Rooch

```ts
// Query transaction details
const txInfo = await client.getTransaction({
    hash: "0x7d013a39fbf123e54b00f3a33c3f7f4d33057fb28d05e707aa39aeea83367ad0"
});

// Wait for transaction completion
await client.waitForTransaction({
    hash: "0x7d013a39fbf123e54b00f3a33c3f7f4d33057fb28d05e707aa39aeea83367ad0"
});
```

## Notes

- Transaction hashes are unique
- Hash values are irreversible
- Any change in transaction data will result in a completely different hash value
- Transaction hashes are usually used as receipts or proofs of transactions