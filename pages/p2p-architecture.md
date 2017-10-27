## Distributed Application Architecture

> How to build applications with IPFS and IPLD

### Practical Knowledge needed

* Distributed Architecture
    * CRDTs
    * Eventual Consistency
		* Spanning Trees

### Interesting Links

* https://github.com/socketio/socket.io-p2p
* https://www.gitbook.com/book/flyingzumwalt/decentralized-web-primer/details
* https://public.etherpad-mozilla.org/p/ipfs-needs
* https://github.com/ipfs/pm/issues/56
* https://en.wikipedia.org/wiki/Tahoe-LAFS

### Open Problems

* IP monetization

#### How to model:

##### Application Gallery

> Ability for anyone to list applications to show
> Some people have special acccess to "feature" applications that are very good

* User
* ACL - Registry people can filter from
    * Secure feeds from curators
    * Receiver makes sure updates from curators are true
    * Architecture before 1989
    * IPNS can be used to get updates from curators
* Submission
* Image

##### Forum (Tree-Talk)

* User
* Category
* Thread
* Post
* Attachments


* Provide caching/replication
* Make use of the new DAG API
* Provide search

    * swarm protocol
    * socket.io socket-p2p

##### Reddit

* User
* Subreddit
* Post
* Comment

##### Twitter

* User
* Tweet

##### Forms

* Form
* Submission
* Report

##### Wiki

* User
* ACL
* Page
* Revision
* Attachment

##### Other CRUD patterns


## Random Questions

### Orbit and IPFS
* Which one to use when?
    * Orbit for collaborative and offline
    * Orbit not a one-size-fits-all

### What's missing?

* DHT
* IPNS

### Inter-Application Linking

Applications should be able to link to each others content. 

### Gossip over IPFS

Does it make sense?

### Dealing with search, trending topics and easy discoverability

* How can we get search to work?
* How can we see popular content on the network?
* Is there other ways we can provide discoverability?

### Dealing with lots of data

* How to deal with operations that normally happen in the business logic in a backend?

* Having bunch of nodes that can help out process loads of data, acting as "supernodes" or
similar to that. Example would be when live-streaming, camera acts as root-sender, sends to one
machine which then helps to propagate.

### Private Communication

Encrypt content with node keypairs, decrypt with public key on receiving end

### Security & Privacy & Spam

* What security concerns should you keep in mind?
* How to prevent spam?
    * reputation
    * proof-of-work
    * crypto-puzzle
