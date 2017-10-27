When adding many files, IPFS can sometimes be overwhelmed. This is a copy-paste
from https://github.com/ipfs/notes/issues/212 about how you can make it faster.

Some of my thoughts on adding lots of data to ipfs:

go-ipfs is currently still alpha software. It is designed to handle absurdly
huge amounts of data across vast expanses of spacetime, but our current
implementation has its fair share of inefficiencies. This guide will serve as
a collection of optimization notes and best practices for efficiently storing
large amounts of information in ipfs.

## Daemon Configuration

This section discusses configurations to apply before starting the process of
ingesting data into ipfs.

### Set flatfs 'NoSync'
```sh
ipfs config --json Datastore.NoSync true
```

Ipfs currently stores all data blocks in flat files on disk. There is quite a
ways to go in optimizing this storage engine, but one quick optimization for
now is to disable some excess fsync calls made by the code. **The drawback this
has is that if the machine ipfs is running on unexpectedly crashes (without                                                                                                                         
proper disk unmounting) then some recently added data may be lost.**                                                                                                                                  
                                                                                                                                                                                                    
### Disable Reproviding                                                                                                                                                                             
```sh                                                                                                                                                                                               
ipfs config Reprovider.Interval "0"
```                                                                                                                                                                                                 
                                                                                                                                                                                                    
By default, the ipfs daemon will announce all of its content to the dht once a                                                                                                                      
day. This works great for small to medium sized datasets, but for huge datasets                                                                                                                     
this becomes incredibly costly. Until we optimize the content routing system                                                                                                                        
(see: https://github.com/ipfs/notes/issues/162), it's best to disable this                                                                                                                          
feature.                                                                                                                                                                                            

## The Add Process

The primary way to get data into ipfs is through the `ipfs add` command.
There are a few optimizations here and different things to note that will aid
in efficiently getting data ingested.

### 'Local' adding
When content is added to ipfs in this way, we automatically start announcing
the content to the dht as it is added. For huge masses of data, we would prefer
not to do that given the cost. To avoid this, pass the `--local` flag when
invoking ipfs add. For example:
```sh
ipfs add -r --local /data/some_huge_dataset
```

### Raw Leaves
All file data that goes into ipfs is broken into chunks, and built into a
merkledag. Initially, the leaf nodes of the dag had some amount of framing.
Recently (still in master at time of writing, should ship in 0.4.5) we added an
option to add that allows us to create leaf data nodes without that framing.
This cuts roughly 12bytes per 256k chunk off, but the real benefit it provides
is making the blocks stored on disk evenly divisible by 4096, resulting in
fewer wasted disk blocks.

Example:
```sh
ipfs add --raw-leaves -r /data/some_huge_dataset
```

### Breaking Up Adds
ipfs add calls are not currently interuptable, if something happens during the
add you will have to restart from the beginning (though previously added
segments will progress much more quickly). To mitigate this risk, it is
generally advisable to add smaller amounts of data and then patch the peices
together afterwards. This process might look something like this:
```sh
# add the individual pieces
$ ipfs add part1
QmPartOne
$ ipfs add part2
QmPartTwo
$ ipfs add part3
QmPartThree
# now patch them all together
$ ipfs object new unixfs-dir # get an empty directory
QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn
$ ipfs object patch QmUNLLsPACCz1vLxQVkXqqLX5R1X345qqfHbsf67hvA3Nn add-link part1 QmPartOne
QmStuff1
$ ipfs object patch QmStuff1 add-link part2 QmPartTwo
QmStuff2
$ ipfs object patch QmStuff2 add-link part3 QmPartThree
QmFinalDir
```

