# **Manual Documentation for Aerochain**

## Introduction
AEROCHAIN is a decentralized, high-efficiency and energy-saving public
chain. It is compatible with smart contracts and supports high- performance
transactions.

## Consensus Mechanism
HPoS consensus mechanism: it has the characteristics of low transaction
cost, low transaction delay, and high transaction concurrency. The
maximum number of validators supported is 21.

## Performance 
TPS: 5000+
Average block interval: 3s

## Meta Transaction Function
The meta-transaction function is supported, which allows users to reduce
gas fees step-wise, and AEROCHAIN will cover the payment of the
reduced part. The meta-transaction function allows to minimize the
migration cost of DApp developers, as well as to effectively reduce the cost
of DApp users

## Consensus
* AEROCHAIN adopts HPoS consensus mechanism with low transaction
cost, low transaction latency, high transaction concurrency, and supports
up to 21 validators.

 * HPoS is a combination of PoA and Pos. To become a validator, you need to
submit a proposal first and wait for other active validators to vote on it, after
more than half of them pass, you will be eligible to become a validator. Any
address can stake to an address that qualifies to become a validator, and
after the validator's staking volume ranks in the top 21, it will become an
active validator in the next epoch.
All active verifiers are ordered according to predefined rules and take turns
to pack out blocks. If a validator fails to pack out a block in time in its own
round, the active validators who have not involved in the past n/2 (n is the
number of active validators) blocks will randomly perform the block-out. At
least n/2+1 active validators work properly to ensure the proper operation
of the blockchain.

* The difficulty value of a block is 2 when the block is generated normally and 1 when
the block is not generated in a predefined order. when a fork of the block chain
occurs, the block chain selects the corresponding fork according to the cumulative
maximum difficulty

## Glossary
* Validator : Responsible for packaging out blocks for on-chain transactions.
* Active Validator : The current set of validators responsible for packing out
blocks, with a maximum of 21.
* Epoch : Time interval in blocks, currently 1epoch = 200block. At the end of each
epoch, the blockchain interacts with the system contracts to update active
validators.

## System Contract
The management of the current validators are all done by the system contracts.
* Proposal Responsible for managing access to validators and managing validator proposals
and votes.
* Validators Responsible for ranking management of validators, staking and unstaking
operations, distribution of block rewards, etc..
* Punish Responsible for punishing operations against active validators who are not working
properly

Blockchain call system contracts：
* At the end of each block, the Validators contract is called and the fees for all transactions
in the block are distributed to active validators.
* The Punish contract is called to punish the validator when the validator is not working
properly.
* At the end of each epoch, the Validators contract is called to update active validators,
based on the ranking.

# Staking
For any account, any number of coins can be staked to the validator, and the minimum staking
amount for each validator is 32 AERO. If you want to unstake, you need to do the following:
1. Send an unstaking transaction for a validator to the Validators contract;
2. Waiting for 86400 blocks before sending a transaction to Validators contract to withdraw all
staking coins on this validator;

Blockchain call system contracts：
* At the end of each block, the Validators contract is called and the fees for all transactions
in the block are distributed to active validators.
* The Punish contract is called to punish the validator when the validator is not working
properly.
* At the end of each epoch, the Validators contract is called to update active validators,
based on the ranking.

## Genesis File
Both the mainnet and testnet genesis information of AEROCHAIN have been hardcoded in
blockchain, and the corresponding genesis files are listed below for verification.

## Glossary
* chainId : The unique identification of the chain.
* homesteadBlock eip150Block eip150Hash eip155Block eip158Block byzantiumBlock
constantinopleBlock petersburgBlock istanbulBlock muirGlacierBlock Hard fork height
configuration.
* congress Consensus parameters period is time interval of blocks. epoch is set for a period
in block, and at the end of each epoch, the validators are adjusted accordingly.
* number gasUsed parentHash nonce timestamp extraData gasLimit difficulty are all
parameters for genesis block.
* extraData The initial validators is set up here.
* alloc Configured initial account information that can be used for asset pre-allocation and
pre-initialization of system contracts.

  | Alloc        |  Purpose    |
  | -------------| ------------|
  | 000000000000000000000000000000000000f000 | //validators contract address |
  | 000000000000000000000000000000000000f001 | // punish contract address    |
  | 000000000000000000000000000000000000f002 | // proposal contract address  |

## Compile and Run
* Install Golang
Reference: Go [Download and install](https://golang.org/doc/install)
* Compile
cd /path/to/aerochain && make geth. If you want to use cross compile, like compiling on Mac for Linux, use make geth-linux, make
geth-linux-amd64, etc.
After compilation completed, the generated binary is in the folder build/bin.
* Genesis init

  | Environment  |     Code      |
  | -------------| ------------- |
  | [Testnet](https://testnet.aeroscan.id)  | ./build/bin/geth init ./genesis/testnet-genesis.json --datadir ./data/  |
  | [Mainnet](https://aeroscan.id)          | ./build/bin/geth init ./genesis/mainnet-genesis.json --datadir ./data/  |

* Run :
  -- ./build/bin/geth \
  --snapshot=false \
  --datadir data \
  --mine \
  --miner.etherbase="$COINBASE" \
  --miner.threads=$THREADS \
  --unlock "$COINBASE" \
  --password <(echo $PASSWORD) \
  --allow-insecure-unlock \
  --port $CHAINPORT \
  --http \
  --http.port $HTTPPORT \
  --http.addr "0.0.0.0" \
  --http.vhosts "*" \
  --http.api "eth,net,web3,admin,personal,txpool,debug" \
  --http.corsdomain "*"
  
  ## Explorer
  cd docker-compose && docker-compose up --build -d
  
  ## Faucet
  npm run build
env GO111MODULE=on go build -o eth-faucet
./eth-faucet -faucet.minutes 30 -httpport 8080 -wallet.provider
https://trpc.aerochain.id/ -wallet.privkey

