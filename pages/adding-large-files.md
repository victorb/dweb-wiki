Adding large files should work fine, problem is that with the way the default
Datastore works, files are duplicated on the harddrive so a 5GB file on your
local machine would need 10GB in total, 5GB in IPFS and 5GB for the real file.

A large file would also be split up into many smaller blocks and when adding
all of those they are being "reprodvided" (not sure about that term) to the DHT.

You can disable that by running `ipfs config Reprovider.Interval "0"`
