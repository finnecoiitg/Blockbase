# BlockBase — Full 8-Day Course Outline
**Mentored by FEC**
*From zero to dApp — scratch to deployment*

---

> **How to read this document**
> Each day follows the same structure:
> - **Theme** — the one idea the day is built around
> - **Theory** — content to teach / explain (lecture + discussion)
> - **🔗 Interaction** — external tool, demo, or explorer the student uses live
> - **💻 Lab** — guided coding task done together
> - **🎯 Challenge** — optional extension for fast finishers
> - **Takeaway** — the one sentence a student should be able to say at the end

---

## Day 1 — What is a Blockchain?

**Theme:** Understanding the problem blockchain solves, how it works mechanically — hashing, blocks, nodes, and consensus — completely.

> **Instructor note:** This is the only day dedicated purely to blockchain fundamentals. Everything taught here must land properly — students won't get another chance to ask "but how does it actually work?" after this. Take time on each section. Ask questions back. The interactions are essential, not optional.

---

### Theory

---

#### Part 1 — The Problem: Why Does Blockchain Exist?

Start with a story, not a definition.

> *Imagine you want to send ₹5,000 to a friend in another city. You open your banking app, type their UPI ID, and hit send. It works — but only because you both trust the same bank to honestly update two numbers in a database. The bank deducted ₹5,000 from your account and added it to your friend's. Simple.*
>
> *Now imagine: what if there was no bank? What if you needed to send money to someone you've never met, in a country you've never visited, using a currency no government controls — and neither of you trusts the other? How do you make sure the money was actually sent? And how do you make sure you didn't secretly send the same money to two people at the same time?*

This second problem — sending the same money to two people simultaneously — is called the **double-spend problem**. It had been an unsolved problem in computer science for decades. Every digital payment system before 2009 needed a trusted middleman (a bank, PayPal, Visa) to prevent it.

In 2008, someone solved it — without a middleman.

---

#### Part 2 — The Bitcoin Origin Story

In October 2008, an anonymous person (or group) operating under the pseudonym **Satoshi Nakamoto** published a 9-page paper titled:

> *"Bitcoin: A Peer-to-Peer Electronic Cash System"*

Nobody knows who Satoshi is. They could be one person or a group of people. They could be living anywhere. Despite Bitcoin being worth trillions of dollars today, Satoshi has never come forward. Their identity remains one of the great mysteries of the internet age.

Satoshi's core insight: *What if instead of one bank keeping the ledger, everyone kept a copy of it?*

If thousands of computers around the world all have the same record, no single person can secretly tamper with it — because they'd have to change everyone else's copy simultaneously, and everyone would immediately notice the discrepancy.

**Bitcoin launched in January 2009.** The first block — called the **Genesis Block** — was mined by Satoshi themselves. Embedded in that block was a message that will exist on the Bitcoin blockchain forever:

> *"The Times 03/Jan/2009 Chancellor on brink of second bailout for banks"*

It was a newspaper headline from that exact day. A quiet political statement: the old financial system is failing. Here is something new.

The first ever Bitcoin transaction: Satoshi sent 10 BTC to **Hal Finney**, a cryptographer and one of the earliest Bitcoin contributors. Hal received the first Bitcoin transaction ever made. (Hal Finney passed away in 2014 from ALS. He had his body cryonically preserved — he wanted to see how the future turned out.)

---

#### Part 3 — What is a Blockchain?

A **blockchain** is a distributed, digital, immutable ledger used to record transactions and store data securely and transparently.

Break that down word by word:

| Word | Meaning |
|---|---|
| **Distributed** | Thousands of computers around the world each hold a full, identical copy |
| **Digital** | It exists only as data — no physical form |
| **Immutable** | Once a record is written, it cannot be changed or deleted — ever |
| **Ledger** | It's a record-keeping book — specifically, a record of every transaction that ever happened |

**State management:** Blockchains start with a **Genesis State** (block 0) and every transaction that ever occurs updates that state. The current state of the blockchain can be recalculated at any time by starting from the genesis block and replaying every transaction in order. This is how nodes verify the chain.

---

#### Part 4 — What is a Hash?

This is the technical foundation everything else builds on. Understand this and the rest becomes obvious.

A **hash function** takes any input — a word, a number, an entire book, a video file — and produces a fixed-length output called a **hash** (or digest). The function has three crucial properties:

**1. Deterministic** — The same input always produces the same output, every time, forever.

**2. Avalanche effect** — A tiny change in input produces a completely unrecognisable output. Not a slightly different output. A completely different one.

**3. One-way** — Given a hash, you cannot mathematically reverse it to find the original input. The only way to "crack" it is to try inputs until one produces the right hash.

Bitcoin uses **SHA-256** (Secure Hash Algorithm 256-bit). Output is always exactly 64 hexadecimal characters, no matter what you put in.

```
Input: "Hello"
SHA-256: 185f8db32921bd46d35fa8acbcd57c8dee7a567e37e1e3ddc63edd3...

Input: "hello"   ← just changed the capital H
SHA-256: 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043...
```

Completely different. One character changed. That is the avalanche effect in action.

**Why does this matter for blockchain?** Each block contains the hash of the previous block. If someone tampers with block 5, block 5's hash changes. But block 6 stores block 5's hash — so block 6 is now wrong too. Which makes block 7 wrong. The whole chain from that point breaks. Tampering is immediately visible.

---

#### Part 5 — What's Inside a Block?

A block contains:
- **Block number** (also called height) — its position in the chain
- **Timestamp** — when it was created
- **Transactions** — a list of all the transactions grouped in this block
- **Nonce** — a special number used in mining (explained below)
- **Hash of the previous block** — the link that creates the "chain"
- **Its own hash** — computed from all of the above

```
Block #847,231
├── Timestamp: 2024-03-15 14:22:07
├── Transactions: [Alice→Bob: 0.5 BTC, Carol→Dave: 1.2 BTC, ...]
├── Previous Hash: 000000000000000abc123...
├── Nonce: 3,847,291
└── This Block's Hash: 000000000000000def456...
```

Notice the hash starts with many zeros. That's not a coincidence. That's the result of mining.

---

#### Part 6 — What is Mining? (Proof of Work)

Here is the question nobody asks but everyone should: *Who decides which transactions go into a block? And why would anyone do that work for free?*

The answer is **mining**, and it operates through a mechanism called **Proof of Work (PoW)**.

**The problem mining solves:** In a distributed network, many computers want to add the next block simultaneously. They can't all add different blocks — that would create conflicting histories. The network needs exactly one agreed-upon next block. How do you choose?

**Bitcoin's answer:** Make it a competition. The first computer to solve a hard mathematical puzzle gets to add the next block and earns a **reward** (new Bitcoin). Everyone else accepts that block and the competition resets.

**What is the puzzle?**

Find a number — called the **nonce** — such that when you combine the nonce with the block's data and hash it all together, the resulting hash starts with a specific number of zeros.

```
Hash(block data + nonce) must start with 0000000000...
```

There's no clever way to solve this. You just try nonces one by one — 1, 2, 3, ... 10,000 ... 3,847,291 — until you find one that works. Modern Bitcoin miners try **billions of nonces per second**.

**Why is this useful?** Because it's:
- **Hard to do** — requires enormous computational work
- **Easy to verify** — anyone can check `Hash(block + nonce)` starts with enough zeros in one calculation

This asymmetry is the genius of Proof of Work. Cheating is computationally expensive. Verifying is essentially free.

**The 51% Attack:** To rewrite blockchain history, you'd need to redo all the computational work for every block you want to change — and do it faster than the entire honest network is adding new blocks. For Bitcoin, the honest network has more computing power than the world's top 500 supercomputers combined. Attacking it is theoretically possible but practically impossible. This is the **51% attack** — you'd need to control 51% of the network's total computing power.

**Mining reward:** When a miner wins the block competition, they receive newly created Bitcoin. This is how new Bitcoin enters circulation. Bitcoin's total supply is capped at **21 million BTC** — hardcoded into the protocol. When all 21 million are mined (around 2140), miners will earn only transaction fees. The reward halves every 210,000 blocks (~4 years) — this is called the **halving**.

**Energy criticism:** Proof of Work is intentionally energy-intensive. The computational difficulty is the security. Bitcoin's network consumes roughly as much electricity as a medium-sized country. This is controversial, and it's one of the reasons Ethereum moved away from PoW.

---

#### Part 7 — Proof of Stake (How Ethereum Works Now)

In September 2022, Ethereum completed one of the most significant upgrades in blockchain history: **The Merge**. Ethereum switched from Proof of Work to **Proof of Stake (PoS)**.

**The difference:**

| | Proof of Work | Proof of Stake |
|---|---|---|
| Who adds blocks? | Whoever solves the puzzle first (miners) | Randomly chosen from those who locked up ETH (validators) |
| How do you participate? | Buy mining hardware | Lock up (stake) 32 ETH as collateral |
| Energy use | Extremely high | ~99.95% less than PoW |
| Security mechanism | Attacking costs computing power | Attacking costs your staked ETH (slashing) |
| Used by | Bitcoin (still PoW) | Ethereum (since Sept 2022) |

**How PoS works on Ethereum:**
1. To become a **validator**, you lock up 32 ETH as collateral
2. Validators are randomly selected to propose the next block
3. Other validators **attest** (vote) that the block is valid
4. The block is added. The proposer earns rewards.
5. If a validator tries to cheat — propose a fraudulent block — their staked ETH is **slashed** (destroyed). This is the punishment that replaces the energy cost of PoW.

**Why did Ethereum switch?** Three reasons:
- Energy consumption — PoS uses 99.95% less electricity
- Scalability — PoS is a stepping stone to future scaling improvements
- Security economics — slashing makes attacks economically self-destructive

**Does PoS mean Ethereum is less secure?** This is debated. The security model is different, not necessarily weaker. Attacking Ethereum PoS requires acquiring enormous amounts of ETH (which costs money) and then having it slashed (destroying that money). For most purposes, both systems are considered secure.

---

#### Part 8 — Nodes: The Network That Runs It All

A **node** is any computer participating in the blockchain network. There are different types:

**Full nodes:** Store the entire blockchain history. Independently verify every transaction against the rules. Do not require anyone else's word. Bitcoin's blockchain is ~600GB; Ethereum's is larger.

**Light nodes:** Store only block headers (not full transaction data). Trust full nodes for transaction details. Used in mobile wallets — your MetaMask app doesn't download the full chain.

**Validator nodes (Ethereum PoS):** Full nodes that also participate in consensus — proposing and voting on blocks. Requires 32 ETH staked.

**Miner nodes (Bitcoin PoW):** Compete to solve the hash puzzle and propose blocks.

There is no headquarters. No server room. No company running Bitcoin. If every node in one country gets shut down, the network continues running on nodes everywhere else. This is **censorship resistance** — arguably the most important property of a public blockchain.

---

### 🔗 Interaction 1 — SHA-256 Hash Playground

**Link:** [xorbin.com/tools/sha256-hash-calculator](https://xorbin.com/tools/sha256-hash-calculator) *(or search "SHA-256 online tool")*

Work through these tasks as a class. Do them together, share results:

**Task A — The avalanche effect:**
1. Hash `"Blockchain"` → copy the output
2. Hash `"blockchain"` (lowercase b) → compare
3. Ask: *How many characters of the hash changed?* (Almost all of them.)
4. Hash `"blockchain "` (with a trailing space) → compare again

**Task B — Determinism:**
1. Hash `"BlockBase2025"`
2. Close the tab, reopen it, hash it again
3. Is the output identical? (Yes — always.)
4. Ask: *What does this tell you about hash functions?*

**Task C — One-way property:**
Show this hash on the screen: `a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3`
Ask: *Can anyone tell me what input produced this hash?*
Give them 2 minutes to try to guess. Nobody will get it.
Reveal: It's `"123"` — a three-character input produced 64 characters of output, and it's irreversible without brute force.

---

### 🔗 Interaction 2 — Anders Brownworth Blockchain Demo

**Link:** [andersbrownworth.com/blockchain](https://andersbrownworth.com/blockchain)

This demo is the most important thing you'll do today. Walk through all four tabs:

**Tab 1 — Hash:**
- Type your name → see the hash
- Change one character → hash completely changes
- **Task:** Each student types their full name. Everyone shares their hash. Can anyone reverse-engineer someone else's name from their hash?

**Tab 2 — Block:**
- The hash must start with `0000` to be "valid" (block shown in green)
- Type anything in the Data field → hash changes → block turns **red** (invalid)
- Click **Mine** → watch it try thousands of nonces per second until it finds one that produces a hash starting with `0000`
- **Task:** Type your name in the Data field. Click Mine. Count how many seconds it takes. Note the nonce it found. Now change one letter in your name and mine again — completely different nonce.
- **Key question:** *Why does changing the data require finding a completely new nonce?* (Because the hash changes, so the old nonce no longer produces a valid hash.)

**Tab 3 — Blockchain:**
- Multiple blocks chained — each block's "Prev" shows the previous block's hash
- Change data in Block 2 → Block 2 turns red. Block 3 also turns red (it stored Block 2's now-incorrect hash). Block 4 turns red too.
- **Task:** Change one word in Block 1. Observe the cascade. Now click Mine on Block 1 → Block 1 turns green, but Blocks 2, 3, 4 are still red. You have to re-mine **every subsequent block**. That's why rewriting history is so expensive — you'd have to redo all the mining work for every block after your change.

**Tab 4 — Distributed:**
- Same blockchain replicated across Peer A, Peer B, Peer C
- Tamper with Peer A's Block 2 → only Peer A's chain turns red
- The network can see that Peer A disagrees with B and C — Peer A is outvoted
- **The 51% rule in action:** To successfully rewrite history, you'd need to control more than half the network simultaneously and out-mine all honest nodes. With Bitcoin's current hash rate — billions of terahashes per second — this is economically and practically impossible.
- **Key insight to drive home:** *Nobody is in charge. The network itself enforces the rules by majority.*

---

### 🔗 Interaction 3 — Find the Bitcoin Genesis Block

**Link:** [blockchair.com/bitcoin/block/0](https://blockchair.com/bitcoin/block/0)

Students find and answer these questions (give them 5 minutes):

1. What date was the Genesis Block mined? *(January 3, 2009)*
2. How many transactions does it contain? *(1 — the "coinbase" transaction rewarding Satoshi)*
3. What was the mining reward? *(50 BTC)*
4. Find the hash of the Genesis Block — how many zeros does it start with? *(10 zeros — the difficulty target)*
5. What is the famous message Satoshi embedded in this block? *("The Times 03/Jan/2009 Chancellor on brink of second bailout for banks")*
6. Why does that message prove the block wasn't secretly pre-mined earlier? *(It references a specific newspaper headline from that exact day — impossible to include it before the newspaper was published)*

**Bonus:** Look at any recent Bitcoin block. How many transactions are in it? How much was the total fees paid by all those transactions? How different is the block reward today vs. 50 BTC in 2009? (Answer: it has halved every ~4 years — currently 3.125 BTC per block after the 2024 halving.)

---

### 💻 Lab — No coding today

Day 1 is conceptual. The goal is that every student can explain to a non-technical friend: what problem blockchain solves, how hashing keeps data immutable, what mining is and why it requires work, and why the distributed nature of nodes is what makes it trustworthy. If those four things are clear, Day 1 has succeeded.

---

### 🎯 Challenge — The 51% Attack Calculator

**Link:** [crypto51.app](https://crypto51.app)

This site shows the theoretical hourly cost of executing a 51% attack on various blockchains.

Find:
- How much would it cost per hour to 51% attack Bitcoin? *(Extremely high — usually shown as impractical)*
- Find a small, lesser-known blockchain. What does a 51% attack cost on that one? *(Often surprisingly cheap — some can be attacked for a few hundred dollars per hour)*
- Discussion: *Why does a blockchain's security depend so heavily on how many miners/validators are securing it?*

---

### Takeaway
> *"A blockchain is a shared ledger that thousands of computers keep in sync. Hashing makes the data tamper-evident. Mining (PoW) or staking (PoS) is how the network agrees on the next block without trusting anyone. No one is in charge — the rules are enforced by the math."*

---
---

## Day 2 — Ethereum and How It Changed Everything

**Theme:** Understanding why Ethereum was built, what makes it fundamentally different from Bitcoin, and all the concepts that underpin it — the EVM, accounts, gas, consensus, wallets, and dApps.

> **Instructor note:** After today, there is no more dedicated theory. Days 3–8 are all about building. So this day needs to be complete. A student finishing Day 2 should have zero unanswered "but how does it actually work?" questions about the Ethereum ecosystem.

---

### Theory

---

#### Part 1 — Quick Recap of Day 1 (10 min)

Ask the class — expect a student to answer each:

1. *"What problem did Bitcoin solve?"* → double-spend, trustless transactions without a middleman
2. *"What is a hash function? What are its three properties?"* → deterministic, avalanche effect, one-way
3. *"What is mining and why does it require real-world work?"* → finding a nonce that produces a hash with enough zeros; work = security
4. *"What is the difference between Proof of Work and Proof of Stake?"* → PoW = computational puzzle; PoS = stake collateral, random selection + slashing
5. *"Why can't you secretly edit a block?"* → its hash changes, breaking every block after it

If any of these aren't landing clearly, spend 5 minutes on it before moving on.

---

#### Part 2 — The Limits of Bitcoin

Bitcoin is brilliant. It solved the double-spend problem elegantly and created the first truly scarce digital asset. But it was designed with one purpose: moving BTC between addresses.

Bitcoin is a calculator that can only add and subtract. It's intentionally limited.

Some developers in the early Bitcoin community wanted to add more functionality — programmable transactions, decentralised applications, complex logic. The Bitcoin Core developers said no. Bitcoin's simplicity was a feature, not a bug. Complexity introduces attack surfaces.

This created a gap: the world had a trustless, immutable network — but it could only do one thing.

---

#### Part 3 — The Vitalik Story

**Vitalik Buterin** was born in 1994 in Russia, moved to Canada as a child. He was prodigiously good at mathematics from an early age — reportedly three times faster at mental arithmetic than his peers by age 4.

As a teenager, he played **World of Warcraft** obsessively. One day, game developer Blizzard patched the game and removed the damage component from his favourite warlock skill — *Siphon Life*. Vitalik was devastated. He went to bed crying.

He later wrote: *"I happily played World of Warcraft during 2007-2010, but one day Blizzard removed the damage component from my warlock's Siphon Life spell... I cried myself to sleep, and on that day I realised what horrors centralised services can bring."*

It sounds trivial. But it planted something: the instinct that centralised systems — where one company can unilaterally change something that matters to you — are a problem.

In 2011, at age 17, he discovered Bitcoin through his father. He became obsessed. To earn Bitcoin (he had no money to buy it), he wrote articles for a cryptocurrency blog at 5 BTC per article. In late 2011, he co-founded **Bitcoin Magazine** — the first serious print publication dedicated to cryptocurrency — and became its lead writer.

He dropped out of the University of Waterloo after winning a **Thiel Fellowship** (a $100,000 grant given to young people to pursue projects instead of college — started by Peter Thiel, co-founder of PayPal).

He travelled the world visiting every notable crypto project of the time. He talked to developers, read whitepapers, understood every approach being tried. And he noticed a pattern: every project was building one specific application on a blockchain — a decentralised domain name system, a decentralised storage system, a prediction market. Each was building its own blockchain from scratch, with its own consensus layer, just to do one thing.

The insight: *"All of these projects are reinventing the same base layer over and over. What if instead of building application-specific blockchains, we built a general-purpose blockchain that anyone could build any application on?"*

In late 2013, he wrote a whitepaper describing this idea. He called it **Ethereum**.

He took the whitepaper to Bitcoin developers, expecting enthusiasm. Instead, he was told: it's not necessary. Do it within Bitcoin's scripting language. (Bitcoin does have a limited scripting system, but it's intentionally not Turing-complete — it cannot run arbitrary programs.)

He decided to build it independently. The Ethereum team — including **Gavin Wood**, **Joseph Lubin**, **Anthony Di Iorio**, and **Charles Hoskinson** among others — formed around the idea.

**Gavin Wood** wrote the first technical specification of Ethereum (the Yellow Paper) and invented **Solidity**, the programming language you'll be writing in from Day 3 onwards.

**Key moments in Ethereum history:**
- **January 2014** — Ethereum announced publicly at the Bitcoin conference in Miami
- **July–August 2014** — Ethereum's crowdsale (ICO) raises 31,591 BTC (~$18 million at the time) — one of the first major token sales ever
- **July 30, 2015** — Ethereum mainnet launches (called "Frontier" — the first of several planned upgrade phases)
- **June 2016** — The DAO hack: ~$60 million in ETH stolen from a popular smart contract due to a vulnerability. The community controversially chose to hard-fork (revert the theft). This split Ethereum into **Ethereum (ETH)** and **Ethereum Classic (ETC)** — the unforked chain.
- **2017** — The ICO boom: hundreds of projects raise billions by selling ERC-20 tokens on Ethereum. Gas fees spike. The network strains.
- **2020** — **DeFi Summer**: Decentralised Finance explodes. Uniswap, Compound, Aave collectively handle billions in daily volume. All running on Ethereum smart contracts.
- **September 15, 2022** — **The Merge**: Ethereum switches from Proof of Work to Proof of Stake. Energy consumption drops 99.95% overnight. One of the most complex live software migrations ever executed on infrastructure holding hundreds of billions of dollars.
- **2024 onwards** — Layer 2 networks (Arbitrum, Optimism, Base, zkSync) scale Ethereum throughput massively. The ecosystem is now a multi-chain world built on Ethereum's security.

---

#### Part 4 — What is Ethereum? (Technically)

Ethereum is a **decentralised, programmable blockchain**. Unlike Bitcoin, which tracks only one thing (BTC balances), Ethereum tracks the state of an entire computer.

**Core components:**

**Ether (ETH)** — Ethereum's native cryptocurrency. Used to pay for computation (gas). Also used as collateral by validators in Proof of Stake. ETH is also a store of value and is used within DeFi protocols.

**The EVM (Ethereum Virtual Machine)** — The EVM is the runtime environment for Ethereum smart contracts. Think of it as a global computer that every node on the network runs simultaneously.

When you deploy a smart contract, your Solidity code is compiled to **EVM bytecode** — a low-level instruction set that the EVM can execute. Every full node runs the same bytecode on the same inputs and gets the same output. This is what makes the result trustworthy — it's not one company's server computing the answer; it's thousands of independent computers agreeing.

The EVM is **Turing-complete** — it can run any computation that a regular computer can. This is the fundamental difference from Bitcoin's scripting system.

**Accounts** — Ethereum has two types of accounts:

| Type | Controlled by | Can initiate transactions? | Has code? |
|---|---|---|---|
| **Externally Owned Account (EOA)** | A private key (a person's wallet) | Yes | No |
| **Contract Account** | Code | No (only responds) | Yes |

Your MetaMask wallet is an EOA. Smart contracts are Contract Accounts. When you call a smart contract, your EOA sends a transaction to the contract's address, which triggers the contract's code to execute.

**Transactions** — Every state change on Ethereum happens through a transaction. A transaction includes:
- **From:** the sender's address (EOA)
- **To:** the recipient's address (EOA or contract)
- **Value:** ETH to send (can be 0)
- **Data:** input data (for contract calls, this contains the function and arguments)
- **Gas limit:** maximum gas you're willing to spend
- **Nonce:** a counter preventing replay attacks (each EOA's transaction count)
- **Signature:** cryptographic proof that the sender owns the private key

---

#### Part 5 — Gas: The Economic Engine

Every operation the EVM executes costs **gas** — a unit of computational work. You pay gas fees in ETH.

**Why gas exists:**
1. **Compensate validators** — they spend resources running the EVM; fees are their income (in addition to staking rewards)
2. **Prevent spam** — every transaction has a cost, so you can't flood the network for free
3. **Prevent infinite loops** — your gas eventually runs out; an infinite loop just exhausts the gas and stops (you lose the gas fee, but the network isn't frozen)

**How gas is calculated:**

```
Total fee = Gas Used × Gas Price
```

- **Gas Used** — determined by the operations your transaction performs. A simple ETH transfer uses exactly 21,000 gas. Deploying a complex contract might use 2,000,000 gas.
- **Gas Price** — how much ETH you pay per unit of gas. Denominated in **gwei** (1 gwei = 0.000000001 ETH = 10⁻⁹ ETH).

**EIP-1559 (August 2021)** — Ethereum upgraded its fee mechanism:
- There is now a **base fee** — a minimum price set by the network, adjusted automatically based on how full the previous block was. The base fee is **burned** (destroyed — permanently removed from circulation).
- You can also pay a **priority fee** (tip) to incentivise validators to include your transaction faster.
- **Total fee = (base fee + priority fee) × gas used**

The burning of base fees is significant: when Ethereum is busy, more ETH is burned than is issued as staking rewards — making ETH **deflationary**. This is why people call ETH "ultrasound money."

**Gas price and congestion:**
- Low network activity → base fee is low → transactions are cheap
- High activity (NFT drop, popular DeFi protocol, market volatility) → base fee spikes → transactions get expensive
- In 2021 during peak NFT activity, gas fees sometimes exceeded $200 for a simple transfer. In quieter periods, they can be under $1.

**Gas limit:** When you send a transaction, you set a maximum gas limit. If your transaction uses less, you get the unused gas refunded. If it needs more than your limit, the transaction fails — but you still pay for the gas used up to the failure point. (This is why you should not set the gas limit too low.)

**gwei intuition:**
- Simple ETH transfer: ~21,000 gas at 20 gwei = 0.00042 ETH
- Contract interaction: ~100,000 gas at 20 gwei = 0.002 ETH
- Complex DeFi interaction: ~300,000 gas at 50 gwei = 0.015 ETH

---

#### Part 6 — Smart Contracts

A **smart contract** is a program stored on the blockchain that executes automatically when its conditions are met. It's code that lives at a specific Ethereum address.

The vending machine analogy:
- You put in money (ETH)
- The machine checks conditions: did I receive enough ETH?
- If yes: it dispenses the item (transfers a token, records your vote, releases a deed)
- No humans, no paperwork, no phone calls, no trust in a company required

Properties of smart contracts:
- **Trustless** — code executes as written; no one — not even the contract's creator — can stop it or alter it once deployed
- **Transparent** — the code is public; anyone can read exactly what it does before interacting
- **Permanent** — once deployed, the contract exists at its address forever on the blockchain
- **Deterministic** — same inputs always produce same outputs; every node agrees

Real example: **Uniswap** is a smart contract that lets anyone swap one token for another. There is no Uniswap company server processing your swap. You send a transaction to the Uniswap contract, the contract's code calculates the exchange rate and executes the swap, and the tokens arrive in your wallet. The entire exchange runs on code. It processes billions of dollars in volume using no employees, no servers (beyond the Ethereum network itself), and no login.

---

#### Part 7 — Wallets, Private Keys, and Addresses

This section is critical for student safety when they start using real wallets.

**A private key** is a 256-bit random number — essentially, an astronomically large secret. It looks like:
```
0x4c0883a69102937d6231471b5dbb6e538ebe...
```

From a private key, you can mathematically derive a **public key**, and from the public key, an **Ethereum address** (your `0x...` identifier). This is one-way: the address reveals nothing about the private key.

When you sign a transaction with your private key, anyone can verify the signature using your public address — proving you authorised the transaction — without ever learning your private key. This is **public-key cryptography** (specifically ECDSA — Elliptic Curve Digital Signature Algorithm).

**Seed phrase (mnemonic):** Rather than making you memorise a 64-character hex string, wallets generate a **seed phrase** — 12 or 24 ordinary English words. The seed phrase deterministically generates all your private keys. It's a human-readable backup.

**CRITICAL rules to establish with students:**
1. **Your seed phrase = your money.** Anyone who has your seed phrase has complete control of your wallet. No exceptions.
2. **Never type your seed phrase into any website, app, or form.** MetaMask will never ask for it except during wallet recovery in the official app.
3. **Never store your seed phrase digitally** — no photos, no cloud notes, no Google Docs.
4. **Write it on paper and store it somewhere safe.** Multiple copies in different locations for important amounts.
5. **Your address is public — safe to share.** Your private key and seed phrase are never shared with anyone, ever.
6. **If your seed phrase is compromised, transfer all funds immediately** to a new wallet and abandon the old one.

**MetaMask** is a browser extension and mobile app that manages your private keys for you. It stores them encrypted on your device. When a dApp wants you to sign a transaction, MetaMask shows you what you're signing and asks for your approval. It never sends your private key to any website.

---

#### Part 8 — dApps and the Web3 Stack

A **dApp** (decentralised application) is an app whose core logic runs on a smart contract rather than a private company server.

**Traditional app stack:**
```
User → Browser/App → Company Server → Company Database
```

**dApp stack:**
```
User → Browser/App → ethers.js/web3.js → MetaMask → Ethereum Smart Contract
```

The key difference: the backend is a smart contract on a public blockchain. Anyone can read the code. Anyone can verify the data. No company can shut it down, change the rules, or deny you access.

**Examples of real dApps:**
- **Uniswap** — swap any ERC-20 token for any other with no account, no KYC, no company
- **Aave** — lend and borrow crypto; interest rates set algorithmically by supply and demand
- **OpenSea** — NFT marketplace where the smart contract handles escrow and ownership transfers
- **ENS (Ethereum Name Service)** — buy a `.eth` domain (e.g. `vitalik.eth`) that maps to your Ethereum address
- **MakerDAO** — a decentralised central bank that issues DAI, a stablecoin pegged to USD via smart contract collateral

**ERC Standards:**
Because Ethereum is programmable, anyone can create tokens. But for tokens to work across all wallets, exchanges, and dApps, they need to follow agreed-upon interfaces:

- **ERC-20** — Fungible tokens (each unit is identical, like coins). Examples: USDC, DAI, LINK, UNI. Standard functions: `transfer()`, `balanceOf()`, `approve()`.
- **ERC-721** — Non-fungible tokens (each token is unique, has an ID). Examples: CryptoPunks, Bored Apes. Used for digital art, collectibles, in-game items.
- **ERC-1155** — Multi-token standard (both fungible and non-fungible in one contract). Common in gaming.

---

### 🔗 Interaction 1 — MetaMask Setup + Sepolia Testnet + Faucet

This is the most important practical setup of the course. Every student must complete this before Day 3.

**Step 1 — Install MetaMask**
- Go to [metamask.io](https://metamask.io) → Install for Chrome/Firefox/Brave
- Do NOT install from any other source — only metamask.io
- Create a new wallet
- Write down your seed phrase on paper — right now, in the session
- Create a strong password

**Step 2 — Understand your wallet**
Once MetaMask is open:
- Your **address** (shown at top, `0x...`) — this is your public identity. Safe to share.
- Your **balance** — currently 0 ETH on mainnet (real money — don't worry about this)

**Step 3 — Switch to Sepolia Testnet**
- Click the network selector at the top of MetaMask (shows "Ethereum Mainnet")
- Click "Show test networks" toggle if Sepolia doesn't appear
- Select **Sepolia test network**

**Step 4 — Get free Sepolia ETH**
Go to one of these faucets (try in order if one doesn't work):
- [sepoliafaucet.com](https://sepoliafaucet.com)
- [faucet.quicknode.com/ethereum/sepolia](https://faucet.quicknode.com/ethereum/sepolia)
- [cloud.google.com/application/web3/faucet/ethereum/sepolia](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)

Paste your wallet address → request ETH → wait ~30 seconds.

**Step 5 — Verify**
Your MetaMask should show 0.1–0.5 SepoliaETH. Paste your address into [sepolia.etherscan.io](https://sepolia.etherscan.io) — you should see the faucet transaction in your history.

**Key concepts to reinforce during setup:**
- Testnet ETH has no real-world value — it's purely for practice
- The Sepolia network is identical to mainnet Ethereum in terms of how it works — same EVM, same tools, same everything
- Your MetaMask address works on mainnet AND all testnets — it's the same key pair

---

### 🔗 Interaction 2 — Read a Real Transaction on Etherscan

**Link:** [sepolia.etherscan.io](https://sepolia.etherscan.io)

Click on any recent transaction hash on the homepage. Walk through each field as a class:

| Field | What it means |
|---|---|
| Transaction Hash | Unique ID of this transaction — its SHA-256-based identifier |
| Status | Success or Failed |
| Block | Which block this transaction was included in |
| From | The EOA that sent this |
| To | The EOA or contract that received this |
| Value | ETH transferred |
| Transaction Fee | Gas Used × Gas Price (in ETH) |
| Gas Limit | Maximum gas the sender was willing to use |
| Gas Used | Actual gas consumed |
| Gas Price | Price per gas unit (in gwei) |

**Tasks for students:**

1. Find a transaction that sent ETH between two addresses. What was the gas fee? Express it in ETH and in gwei.

2. Find a transaction that interacted with a contract (the "To" field will show a contract address, not a wallet). Click on the contract address — explore it.

3. Click the **Contract** tab on any verified contract — find the Solidity code. Can you spot any `function` keywords even without knowing Solidity yet?

4. Click the **Read Contract** tab — call `balanceOf` or similar functions if available. Notice that reading data from a contract costs you nothing — no MetaMask popup, no gas fee.

5. Find your own wallet on Etherscan by pasting your address. You should see the faucet transaction.

---

### 🔗 Interaction 3 — Find Ethereum's Genesis Block

**Link:** [etherscan.io/block/0](https://etherscan.io/block/0)

Find and answer:

1. What date was Ethereum's Genesis Block created? *(July 30, 2015)*
2. How many transactions does block 0 contain? *(8893 — pre-allocated ETH balances from the crowdsale)*
3. Compare to Bitcoin's Genesis Block: Bitcoin had 1 transaction (mining reward to Satoshi). Ethereum had 8,893 (all the ICO participants getting their ETH). What does that tell you about how each network launched?
4. Navigate to block 1 — the first block after genesis. When was it? Who mined it? How much was the block reward?

**Bonus:** Vitalik's Ethereum address is publicly known: [etherscan.io/address/0xab5801a7d398351b8be11c439e05c5b3259aec9b](https://etherscan.io/address/0xab5801a7d398351b8be11c439e05c5b3259aec9b). Look at it. How much ETH does he hold? Have a look at some of his earliest transactions.

---

### 💻 Lab — No coding today

Spend any remaining time exploring Etherscan freely. Look up the Uniswap contract, find a recent large ETH transfer, look at a verified contract's code even if you can't understand it yet. Build intuition for what "on-chain" actually looks like.

---

### 🎯 Challenge — The Merge

On September 15, 2022, Ethereum switched from PoW to PoS in a live network upgrade. Find block number **15,537,393** on Etherscan — the last Proof of Work block. Then look at **15,537,394** — the first Proof of Stake block.

Find:
- What changed between the two blocks? (Look at the "Mined by" vs "Proposed by" field)
- What was the energy consumption reduction of The Merge? (~99.95%)
- Why couldn't Ethereum just restart from scratch — why did it have to migrate a live network?

---

### Takeaway
> *"Ethereum is a programmable blockchain — a world computer. Smart contracts are programs on that computer that run automatically, exactly as written, with no company or middleman. Gas is the cost of computation. Your wallet is a cryptographic identity. Everything else in this course — tokens, dApps, deployment — is built on top of this foundation."*

---
---

## Day 3 — Solidity Basics

**Theme:** Writing your first smart contracts — variables, functions, and how state works.

---

### Theory

#### Quick Recap (5 min)

Ask: *"What is a smart contract?"*
Today you write one.

#### Setting Up Remix IDE

**Link:** [remix.ethereum.org](https://remix.ethereum.org)

*(Do NOT use Safari — MetaMask extension doesn't work on Safari)*

Tour of the interface:
- **Left sidebar** — File Explorer, Compiler, Deployer, Plugins
- **Centre** — Code editor
- **Bottom panel** — Console output (compilation errors, transaction logs)

Open `contracts/1_Storage.sol` — read through it together before touching anything.

#### Solidity — What Kind of Language Is It?

Solidity is a **statically-typed, object-oriented** language designed specifically for the EVM. It looks similar to JavaScript/C++ but has some unique concepts.

Every Solidity file starts with two things:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;
```

- The **license identifier** is required (use MIT for open source)
- The **pragma** tells the compiler which version to use. `^0.8.10` means "version 0.8.10 or higher (but not 0.9.x)"

#### Contracts

A `contract` in Solidity is like a `class` in other languages. It's the fundamental unit.

```solidity
contract HelloWorld {
    // everything goes in here
}
```

#### Variables — Three Types

```solidity
contract Variables {
    // STATE variables — stored permanently on the blockchain
    uint public myNumber = 42;
    string public myName = "BlockBase";
    bool public isActive = true;

    function doSomething() public {
        // LOCAL variable — exists only during function execution
        uint localVar = 10;

        // GLOBAL variables — injected by the EVM (no declaration needed)
        address sender = msg.sender;    // who called this function
        uint time = block.timestamp;    // current block's Unix timestamp
    }
}
```

**Key data types:**
- `uint` — unsigned integer (non-negative whole number). Alias for `uint256` (0 to 2²⁵⁶−1)
- `uint8`, `uint16`, `uint256` — different size integers
- `int` — signed integer (can be negative)
- `bool` — true or false
- `address` — an Ethereum wallet/contract address (`0x...`)
- `string` — text

**Visibility modifiers** (who can see/call this?):
- `public` — visible to everyone inside and outside the contract
- `private` — only this contract
- `internal` — this contract + contracts that inherit from it
- `external` — only callable from outside (not internally)

#### Functions

```solidity
contract Counter {
    uint public count = 0;

    // Changes state — costs gas when called
    function increment() public {
        count = count + 1;
    }

    // view: reads state, doesn't change it — FREE (no gas)
    function getCount() public view returns (uint) {
        return count;
    }

    // pure: doesn't read OR write state — also FREE
    function add(uint a, uint b) public pure returns (uint) {
        return a + b;
    }
}
```

**Rule of thumb:**
- `view` = "I promise I'm only looking, not touching"
- `pure` = "I'm not even looking at the contract's storage"
- Neither `view` nor `pure` = you're changing something → costs gas

#### Control Flow

```solidity
function describeNumber(uint x) public pure returns (string memory) {
    if (x < 10) {
        return "small";
    } else if (x < 100) {
        return "medium";
    } else {
        return "large";
    }
}

function sumToN(uint n) public pure returns (uint) {
    uint total = 0;
    for (uint i = 1; i <= n; i++) {
        total = total + i;
    }
    return total;
}
```

Note: `string memory` — strings need a memory location keyword. For now, always write `string memory` when returning strings from functions.

---

### 💻 Lab — Build a Storage Contract

**Goal:** Write, deploy, and interact with your first smart contract.

**Step 1 — Create a new file**
In Remix → File Explorer → New File → name it `MyStorage.sol`

**Step 2 — Write the contract**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract MyStorage {
    uint public storedNumber;
    string public storedMessage;
    address public lastCaller;

    function setNumber(uint _number) public {
        storedNumber = _number;
        lastCaller = msg.sender;
    }

    function setMessage(string memory _message) public {
        storedMessage = _message;
        lastCaller = msg.sender;
    }

    function getNumber() public view returns (uint) {
        return storedNumber;
    }
}
```

**Step 3 — Compile**
- Click the **Solidity Compiler** tab (left sidebar)
- Select compiler version `0.8.10` (or higher)
- Click **Compile MyStorage.sol**
- Green checkmark = success. Red = there's an error in your code.

**Step 4 — Deploy**
- Click the **Deploy & Run** tab
- Environment: **Remix VM (Shanghai)** — this is a local simulator, no real ETH needed
- Contract: select `MyStorage`
- Click **Deploy**
- You'll see it appear under "Deployed Contracts" at the bottom

**Step 5 — Interact**
- Expand the deployed contract
- Call `setNumber` with the value `42` → click the orange button
- Call `getNumber` → should return `42`
- Call `setMessage` with `"Hello from BlockBase"`
- Call `storedMessage` → see your message returned
- Check `lastCaller` → should show your Remix VM address

**What just happened?** You deployed a program to a simulated blockchain. It stores data permanently and anyone with the address can call it.

---

### 🎯 Challenge — Build a Counter

Extend your skills by writing this from scratch without copying:

```
Contract name: Counter
State variables: count (uint), owner (address)
Constructor: sets owner to msg.sender
Functions:
  - increment(): adds 1 to count
  - decrement(): subtracts 1 from count (hint: what happens if count is 0?)
  - reset(): sets count back to 0
  - getCount(): returns current count (view function)
```

Hints:
- A `constructor()` runs once when the contract is deployed
- To guard against underflow on decrement, use: `require(count > 0, "Already zero");`

---

### Takeaway
> *"A smart contract is just a program. State variables live on the blockchain permanently. Functions either read state for free or write state for gas."*

---
---

## Day 4 — Data Structures and Modifiers

**Theme:** Arrays, structs, mappings — the data patterns used in almost every real contract.

---

### Theory

#### Quick Recap (5 min)

Ask a student to explain `view` vs regular functions. Ask another: what is `msg.sender`?

#### Memory vs Storage vs Calldata

These are **data locations** — where in the EVM data physically lives.

| Location | Where | Lifetime | Cost |
|---|---|---|---|
| `storage` | Blockchain | Permanent | Expensive |
| `memory` | RAM (temporary) | During function execution | Cheap |
| `calldata` | Input data | Read-only, during function | Cheapest |

**Rule:** State variables are always `storage`. Function parameters and local arrays/strings need you to specify `memory` or `calldata`.

```solidity
// ✅ Correct
function greet(string memory _name) public pure returns (string memory) {
    return _name;
}

// ❌ Will not compile — string needs a location
function greet(string _name) public pure returns (string) { ... }
```

#### Arrays

```solidity
contract ArrayExample {
    // Dynamic array — grows and shrinks
    uint[] public numbers;

    // Fixed-size array — always exactly 5 elements
    uint[5] public fixedNumbers;

    function addNumber(uint _n) public {
        numbers.push(_n);          // add to end
    }

    function removeLastNumber() public {
        numbers.pop();             // remove from end
    }

    function getLength() public view returns (uint) {
        return numbers.length;
    }

    function getAt(uint _index) public view returns (uint) {
        return numbers[_index];
    }
}
```

#### Structs

Structs let you group related data into a custom type.

```solidity
contract StudentRegistry {
    struct Student {
        string name;
        uint age;
        bool isEnrolled;
    }

    Student[] public students;

    function addStudent(string memory _name, uint _age) public {
        students.push(Student({
            name: _name,
            age: _age,
            isEnrolled: true
        }));
    }

    function getStudent(uint _index) public view returns (string memory, uint, bool) {
        Student memory s = students[_index];
        return (s.name, s.age, s.isEnrolled);
    }
}
```

#### Mappings

A **mapping** is a key-value store — like a dictionary or hash map. This is the most commonly used data structure in Solidity.

```solidity
contract Balances {
    // mapping(keyType => valueType)
    mapping(address => uint) public balances;

    function deposit(uint _amount) public {
        balances[msg.sender] = balances[msg.sender] + _amount;
    }

    function getBalance(address _user) public view returns (uint) {
        return balances[_user];
    }
}
```

Important: mappings cannot be iterated. You can't loop over all keys. If you need to loop over users, you need to maintain a separate array of keys alongside the mapping.

#### Enums

```solidity
contract ShippingStatus {
    enum Status { Pending, Shipped, Delivered, Cancelled }

    Status public currentStatus;

    function ship() public {
        currentStatus = Status.Shipped;
    }

    function isDelivered() public view returns (bool) {
        return currentStatus == Status.Delivered;
    }
}
```

#### Events

Events are logs stored on the blockchain. They're cheaper than storing data in state variables and are used to notify frontends about things that happened.

```solidity
contract EventExample {
    // Declare the event
    event Transfer(address indexed from, address indexed to, uint amount);

    function transfer(address _to, uint _amount) public {
        // ... transfer logic ...
        // Emit the event
        emit Transfer(msg.sender, _to, _amount);
    }
}
```

`indexed` means you can filter events by that field on Etherscan or in your frontend.

#### Modifiers — the `onlyOwner` Pattern

Modifiers are reusable conditions that wrap functions. The most common pattern in Solidity:

```solidity
contract Owned {
    address public owner;

    constructor() {
        owner = msg.sender;   // whoever deploys the contract becomes the owner
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;    // this means "now run the actual function"
    }

    function sensitiveAction() public onlyOwner {
        // only the owner can call this
    }
}
```

`require(condition, "error message")` — if the condition is false, the transaction reverts (nothing happens, gas is partially refunded) and the error message is returned.

---

### 💻 Lab — Build a To-Do List Contract

This brings together everything from Days 3 and 4.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

contract TodoList {
    address public owner;

    struct Task {
        string text;
        bool completed;
        uint createdAt;
    }

    Task[] public tasks;

    event TaskAdded(uint indexed taskId, string text);
    event TaskCompleted(uint indexed taskId);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can do this");
        _;
    }

    function addTask(string memory _text) public {
        tasks.push(Task({
            text: _text,
            completed: false,
            createdAt: block.timestamp
        }));
        emit TaskAdded(tasks.length - 1, _text);
    }

    function completeTask(uint _index) public {
        require(_index < tasks.length, "Task does not exist");
        require(!tasks[_index].completed, "Already completed");
        tasks[_index].completed = true;
        emit TaskCompleted(_index);
    }

    function deleteTask(uint _index) public onlyOwner {
        require(_index < tasks.length, "Task does not exist");
        tasks[_index] = tasks[tasks.length - 1];
        tasks.pop();
    }

    function getTaskCount() public view returns (uint) {
        return tasks.length;
    }
}
```

**Deploy and test in Remix:**
1. Add 3 tasks: `"Learn Solidity"`, `"Deploy a contract"`, `"Build a dApp"`
2. Call `getTaskCount()` → should return 3
3. Complete task at index 0
4. Try completing it again → should fail with "Already completed"
5. Delete task at index 1 (only works because you're the deployer = owner)
6. Switch to a different account in Remix and try to delete → should fail with "Only owner can do this"

---

### 🎯 Challenge — Voting Contract

Build a simple voting system:

```
Contract: SimpleVote
- Candidates are stored as strings in an array
- A mapping tracks how many votes each candidate index has received
- A mapping tracks whether an address has voted (to prevent double voting)
- addCandidate(string): adds a candidate (onlyOwner)
- vote(uint candidateIndex): casts a vote (each address votes once)
- getVotes(uint candidateIndex): returns vote count
- getWinner(): returns the name of the candidate with most votes
```

---

### Takeaway
> *"Mappings connect addresses to data. Structs group related data. Modifiers guard functions. These three patterns are the backbone of almost every real smart contract."*

---
---

## Day 5 — ERC20 Tokens

**Theme:** Understanding the token standard and building your own coin.

---

### Theory

#### Quick Recap (5 min)

Ask: *"What does a mapping do? Give me an example."*
Expected: `mapping(address => uint) balances` — maps each address to a balance.

Now: *"What would you need to build a token?"* Let students think...
- A way to track how many tokens each address has → `mapping(address => uint)`
- A way to transfer tokens → a function that decrements one balance and increments another
- A way to check someone's balance → a `view` function on the mapping

That's basically it. ERC-20 is just a standardised version of exactly this.

#### What is a Token?

A **token** is a smart contract that tracks balances. Unlike ETH (which is built into the Ethereum protocol), tokens are just code.

When you "hold" a token, it means: the token's smart contract has a record that your address has X units.

When you "send" a token, the token's smart contract deducts from your balance and adds to the recipient's.

#### The ERC-20 Standard

ERC stands for **Ethereum Request for Comment** — it's the process for proposing new standards. ERC-20 was proposed in 2015 and defines the minimum set of functions every token must implement so that wallets, exchanges, and dApps can support any token without custom code.

Required functions in ERC-20:
```
totalSupply()        — returns total number of tokens in existence
balanceOf(address)   — returns how many tokens an address holds
transfer(to, amount) — sends tokens from caller to another address
approve(spender, amount)      — lets another address spend tokens on your behalf
allowance(owner, spender)     — check approved spending amount
transferFrom(from, to, amount) — spend approved tokens
```

Required events:
```
Transfer(from, to, amount)
Approval(owner, spender, amount)
```

You don't need to write all of this from scratch. **OpenZeppelin** has already written a battle-tested implementation.

#### OpenZeppelin

**OpenZeppelin** is a library of secure, community-audited smart contract building blocks. Think of it like npm for smart contracts — you import their code and extend it.

Why use OpenZeppelin instead of writing your own?
- Written by experts
- Audited by security researchers
- Used by millions of dollars worth of production contracts
- You avoid subtle bugs that could drain your contract

In Remix, you import directly from their GitHub:

```solidity
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

#### Building an ERC-20 Token

Using OpenZeppelin, creating a token takes about 10 lines:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {
    address public owner;

    constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {
        owner = msg.sender;
        _mint(msg.sender, 1000000 * 10 ** decimals());
        // Mints 1,000,000 tokens to the deployer
        // 10 ** decimals() = 10^18 — tokens use 18 decimal places by default
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function mint(address _to, uint _amount) public onlyOwner {
        _mint(_to, _amount);
    }
}
```

`is ERC20` means our contract **inherits** from OpenZeppelin's ERC20. We get all its functions for free. We just customise the name, symbol, and initial supply.

#### Decimals Explained

ERC-20 tokens use 18 decimal places by default (same as ETH's wei/ether relationship). So `1 token` is stored as `1 * 10^18` in the contract.

This is why you see:
```solidity
_mint(msg.sender, 1000000 * 10 ** 18);
```
or equivalently using the `decimals()` helper:
```solidity
_mint(msg.sender, 1000000 * 10 ** decimals());
```

---

### 💻 Lab — Deploy Your Own Token

**Step 1 — Create `MyToken.sol` in Remix**

Copy the contract from the Theory section. Choose your own token name and symbol (e.g. `"BlockBase Token"`, `"BBT"`).

**Step 2 — Compile**
Make sure compiler version is 0.8.10+. Compile.

**Step 3 — Deploy to Remix VM**
- Deploy with constructor args: `"BlockBase Token"` and `"BBT"`
- After deployment, expand the contract

**Step 4 — Interact**

Call `totalSupply()` → should return `1000000000000000000000000` (1 million with 18 decimals)

Call `balanceOf(your-address)` → same number (you got all the initial supply)

Call `transfer(another-address, 1000000000000000000)` to send 1 token
→ Use one of the other fake addresses in the Remix VM dropdown

Call `balanceOf(other-address)` → should now show `1000000000000000000`

**Step 5 — Add a second Remix account as a minter**
Switch accounts in Remix. Call `mint()` — it should fail with "Not owner."
Switch back to original account. Call `mint(other-address, 5000000000000000000)` (5 tokens).
Check balance of other address — should increase.

---

### 🔗 Interaction — Import Your Token into MetaMask

On Remix VM this won't work (it's a local simulator). But after deploying to Sepolia on Day 6, students will:

1. Copy the deployed contract address
2. Open MetaMask → Assets → Import Token
3. Paste the contract address
4. MetaMask auto-fills name and symbol
5. Your token appears in your wallet with its balance

This is worth previewing today so students know what they're building toward.

---

### 🎯 Challenge — Add a Burn Function

Extend your ERC-20 with:
- A `burn(uint amount)` function that lets any holder destroy their own tokens (reducing total supply)
- OpenZeppelin has a `_burn(address, amount)` internal function — use it
- A `cap` on total supply — minting should fail if total supply would exceed 2,000,000 tokens

---

### Takeaway
> *"A token is just a smart contract with a mapping from addresses to balances. ERC-20 is the agreed interface so wallets and dApps can work with any token. OpenZeppelin gives you a safe, ready-made base to extend."*

---
---

## Day 6 — Deploying to a Real Network

**Theme:** Getting your contract live on a real (test) blockchain and verifying it.

---

### Theory

#### What is a Testnet?

A **testnet** is a full copy of the Ethereum network — same rules, same EVM, same everything — but the ETH on it has no real-world value. It exists purely for testing before you deploy to mainnet (where ETH costs real money).

Major Ethereum testnets:
- **Sepolia** — the current recommended testnet for development. Fast, stable, well-supported.
- **Goerli** — older testnet, being deprecated. Avoid.
- **Mainnet** — real Ethereum. Real ETH. Mistakes cost real money.

For this course we use **Sepolia** exclusively.

#### The Deployment Process (How It Works)

When you deploy a contract:
1. You write a special "deployment transaction" containing your compiled contract's bytecode
2. You sign it with your private key (MetaMask handles this)
3. It's broadcast to the network
4. Miners/validators include it in a block
5. The EVM processes it, assigns the contract an address, and stores the code
6. The contract now lives on-chain at that address — forever

After deployment you receive a **contract address** — a permanent address on the blockchain. Anyone with this address can interact with your contract.

#### Etherscan Verification

After deploying, your contract exists on-chain but its source code is private (only bytecode is visible). **Verifying** the contract means submitting your Solidity source code to Etherscan, which then confirms it matches the deployed bytecode and publishes the readable code.

Why verify? So others can read, audit, and trust your contract. Unverified contracts look like this:

```
Contract source code not verified
```

Verified contracts show the full readable Solidity.

#### Hardhat — An Awareness Introduction

**Hardhat** is a professional Ethereum development environment. Real developers use it instead of Remix for serious projects.

You don't need Hardhat for this course — Remix is fine. But you'll see it everywhere in tutorials and job descriptions, so here's what to know:

**What Hardhat gives you:**
- A local blockchain for testing (faster than Remix VM)
- Test files written in JavaScript (with Mocha/Chai)
- Scripts to automate deployments
- A plugin ecosystem

**The 3 commands you'll encounter:**

```bash
# 1. Create a new Hardhat project
npx hardhat init

# 2. Compile all contracts in /contracts folder
npx hardhat compile

# 3. Run all test files in /test folder
npx hardhat test
```

**What a test looks like:**

```javascript
const { expect } = require("chai");

describe("MyToken", function () {
  it("Should mint 1 million tokens to the deployer", async function () {
    const [owner] = await ethers.getSigners();
    const MyToken = await ethers.getContractFactory("MyToken");
    const token = await MyToken.deploy("BlockBase Token", "BBT");

    const balance = await token.balanceOf(owner.address);
    expect(balance).to.equal(ethers.parseUnits("1000000", 18));
  });
});
```

This is a taster — the concept is: you deploy your contract in a test, call its functions, and assert the results are what you expect. You won't write Hardhat tests in this course but you'll know what they are.

---

### 💻 Lab — Deploy Your ERC-20 to Sepolia

**Pre-requisite:** MetaMask installed with Sepolia ETH (from Day 2 faucet task)

**Step 1 — Open your `MyToken.sol` from Day 5 in Remix**

Make sure it compiles cleanly.

**Step 2 — Change the deployment environment**
- Click the **Deploy & Run** tab
- Environment dropdown → select **"Injected Provider - MetaMask"**
- MetaMask will pop up asking to connect → Click **Connect**
- Make sure MetaMask is on the **Sepolia** network

**Step 3 — Deploy**
- Fill in constructor args: your token name and symbol
- Click **Deploy**
- MetaMask pops up showing the transaction → review the gas fee → Click **Confirm**
- Wait ~15–30 seconds for the transaction to be mined
- The contract address appears in Remix under "Deployed Contracts"

**Step 4 — Find your contract on Etherscan**
- Copy the contract address
- Go to [sepolia.etherscan.io](https://sepolia.etherscan.io) and paste it in the search
- You should see: your contract, the deployment transaction, your wallet as the creator

**Step 5 — Verify on Etherscan**
- On your contract's Etherscan page → click the **Contract** tab → **Verify and Publish**
- Compiler type: **Solidity (Single file)**
- Compiler version: match what you used in Remix (e.g. `0.8.10`)
- License: MIT
- Paste your full Solidity source code
- Click **Verify and Publish**

After a minute, the Contract tab will show your full readable source code. ✅

**Step 6 — Import token to MetaMask**
- MetaMask → Assets → Import Token
- Paste your contract address
- Your token appears in MetaMask with your balance

---

### 🔗 Interaction — Share Your Contract

- Drop your Etherscan link in the group chat
- Open a classmate's contract link
- Click **Read Contract** → call `balanceOf` with your own address → you have 0 (you don't hold any of their token)
- Ask them to `transfer` some tokens to your address → check your balance again

---

### 🎯 Challenge — Deploy the Voting Contract from Day 4

Take your voting contract from the Day 4 challenge and:
1. Deploy it to Sepolia
2. Verify it on Etherscan
3. Use Etherscan's **Write Contract** tab to add candidates and cast votes
4. Use **Read Contract** to check the vote tallies

---

### Takeaway
> *"Testnet deployment is exactly like mainnet deployment — same process, same tools, no real money at risk. A verified contract is a trustworthy contract — anyone can read exactly what it does."*

---
---

## Day 7 — Connecting to a Frontend

**Theme:** Building a simple web interface that talks to your deployed smart contract.

---

### Theory

#### How a dApp Actually Works

```
User's Browser
     │
     ▼
HTML/CSS/JavaScript  ← the frontend you build
     │
     ▼ (via ethers.js)
MetaMask (wallet)   ← signs transactions, holds private key
     │
     ▼ (signed transaction)
Ethereum Network    ← your smart contract lives here
```

The key library is **ethers.js** — it handles all the complexity of talking to the Ethereum network from JavaScript. It lets you:
- Connect to a wallet (MetaMask)
- Read data from a contract (call `view` functions)
- Write data to a contract (send transactions)
- Listen for events

#### What is an ABI?

**ABI (Application Binary Interface)** — the bridge between your JavaScript and your Solidity contract. It's a JSON file that describes all your contract's functions and their input/output types.

When you compile in Remix, the ABI is generated automatically. You copy it into your frontend so ethers.js knows how to call your contract's functions.

You can find your ABI in Remix: Compiler tab → after compiling → click "ABI" button at the bottom → copy.

#### ethers.js via CDN — No npm Needed

For Day 7 we use ethers.js directly from a CDN (no Node.js, no npm, no build tools). Just plain HTML.

```html
<script src="https://cdn.ethers.io/lib/ethers-5.7.2.umd.min.js"></script>
```

That one line gives you the full ethers.js library.

#### Key ethers.js Patterns

**Connect wallet:**
```javascript
const provider = new ethers.providers.Web3Provider(window.ethereum);
await provider.send("eth_requestAccounts", []);
const signer = provider.getSigner();
const address = await signer.getAddress();
```

**Connect to a contract:**
```javascript
const contractAddress = "0x..."; // your deployed contract address
const abi = [...];               // your contract's ABI

// Read-only connection (no wallet needed)
const readContract = new ethers.Contract(contractAddress, abi, provider);

// Write connection (wallet required, signs transactions)
const writeContract = new ethers.Contract(contractAddress, abi, signer);
```

**Call a view function (free, no transaction):**
```javascript
const balance = await readContract.balanceOf(userAddress);
console.log(ethers.utils.formatUnits(balance, 18)); // convert from wei to tokens
```

**Send a transaction (costs gas, MetaMask popup):**
```javascript
const tx = await writeContract.transfer(recipientAddress, ethers.utils.parseUnits("10", 18));
await tx.wait(); // wait for it to be mined
console.log("Transfer complete!");
```

**Listen for events:**
```javascript
readContract.on("Transfer", (from, to, amount) => {
    console.log(`${from} sent ${ethers.utils.formatUnits(amount, 18)} tokens to ${to}`);
});
```

---

### 💻 Lab — Build a Token Dashboard

Build a complete HTML dApp for the ERC-20 token you deployed on Day 6.

Create a file `index.html` on your computer (or use Remix's built-in file system) with this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Token Dashboard</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 40px auto; padding: 0 20px; }
        button { padding: 10px 20px; margin: 8px 0; cursor: pointer; background: #6366f1; color: white; border: none; border-radius: 6px; }
        input { padding: 8px; margin: 4px 0; width: 100%; box-sizing: border-box; border: 1px solid #ccc; border-radius: 4px; }
        .card { border: 1px solid #e5e7eb; border-radius: 8px; padding: 20px; margin: 16px 0; }
        #status { color: #6b7280; font-size: 14px; margin-top: 8px; }
    </style>
</head>
<body>
    <h1>🪙 My Token Dashboard</h1>

    <div class="card">
        <h2>Wallet</h2>
        <button onclick="connectWallet()">Connect MetaMask</button>
        <p id="walletAddress">Not connected</p>
        <p>Your balance: <strong id="userBalance">—</strong></p>
    </div>

    <div class="card">
        <h2>Token Info</h2>
        <p>Name: <strong id="tokenName">—</strong></p>
        <p>Symbol: <strong id="tokenSymbol">—</strong></p>
        <p>Total Supply: <strong id="totalSupply">—</strong></p>
    </div>

    <div class="card">
        <h2>Send Tokens</h2>
        <input type="text" id="recipientAddress" placeholder="Recipient address (0x...)">
        <input type="number" id="sendAmount" placeholder="Amount to send">
        <button onclick="sendTokens()">Send</button>
        <p id="status"></p>
    </div>

    <script src="https://cdn.ethers.io/lib/ethers-5.7.2.umd.min.js"></script>
    <script>
        // ⚠️ REPLACE THESE WITH YOUR OWN VALUES
        const CONTRACT_ADDRESS = "0xYOUR_CONTRACT_ADDRESS_HERE";
        const ABI = [
            "function name() view returns (string)",
            "function symbol() view returns (string)",
            "function totalSupply() view returns (uint256)",
            "function balanceOf(address) view returns (uint256)",
            "function transfer(address to, uint256 amount) returns (bool)",
            "event Transfer(address indexed from, address indexed to, uint256 value)"
        ];
        // Note: you can use this simplified "human-readable ABI" format with ethers.js
        // instead of the full JSON ABI from Remix — much easier to read!

        let provider, signer, contract;

        async function connectWallet() {
            provider = new ethers.providers.Web3Provider(window.ethereum);
            await provider.send("eth_requestAccounts", []);
            signer = provider.getSigner();
            const address = await signer.getAddress();
            document.getElementById("walletAddress").textContent = address;

            contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, signer);

            // Load token info
            document.getElementById("tokenName").textContent = await contract.name();
            document.getElementById("tokenSymbol").textContent = await contract.symbol();

            const supply = await contract.totalSupply();
            document.getElementById("totalSupply").textContent =
                ethers.utils.formatUnits(supply, 18) + " tokens";

            // Load user balance
            await refreshBalance(address);

            // Listen for Transfer events
            contract.on("Transfer", async (from, to, amount) => {
                const myAddr = await signer.getAddress();
                if (to.toLowerCase() === myAddr.toLowerCase()) {
                    await refreshBalance(myAddr);
                    setStatus("✅ Received " + ethers.utils.formatUnits(amount, 18) + " tokens!");
                }
            });
        }

        async function refreshBalance(address) {
            const bal = await contract.balanceOf(address);
            document.getElementById("userBalance").textContent =
                ethers.utils.formatUnits(bal, 18) + " " + await contract.symbol();
        }

        async function sendTokens() {
            const recipient = document.getElementById("recipientAddress").value;
            const amount = document.getElementById("sendAmount").value;

            if (!recipient || !amount) {
                setStatus("Please fill in all fields.");
                return;
            }

            try {
                setStatus("⏳ Sending transaction...");
                const tx = await contract.transfer(
                    recipient,
                    ethers.utils.parseUnits(amount, 18)
                );
                setStatus("⏳ Waiting for confirmation...");
                await tx.wait();
                setStatus("✅ Sent! Tx: " + tx.hash);
                const addr = await signer.getAddress();
                await refreshBalance(addr);
            } catch (err) {
                setStatus("❌ Error: " + err.message);
            }
        }

        function setStatus(msg) {
            document.getElementById("status").textContent = msg;
        }
    </script>
</body>
</html>
```

**Step-by-step:**
1. Replace `CONTRACT_ADDRESS` with your deployed Sepolia ERC-20 address from Day 6
2. Open `index.html` in your browser (just double-click the file)
3. Click **Connect MetaMask** → approve in MetaMask
4. Token name, symbol, total supply should load
5. Try sending tokens to a classmate's address
6. Check MetaMask for the transaction confirmation popup

---

### 🎯 Challenge — Add a Balance Checker

Add a new section to your dApp:

```html
<div class="card">
    <h2>Check Any Balance</h2>
    <input type="text" id="checkAddress" placeholder="Any Ethereum address">
    <button onclick="checkBalance()">Check Balance</button>
    <p id="checkResult">—</p>
</div>
```

Write the `checkBalance()` function that reads the balance of whatever address is in the input field and displays it. This is a **read-only** call — it doesn't need MetaMask to sign anything, just a provider.

---

### Takeaway
> *"ethers.js is the bridge between your webpage and the blockchain. A provider reads data. A signer sends transactions. An ABI tells ethers.js how to talk to your specific contract."*

---
---

## Day 8 — Capstone Project

**Theme:** Build, deploy, and demo a complete dApp from scratch.

---

### Structure of the Day

| Time | Activity |
|---|---|
| 0:00 – 0:30 | Project briefing + team formation + pick your project |
| 0:30 – 2:30 | Build the smart contract + deploy to Sepolia |
| 2:30 – 3:30 | Build the frontend + wire up ethers.js |
| 3:30 – 4:00 | Demo presentations (2–3 min per person/team) |
| 4:00 – 4:30 | Peer feedback + what to learn next |

---

### Project Options

Students pick one of three options. All three use skills from Days 3–7.

---

#### Option A — Token Voting Dashboard

**What you build:**
A dApp where users vote for candidates by spending tokens. The candidate with the most tokens voted for wins.

**Smart contract features:**
- Owner adds candidates (name + description)
- Any token holder can vote by sending tokens to the contract for a candidate
- Votes are weighted by token amount
- View current standings
- Winner announced when owner closes voting

**Frontend features:**
- List of candidates with current vote counts
- Input to vote (how many tokens + which candidate)
- Live updating standings via Transfer events
- Connect MetaMask button

**Hint for the contract:**
```solidity
mapping(uint => uint) public votesFor;  // candidateIndex => total tokens voted
mapping(address => bool) public hasVoted;
```

---

#### Option B — Simple NFT Minter

**What you build:**
A dApp where users can mint an NFT with a name they choose. Each NFT has a unique ID.

**Smart contract:**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract MyNFT is ERC721 {
    uint public nextTokenId;
    address public owner;
    mapping(uint => string) public tokenNames;

    constructor() ERC721("BlockBase NFT", "BBNFT") {
        owner = msg.sender;
    }

    function mint(string memory _name) public {
        uint tokenId = nextTokenId;
        nextTokenId++;
        _mint(msg.sender, tokenId);
        tokenNames[tokenId] = _name;
    }

    function getTokenName(uint _tokenId) public view returns (string memory) {
        return tokenNames[_tokenId];
    }
}
```

**Frontend features:**
- Input for NFT name
- Mint button (calls `mint()` via ethers.js)
- Shows all NFTs minted by the connected wallet
- Shows the NFT name and token ID

---

#### Option C — Decentralised Fundraiser

**What you build:**
A crowdfunding contract where anyone can donate ETH toward a goal. If the goal is met, the owner can withdraw. If the deadline passes without reaching the goal, donors can reclaim their ETH.

**Smart contract features:**
- Owner sets a fundraising goal (in ETH) and a deadline (block timestamp)
- Anyone can `donate()` ETH — tracked by address
- If goal reached: `withdraw()` lets owner collect funds
- If deadline passed and goal not met: `refund()` lets donors get their ETH back
- View functions: current raised amount, goal, deadline, donor balance

**Frontend features:**
- Progress bar (raised / goal)
- Donate form (enter ETH amount)
- Withdraw button (only shown to owner)
- Refund button (shown if deadline passed + goal not met)
- List of recent donations via events

---

### Demo Guidelines

Each student/team gets **2–3 minutes** to show:
1. Their Etherscan contract link (verified + deployed)
2. One live interaction on the frontend
3. Answer: *"What was the hardest part?"*

No slides needed. Just show the thing working.

---

### Peer Feedback

After all demos, each student writes one thing on a sticky note (or shared doc) for each presenter:
- ⭐ One thing they liked
- 💡 One thing they'd add or improve

---

### What to Learn Next

After BlockBase, here are the best next steps:

**Solidity practice:**
- [cryptozombies.io](https://cryptozombies.io) — learn Solidity through building a zombie game, completely free and interactive
- [speedrunethereum.com](https://speedrunethereum.com) — guided challenges from scaffold-eth, teaches Hardhat + real patterns
- [solidity-by-example.org](https://solidity-by-example.org) — short, focused contract examples for every concept

**Frontend + tooling:**
- [docs.ethers.org](https://docs.ethers.org) — ethers.js official documentation
- [wagmi.sh](https://wagmi.sh) — React hooks for Ethereum (the step up from plain ethers.js)
- [rainbowkit.com](https://www.rainbowkit.com) — wallet connection UI components

**Go deeper on Ethereum:**
- [ethereum.org/developers](https://ethereum.org/developers) — official Ethereum developer documentation
- [hardhat.org](https://hardhat.org) — Hardhat full documentation
- [learnweb3.io](https://learnweb3.io) — structured multi-track Web3 developer curriculum

**YouTube channels:**
- Patrick Collins — best full-length Solidity + Foundry courses (free)
- Whiteboard Crypto — concepts explained visually
- Finematics — DeFi and protocol deep dives

---

### Takeaway
> *"You built a full-stack dApp. A smart contract on the blockchain + a frontend that talks to it through ethers.js + deployed to a live testnet. That's the complete Web3 development loop."*

---

---

## Appendix — Interaction Resources Quick Reference

| Day | Resource | Link | Purpose |
|---|---|---|---|
| 1 | Anders Brownworth demo | andersbrownworth.com/blockchain | Visual blockchain + mining demo |
| 1 | SHA-256 hash tool | xorbin.com/tools/sha256-hash-calculator | Hash playground |
| 1 | Bitcoin Genesis Block | blockchair.com/bitcoin/block/0 | Explorer challenge |
| 2 | Sepolia Etherscan | sepolia.etherscan.io | Read real transactions |
| 2 | MetaMask | metamask.io | Wallet setup |
| 2 | Sepolia faucet | sepoliafaucet.com | Get free testnet ETH |
| 2 | Ethereum Genesis Block | etherscan.io/block/0 | Explorer challenge |
| 3–5 | Remix IDE | remix.ethereum.org | All Solidity coding |
| 5–6 | OpenZeppelin Contracts | docs.openzeppelin.com/contracts | ERC20/ERC721 base |
| 6 | Sepolia Etherscan | sepolia.etherscan.io | Verify deployed contracts |
| 7 | ethers.js CDN | cdn.ethers.io/lib/ethers-5.7.2.umd.min.js | Frontend library |

---

## Appendix — Solidity Cheatsheet

```solidity
// ─── STRUCTURE ───────────────────────────────────────────
// SPDX-License-Identifier: MIT
// pragma solidity ^0.8.10;
// contract Name { ... }
// constructor() { runs once at deployment }

// ─── DATA TYPES ──────────────────────────────────────────
// uint / uint256   non-negative integer
// int / int256     signed integer
// bool             true / false
// address          Ethereum address (0x...)
// string           text (always needs "memory" keyword in functions)
// bytes32          fixed-size byte array (used for hashes, ids)

// ─── VISIBILITY ──────────────────────────────────────────
// public    anyone can call / read
// private   only this contract
// internal  this contract + child contracts
// external  only from outside (not internally)

// ─── FUNCTION TYPES ──────────────────────────────────────
// (none)    changes state, costs gas
// view      reads state, free
// pure      no state read or write, free

// ─── DATA STRUCTURES ─────────────────────────────────────
// uint[] public arr;              dynamic array
// uint[5] public fixed;           fixed array
// arr.push(x)  arr.pop()  arr.length
// mapping(address => uint) map;   key-value store
// struct MyType { uint x; bool y; }

// ─── SPECIAL VARIABLES ───────────────────────────────────
// msg.sender    address that called the function
// msg.value     ETH sent with the transaction (in wei)
// block.timestamp  current block Unix timestamp
// block.number     current block number

// ─── CONTROL FLOW ────────────────────────────────────────
// if (x > 0) { } else if (x == 0) { } else { }
// for (uint i = 0; i < 10; i++) { }
// while (condition) { }

// ─── ERROR HANDLING ──────────────────────────────────────
// require(condition, "message");  reverts if false
// revert("message");              always reverts
// emit MyEvent(arg1, arg2);       emit an event

// ─── MODIFIERS ───────────────────────────────────────────
// modifier onlyOwner() {
//     require(msg.sender == owner, "Not owner");
//     _;  // run the function body here
// }
// function doThing() public onlyOwner { ... }

// ─── INHERITANCE ─────────────────────────────────────────
// contract Child is Parent { ... }
// import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
// contract MyToken is ERC20 { ... }
```
