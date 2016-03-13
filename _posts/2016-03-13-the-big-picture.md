---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: 'Having introduced the new libary in a previous post, I thought it might be helpful to describe the overall architecture of the library. At the very core is the concept of an Experiment. Experiments represent all the settings and tools necessary for examining a problem and finding a solution.'
datePublished: '2016-03-13T21:46:56.026Z'
dateModified: '2016-03-13T21:44:45.807Z'
title: The big picture
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2016-03-13-the-big-picture.md
published: true
url: the-big-picture/index.html
_type: Article

---
Having introduced the new libary in a previous post, I thought it might be helpful to describe the overall architecture of the library. The top-most package contains the main algorithm and interface definitions. Actual implementations are create in sub-packages.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/3d01ff10-af64-4bee-bd7d-b5cbd9aaa57b.jpg)

At the very core is the concept of an Experiment. Experiments represent all the settings and tools necessary for examining a problem and finding a solution.

Experiments are run within a Host. Two hosts are provided, Console and Web, and there are starter examples that help with their use. Console basically provides a one-time running of the Experiment with all output going to the terminal window. Web takes a client-server approach. The Experiment is run within the client but all data are collected on the server. The server provides graphs and other visualisation tools as well as the opportunity to group Experiments into a trial for comparison (useful for comparing different values for the various settings).

Experiments are comprised of different helpers. These are all described with interfaces in Go so that the choice of implementation is left up to the user. (Note: the starter package wraps these choices up neatly into a NEAT and HyperNEAT set of examples.) 

* Comparer - Provides the mechanism for comparing two Genomes with each other. An implementation of Distance is provided

* Crosser - Crosses to Genomes together to produce a child Genome. A simple implementation is provided.

* Evolver - Creates a new Population from the current one. A Generational model (used in traditional NEAT) is provided and a Real-time one is in the works. 

* Mutator - Mutates a Genome in some particular way. Weight and Complexify are available now. As is Activation which is used in HyperNEAT. Phased (à la Coling Green, including Simplify) is also avaiable and is demonstrated in the OCR example. The Join function is helpful here for creating a single Mutator by combining others. 

* Searcher - This is the helper that explores the solution space using the Phenomes in the current population. Both a Serial (useful for debugging) and a Concurrent version are delivered. 

* Transcriber - Turns a Genome into a Substrate. Both NEAT and HyperNEAT are provided. ES-HyperNEAT will come along shortly.

* Translator - Turns a Substrate into a Network. A simple, array (implemented as a slice in Go)-based Network is implemented.

* Speciater - Divides the Genomes within the Population into separate Species. Both a simple and Dynamic version are provided.

* Watcher - Receives the Population after evaluation. This can be useful for collecting statistics, determining the best Genome, displaying progress or any other activity.

Lastly, there is the Evaluator. This helper performs the actual evaluation within the problem space using one of the Networks (as a Phenome) from the current Generation. It is possible to get things going using just your own Evaluator and some settings by using the starter package.