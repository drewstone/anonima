# Anonima
Decentralized Ethereum Anonymous Voting Protocol derived from barryWhiteHat's [Mixmius](https://github.com/barryWhiteHat/miximus)

## How it works
When someone sends 1 ether to the `deposit` function in miximus.sol they append single leaf to the merkle tree. 

Afterwards someone who has the secret key (sk) and `nullifier` of the leaf of the merkle tree is allowed to submit a vote. Instead of revealing the information to prove that they control it. They (using a zksnark) produce a proof that they know this information without revealing it. They also create a proof that their leaf is in the merkle tree. 

When they verify this proof they reveal the nullifier, but not the sk. So no one is able to tell which nullifier maps to which leaf.

To prevent double votes the smart contract tracks the nullifiers and only allows a single withdrawal per nullifiers. 


## build instructions:



### build libsnark gadget and getting the proving key
get dependencies `git submodule update --init --recursive`
`mkdir build` 
`cd build`
`cmake .. && make`

Finally you will need to download the ~400MB proving key from [here](https://github.com/barryWhiteHat/miximus/releases/download/untagged-5e043815d553302be2d2/rinkeby_vk_pk.tar.gz), unzip it and save it in the `./zksnark_element` directory.

### Running the Mixmius tests
Start your prefered ethereum node, `cd tests` and run `python3 test.py` This will 
1. Generate verification keys, proving keys, This step takes a lot of ram and its likely your OS will kill it if you have a bunch of windows open.
2. deploy the contract
3. Deposit 32 ether in 1 ether chunks.
4. Withdraw the 32 eth so that an observer cannot tell which deposit it was based upon. 

