## Easy Replication - IPFS Pubsub

This helps you setup a quick system for replicating a arbitrary amount of hashes.

Be careful since pubsub subscriptions are public and anyone can publish hashes there.

## Dump all hashes into a temporary file

```
ipfs pubsub sub test-channel > hash-list
```

Now we're listening to `test-channel` and saving everything from there into a 
file called `hash-list` that we can use for reading.

## Listen for any changes to file and pin contents that appear

```
while inotifywait hash-list; do cat hash-list | tr ' ' '\n' | ipfs pin add; done
```

Everytime that `hash-list` changes, our part inside `do` will be executed. So
everytime it changes, we cat the contents, split by space and then pipe the
hashes into `ipfs pin add` for local replication.

## Publish hashes via pubsub (on other machine)

```
head -c 4000000 /dev/urandom | base64 | ipfs add -q | awk '{print $0,"\n"}' | ipfs pubsub pub test-channel
```

Now we can publish our hashes via pubsub. We have to add a newline which would
be represented as a whitespace, which is what we split the hashes by in the 
previous step.

What the parts before `ipfs add` is doing is just generating some random content,
which we then add to ipfs, add a newline and pass in the resulting hash into
pubsub.

## Effects

Once we published our hash via pubsub, it'll be received when we setup our
subscription, which adds the hash to the file. When `inotifywait` sees that
the file has changed, it runs our little piece of script for pinning the content.
