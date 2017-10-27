Bootstrap nodes is a couple of addresses of nodes in a P2P network that you
connect to directly when your node is started. This helps with discovery when
nodes tells each other about the presence of other nodes.

IPFS uses bootstrap nodes that are hosted by the IPFS community.

You can interact with your list of bootstrap nodes via the `ipfs bootstrap` command.

```
SUBCOMMANDS
  ipfs bootstrap add [<peer>]... - Add peers to the bootstrap list.
  ipfs bootstrap list            - Show peers in the bootstrap list.
  ipfs bootstrap rm [<peer>]...  - Remove peers from the bootstrap list.
```
