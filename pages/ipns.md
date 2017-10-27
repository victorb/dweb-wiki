IPNS is a mutable reference system on top of IPFS.

It allows you to publish hashes under mutable IDs that you can create.

## Basic Example

```
$ head -c 100 /dev/random | base64 | ipfs add -q | ipfs name publish
```

Takes random data, adds to IPFS and takes the resulting hash and publishes it
under your running node's peer ID.

## Using other key

You can also publish under other keys.

First, generate new key:

```
$ ipfs key gen victor --type rsa --size 1024
QmSgbwUKNMj23asfn7MB7RbYoaqoHgDwgvrZF3da5fq2Km
```

Then use key when publishing:

```
$ head -c 100 /dev/random | base64 | ipfs add -q | ipfs name publish --key victor
Published to QmSgbwUKNMj23asfn7MB7RbYoaqoHgDwgvrZF3da5fq2Km: /ipfs/QmP1jnv4UgbB8Mm8gyEAxfLYoSFMW3PNqhC4duwvn1Eqje
```
