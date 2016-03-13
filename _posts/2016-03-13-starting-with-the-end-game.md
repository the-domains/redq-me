---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: "A couple of years back, I decided to learn Go by implementing a new NEAT distribution from scratch. My plan was to use just Stanley and Miikkulainen's paper as a guide and deliver a flexible Go package. Well, the journey took a lot longer than expected and what I ended up creating was only just mildly flexible and not very good Go code either. So let's consider this a NEAT for Go reboot and, this, time giving it a fancy new name: redq.NEAT.\_"
datePublished: '2016-03-13T21:59:01.356Z'
dateModified: '2016-03-13T21:57:45.400Z'
title: Starting with the end game
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2016-03-13-starting-with-the-end-game.md
published: true
url: starting-with-the-end-game/index.html
_type: Article

---
A couple of years back, I decided to learn [Go][0] by implementing a new [NEAT][1] distribution from scratch. My plan was to use just [Stanley and Miikkulainen's paper][2] as a guide and deliver a flexible Go package. Well, the journey took a lot longer than expected and what I ended up creating was only just mildly flexible and not very good Go code either. So let's consider this a NEAT for Go reboot and, this, time giving it a fancy new name: redq.NEAT. ![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/41e485ef-d86b-4015-b308-4b105e610cf1.jpg)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/4923050e-ce6c-4c54-a0d6-716e46097fcf.jpg)

Having been through that experience, though, did give me some insignts into how I could better approach the library. I had gone all the way down the path of not just NEAT but also HyperNEAT and Novelty Search so I can now think of those things as core concepts of the library and make decisions which incorporate these newer ideas into the heart of what I am building.

A good example of this is how I build a new Network. I will discuss the actual implementation of the Network in a future post but what I settled on is that to create a Network I must first have a Substrate. After all, this is what HyperNEAT requires. But regular NEAT does not. It uses the Genome directly. How should I combined the two and keep things simple?

I began thinking of what the Genome components were actually meant to do. A Node, for instance, really just wraps a Neuron definition with some data to track it with respect to Connections. And Connections are really just wrappers for Synapse definitions with some metadata about innovation and being enabled. So, perhaps, I could treat Genome as just a wrapper, too. Ultimately a Genome provides the definition of the Network and that's exactly what a Substrate is: the Network's definition. Throw in some tracking and fitness related metadata and we're right back at Genome!

Moving Substrate into the core concept of Genomes and genes (Nodes and Conns) provides other benefits, too. As Substrate components use Position instead of a mere ID to identify Nodes, the Add Connection method of the complexifying mutator now has an easy way to determine if the potential connection is feed-forward or recurrent. Additionally, any visualiser tool (one is coming) need only work with a Substrate instead of having to support Genomes as well.

Some things are more complicated, though. Creating new Genomes, for instance, requires more complex definitions for Nodes (Positions instead of just IDs) and Conns (Source and Target are Position values not just ints representing IDs or index positions). This particular challenge can be mitigated by using the the Seed methods associated with one of the Transcriber helpers (NEAT, HyperNEAT, etc.).


[0]: https://golang.org/
[1]: http://www.cs.ucf.edu/~kstanley/neat.html
[2]: http://nn.cs.utexas.edu/keyword?stanley:ec02