# BlockBase — Day 1

_Mentored by FEC_

---

## How Does a Blockchain Work?

---

### Hashing

The reason a blockchain can't be silently tampered with comes down to one mathematical function: a **hash function**.

Give a hash function any input — a single word, a sentence, an entire novel — and it produces a fixed-length string of characters called a **hash**. Bitcoin uses a specific one called SHA-256, which always outputs exactly 64 hexadecimal characters regardless of what you put in. Hash a single letter and you get 64 characters. Hash a 500-page book and you still get 64 characters.

What makes it useful is how it behaves. Hash "Blockchain" and you'll always get the same output, on every computer in the world, every time. That's just how the function works — it's deterministic. But change a single character — lowercase the B — and the output is completely unrecognisable from the first. Not slightly different. Unrecognisable. This sensitivity is called the **avalanche effect**, and it's a deliberate design property, not an accident.

```
Input: "Hello"
SHA-256: 185f8db32921bd46d35fa8acbcd57c8dee7a567e37e1e3ddc63edd3...

Input: "hello"   ← just changed the capital H
SHA-256: 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043...
```

The other critical property is that the function only runs one way. Given a hash, there is no mathematical process to recover the original input. The only approach is to guess inputs until one produces a matching output. For SHA-256's output space, that's not a feasible strategy — the number of possible outputs is larger than the estimated number of atoms in the observable universe.

Before going further, open this and spend a few minutes with it:

> https://xorbin.com/tools/sha256-hash-calculator

Hash `"Blockchain"`, copy the output, then hash `"blockchain"` (lowercase b) and compare. Almost none of the 64 characters will match. Then hash `"blockchain "` with a trailing space at the end — a single invisible character changes the output completely.

Next, hash `"BlockBase2026"`, copy the result, close the tab, reopen it, and hash it again. The output is identical. That's determinism — the function produces no variation based on when or where it runs.

Finally, try to figure out what input produced this:

`a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3`

The answer is `123`. Three characters in, 64 characters out — and there's no path back.

---

### What a block contains

Each block is a data structure with a small number of fields:

- its position in the chain (block number)
- a timestamp
- the list of transactions it contains
- a number called a **nonce** (used in mining, covered below)
- the hash of the previous block
- its own hash, computed from everything above

```
Block #847,231
├── Timestamp: 2024-03-15 14:22:07
├── Transactions: [Alice → Bob: 0.5 BTC, Carol → Dave: 1.2 BTC ...]
├── Previous Hash: 000000000000000abc123...
├── Nonce: 3,847,291
└── This Block's Hash: 000000000000000def456...
```

Notice that the block's hash starts with several zeros. That's not arbitrary — it's the direct result of mining, which is covered next.

That second-to-last field is what creates the chain. Every block embeds the hash of the block before it. So if you go back and change any data in block 5, block 5's hash changes. But block 6 stored block 5's original hash — so now block 6 contains a reference that no longer matches. Block 7 stored block 6's hash, which also now reflects an inconsistency. Every node on the network can detect the break immediately.

The best way to see this directly is this demo:

> https://andersbrownworth.com/blockchain

Work through all four tabs in order. On the **Hash** tab, just type something and change one character — watch the output flip entirely. On the **Block** tab, a block is only valid when its hash starts with `0000`. Change the data field and the block turns red. Click Mine and watch the browser try nonces one by one — 1, 2, 3... thousands per second — until it lands on one where the hash of (your data + that nonce) happens to start with four zeros. When it finds it, the block goes green. Change one letter in your data and the nonce is immediately useless — you have to mine again from scratch.

The **Blockchain** tab is where it clicks. Change a word in Block 1 and every block after it turns red — each one stored the previous block's hash, and those hashes are now wrong all the way down. Mine Block 1 to fix it, but Blocks 2, 3, and 4 are still broken. You'd need to re-mine each of them in sequence. That's the entire cost of rewriting history.

The **Distributed** tab adds the network. Three peers each hold a copy of the same chain. Tamper with Peer A's Block 2 — only Peer A's chain goes red. Peers B and C still agree with each other. When the network compares, Peer A's version is outvoted and rejected. No authority made that call. The math did.

---

### Mining

Someone has to bundle transactions into a block and propose it to the network. On Bitcoin, that's the job of **miners**, and the mechanism deciding who gets to do it is called **Proof of Work**.

Many computers want to add the next block at the same time. They can't all propose different blocks — that would create conflicting histories with no way to resolve them. Bitcoin's solution is a race with a puzzle that requires real computational work to solve, and a reward paid to whoever finishes first.

The puzzle: find a number (the **nonce**) such that when you hash it together with the block's contents, the resulting hash starts with a specific number of zeros. There's no formula or shortcut. You start at zero and increment, hashing each time, until something produces the right output. On modern mining hardware, that means trying billions of candidates per second.

When a miner finds a valid nonce, they broadcast the block. Every other node verifies it in one calculation — check that hashing (block data + claimed nonce) produces a hash with enough leading zeros — and if it passes, they accept the block and the competition moves to the next one. The winner earns newly created Bitcoin.

This asymmetry is the core of what makes Proof of Work work. Finding a valid nonce requires billions of attempts. Verifying someone else's solution takes one calculation. Cheating is computationally expensive. Catching cheaters is essentially free.

That reward is how new Bitcoin enters circulation. The total supply is hard-capped at 21 million coins. Every 210,000 blocks — roughly every four years — the reward halves. It was 50 BTC in 2009, then 25, 12.5, 6.25, and after the April 2024 halving, 3.125 BTC. The final Bitcoin will be mined around 2140, after which miners earn only transaction fees.

Proof of Work is intentionally energy-intensive — the computational difficulty is the security. Bitcoin's network today consumes roughly as much electricity as a medium-sized country. This is controversial, and it's one of the primary reasons Ethereum chose a different model. Whether that energy expenditure is justified as the cost of trustless consensus, or simply wasteful, is a question with thoughtful people on both sides.

To rewrite a block that's already in the chain, you'd need to re-mine it and every block after it, while outpacing the rest of the honest network the entire time. Bitcoin's combined mining power currently exceeds the world's top 500 supercomputers. Controlling more than half of it — the threshold needed to reliably overwrite history, called a **51% attack** — would cost an almost incomprehensible amount of money. This doesn't mean every blockchain is equally secure. Smaller chains have far fewer miners, and some can theoretically be attacked for a few hundred dollars per hour.

Check the estimated hourly cost of a 51% attack on different chains here:

> https://crypto51.app

Look up Bitcoin, then look up a few smaller names. The difference puts the security model in perspective.

---

### Proof of Stake

Bitcoin still runs on Proof of Work. Ethereum doesn't, not since September 2022, when it completed a transition called **The Merge** — switching the entire live network from one consensus mechanism to another without downtime, while hundreds of billions of dollars in assets sat on it. One of the more technically audacious things done in software.

Ethereum now runs **Proof of Stake**. The security model is fundamentally different.

Under Proof of Work, attacking the network is expensive because computational work costs electricity. Under Proof of Stake, the cost is denominated in ETH. To become a **validator** — someone who proposes and votes on blocks — you lock up 32 ETH as collateral. Validators are chosen randomly to propose the next block, and a committee of other validators attests to its validity. If a validator tries to submit a fraudulent block, their staked ETH gets **slashed**: some or all of it is permanently destroyed by the protocol. The cost of cheating isn't just wasted resources — you lose the assets you put up.

|                         | Proof of Work                   | Proof of Stake                           |
| ----------------------- | ------------------------------- | ---------------------------------------- |
| Who proposes blocks?    | Whoever solves the puzzle first | Randomly selected from staked validators |
| How to participate      | Buy mining hardware             | Lock up 32 ETH as collateral             |
| Punishment for cheating | Wasted electricity              | Staked ETH is slashed (destroyed)        |
| Energy use              | Extremely high                  | ~99.95% less than PoW                    |
| Used by                 | Bitcoin                         | Ethereum                                 |

Ethereum switched for three reasons. Energy consumption was the most immediate — Proof of Stake uses 99.95% less electricity. Second, PoS is a prerequisite for certain scaling upgrades that are architecturally difficult to implement under Proof of Work. Third, the economic security model changes in a meaningful way: successfully attacking Ethereum PoS requires first acquiring an enormous amount of ETH to stake, and any attack would likely crash the price of that same ETH. Attackers risk destroying the value of the asset they're using to attack. Ethereum's designers considered this a more elegant deterrent than the hardware and electricity cost of a PoW attack.

Whether PoS is more or less secure than PoW is genuinely debated. Bitcoin's camp argues the physical, irreversible cost of electricity makes PoW attacks impossible to hide. Ethereum's camp argues slashing creates equally strong disincentives at a fraction of the environmental cost. Both arguments have merit, and both systems are considered secure at their respective scales.

---

### Nodes

The blockchain doesn't run on a server. It runs on **nodes** — computers that voluntarily participate in the network by running the protocol software. There's no company operating Bitcoin. No headquarters you could locate on a map.

A **full node** downloads and independently verifies the entire chain from the genesis block. It doesn't trust anyone's account of the current state — it checks everything itself. Bitcoin's full chain is around 600 GB. Ethereum's is larger.

Not everyone runs a full node. **Light nodes** store only block headers and request specific data from full nodes when needed. Your MetaMask extension is effectively a light client — it doesn't download the full chain; it asks full nodes for what it needs.

On Ethereum, **validators** are full nodes with 32 ETH staked that participate in block proposal and attestation. On Bitcoin, **miners** are full nodes competing in the Proof of Work puzzle.

Because nodes are spread across every country, there's no single point of failure. If a government shuts down every node in one region, the network continues running everywhere else. No one has the authority to order the Bitcoin network offline — there's no one to give that order to.

---

### The Genesis Block

Everything covered here becomes concrete when you look at the actual data. Open Bitcoin's block zero — mined by Satoshi in January 2009:

> https://blockchair.com/bitcoin/block/0

Find the message Satoshi embedded in it and think about what it proves. Note the original block reward, the number of transactions, the hash. Then open any recent block and compare — the transaction volume, the fees paid, the reward after sixteen years of halvings.

The entire history from that first block to today is an unbroken chain. Every link is verifiable by anyone with a computer and enough disk space.
