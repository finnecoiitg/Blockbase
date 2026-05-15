# BlockBase — Day 1

_Mentored by FEC_

---

## What is a Blockchain?

---

Think about the last time you sent money to someone. You opened an app, typed an amount, hit send. It worked because your bank updated two numbers in their database — subtracted from yours, added to theirs. You trusted the bank to do that honestly. That trust is the entire foundation of how money moves today.

Now consider a harder version of the same problem. You want to send value to a complete stranger, in another country, without involving any bank or intermediary at all. No one you both trust exists in this transaction. How do you convince them the money actually left your account? And more importantly, how do you prevent the most obvious form of fraud in a purely digital system — spending the same money twice by sending it to two different people at once?

This second problem has a name: the **double-spend problem**. For decades it was considered unsolvable without a trusted authority in the middle. Every digital payment system ever built — Western Union, PayPal, Visa, your bank — exists fundamentally to solve this. Remove the intermediary, and you have no reliable way to stop someone from duplicating a digital payment the same way you'd duplicate a file. Satoshi Nakamoto published a solution to this in 2008.

---

### Bitcoin and the person nobody knows

The paper was titled _"Bitcoin: A Peer-to-Peer Electronic Cash System"_ and ran nine pages. The author signed it **Satoshi Nakamoto**.

Nobody knows who this is. Not a mystery waiting to be solved — genuinely unknown. Satoshi could be one person or several, could be living anywhere in the world. The network they designed secures trillions of dollars in value today, and Satoshi has never come forward to claim credit, collect the roughly one billion dollars worth of Bitcoin sitting unmoved in their original wallets, or send a single verifiable message since 2010. Journalists have spent careers on this. The FBI has investigated. Nothing conclusive.

Satoshi's proposal was conceptually simple: instead of one bank keeping the authoritative ledger, have thousands of computers across the world each maintain an identical copy. If everyone holds the same record, you can't secretly alter yours — you'd have to alter everyone else's simultaneously, and the honest majority would immediately reject your version.

Bitcoin launched in January 2009. The first block ever recorded — the **Genesis Block** — was mined by Satoshi themselves. Embedded in its data is a line of text that now exists permanently on the chain:

> _"The Times 03/Jan/2009 Chancellor on brink of second bailout for banks"_

A headline from that morning's newspaper. It's not just a political statement — it's also proof the block wasn't secretly pre-mined earlier, since the headline couldn't have been included before it was printed.

The first Bitcoin transaction came shortly after: Satoshi sent 10 BTC to **Hal Finney**, a cryptographer and one of the earliest contributors to the project. Hal had ALS and passed away in 2014. He had his body cryonically preserved. He wanted to see how this turned out.

---

### What a blockchain actually is

A **blockchain** is a distributed, digital, immutable ledger.

_Distributed_ means there is no single copy and no single point of control. Thousands of computers independently hold the full history. There is no server room you can raid, no company you can legally compel to delete a record.

_Digital_ means it exists only as data. No physical form, no paper ledger, no filing cabinet that can be seized or destroyed.

_Immutable_ means once data is recorded, it cannot be changed without the rest of the network immediately detecting the inconsistency. Not difficult to change — structurally detectable if changed. The mechanism behind this is covered in the next task.

_Ledger_ just means it's a record of transactions, the same concept as an accounting ledger, maintained collectively by thousands of machines instead of one.

Bitcoin's blockchain is a record of every BTC transaction since January 2009. But the design is more general than money. Any data requiring a permanent, tamper-evident, trustless record can be stored this way — smart contracts, asset ownership, supply chain logs. All of these are built on the same model Satoshi described.

---

### How the chain tracks history

Every blockchain starts from a **genesis state** — the initial conditions at block zero. Bitcoin's is January 2009. Ethereum's is July 2015. Everything since is the result of applying every subsequent transaction to that starting point, in order, one block at a time.

This means the current state of the entire network can be independently verified by anyone. Download the full history, replay it from block zero, and the math either checks out or it doesn't. No authority required.

Transactions are batched into **blocks** for efficiency rather than being confirmed individually. Each block references the one before it, which is what creates the chain structure. Once a block is added, it doesn't move.

---

**Watch:** How does a blockchain work — Simply Explained
https://www.youtube.com/watch?v=SSo_EIwHSd4
