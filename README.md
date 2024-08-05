# eXaMineR
Free and open source monero tracing tool

Also known as Snooper Testnet

![tracing an example transaction, this one: c97d96fe61b2aee18a04fdb0975594c7e6d3334036e0e57e70666b9e5fe309b3](https://supertestnet.github.io/examiner/examiner-example.gif)

I recently explored the tracability of monero and found this gem from Ciphertrace: https://news.bitcoin.com/ciphertrace-enhanced-monero-tracing-capabilities-governments/

It's a really neat visualizer of the monero blockchain with a way to trace it. And I wanted to make something similar. So I did! Please help me improve it.

# How to try it

Click here: https://supertestnet.github.io/examiner/

Here is an example of a transaction where it identifies the sender by eliminating decoys through recency bias detection: e32ab58394a86b1de3bbc89febd45472044951cecb0d156fb975d994d19151b0

# Video demo

https://youtu.be/OwkiSeoaaF8

# How it works

Mostly it gives you the basic info about a transaction and buttons for eliminating "decoy senders" whom you are mostly expected to identify using off chain data. I have a way to manually identify ringsig members as decoys but I haven't automated it yet. What I want to do is let you import a monero address, its view key, and its private key. Then I could write software to automatically identify your own keys as a decoy, because if you have the view key and private key, you know whether you signed any transaction, so if you've been used as a decoy you can automatically filter that out.

## Merge analysis and recency bias

Right now it also has two automated heuristics: merge analysis and recency bias. If you received coins in two very close blocks and then spend them together, your ring, which should contain keys from semi-random blocks, will have two suspiciously close blocks where you held both inputs. So you can identify those as the real spender's coins.

Recency bias takes advantage of the fact that most decoy selection algorithms are biased toward selecting keys from recently created utxos (on the principle that actively circulating coins are more likely to be spent than old ones). When you have a group of "new coin" decoys, coins that are significantly older stick out like a sore thumb, and you can plausibly identify them as the real spender's coins.

## Potential additional heuristics

Other metrics I want to add include bot detection (which is based on identifying txs with many outputs, since these tend to be created by automated software rather than a real person) and taint tree analysis (which creates trees of possible senders fanning out backward in time from a known destination). I also think I can fingerprint wallets by their coin selection algorithm and then identify change when it shows up in a tx that uses the same algorithm.

## Things I learned

One thing I learned while making this is that tracing monero is possible but in most cases it seems to take a considerable amount of manual work, a large set of off chain data about addresses that you know the view keys for, and regularly creating xmr transactions yourself so that you can identify your outputs as decoys when they show up in other people's transactions.

My two automated heuristics right now only identify the sender in about 1 in 15 cases, so most of the time when looking at a transaction all you see is a tool for manually filtering out decoys on your own, which is still useful, but more automation is desirable.

I hope to add about four more automated heuristics but I still think you'll need a considerable amount of off chain data entry to get really good data.
