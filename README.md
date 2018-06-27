
## Simplified Payment Verification

The one field that we didn't investigate much in the last chapter is the Merkle Root field. The reason is that in order to understand why it's useful, we have to learn a few things about what Merkle Trees are and what properties they have. In this chapter, we're going to learn exactly what a Merkle Root is. This will be motivated by something called a Proof of Inclusion.

### Motivation

For a device that doesn't have much hard drive space, like your phone, it's hard to store the entire blockchain. As of this writing, the entire Bitcoin blockchain is around 161.5GB, which is a lot more than many phones can handle. If the entire blockchain cannot be put on the phone, what else can we do? Is it possible to create a Bitcoin wallet on a phone without having all the data?

There are two scenarios that we're concerned with. Paying someone and getting paid.

If you are paying someone with your Bitcoin wallet, it is up to the person receiving your Bitcoins to verify that indeed, they've received it. Once they've verified that the transaction has been included in a block, they'll give you the good or service you are expecting in return. Once you've sent the transaction to the other party, there really isn't anything for you to do than wait.

When getting paid Bitcoins, however, we have a dilemma. If we are connected and have the full blockchain, we don't need to worry as we can observe that the transaction paying us is in a block at which point we'd give them our goods or services. If we don't have the full blockchain, as with a phone, what can we do?

It turns out that the answer lies in the Merkle Root field. We can get just the block headers and get proof that a particular transaction is in a block that we know about. Since the block is secured by proof-of-work, we know that a proof of inclusion in that block is sufficient. The rest of this chapter goes into what the proof of inclusion looks like and how we both construct it and verify it.

A Merkle Tree is a computer science structure designed for efficient proofs of inclusion. The prerequisite is an ordered list of things, in our case, transactions, and a hash function, in our case, double-sha256. What we do is this:

1. Hash all the items of the ordered list with the provided hash function
2. If there is exactly 1 hash, we are done
3. If there is an odd number of hashes, we add a copy of the last hash in the list to the end so that we have two of the same ones.
4. We pair the hashes in order and hash the concatenation to get the parent level.
5. Go to 2.

The idea is to come to a single hash that represents all of the hashes. We'll go through each thing we need below.

### Merkle Parent

Given two hashes, we need some way to produce another hash that represents both of them. As they are ordered, we will call the two hashes the *left* hash and the *right* hash. Tho combination, we call the *parent* hash. The formula for getting the parent is pretty simple:

H = Hashing function, P = Parent Hash, L = Left Hash, R = Right Hash

P=H(L||R)

At this point, it's important to note that we can show that either the left or right hash is a "part" of the parent by revealing the other hash. That is, if we want to know that L is a part of P, someone can prove this is so, by revealing R and the fact that we're the left hash. We can combine L and R in the same way to produce P. This would prove that L and R are part of P.


```python
# Merkle Parent Example

from helper import double_sha256

tx_hash0 = bytes.fromhex('c117ea8ec828342f4dfb0ad6bd140e03a50720ece40169ee38bdc15d9eb64cf5')
tx_hash1 = bytes.fromhex('c131474164b412e3406696da1ee20ab0fc9bf41c8f05fa8ceea7a08d672d7cc5')

merkle_parent = double_sha256(tx_hash0+tx_hash1)
print(merkle_parent.hex())
```

### Exercise

#### Calculate the Merkle parent of these hashes:
```
f391da6ecfeed1814efae39e7fcb3838ae0b02c02ae7d0a5848a66947c0727b0
3d238a92a94532b946c90e19c49351c763696cff3db400485b813aecb8a13181
```


```python
from helper import double_sha256

hex_hash1 = 'f391da6ecfeed1814efae39e7fcb3838ae0b02c02ae7d0a5848a66947c0727b0'
hex_hash2 = '3d238a92a94532b946c90e19c49351c763696cff3db400485b813aecb8a13181'

# bytes.fromhex to get the bin hashes
# double_sha256 the combination
# hex() to see the result
```


```python
from io import BytesIO
from block import Block, Proof

def merkle_parent(hash1, hash2):
    '''Takes the binary hashes and calculates the double-sha256'''
    # return the double-sha256 of hash1 + hash2
    pass
```
