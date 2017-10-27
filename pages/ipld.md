IPLD is authenticated and distributed data structures.

It came from MerkleDAG's.

### Example

Avatar Object

```js
{
	"filename": "avatar.jpeg",
	"content": Buffer([...]) // Binary content
}
```

Once we processes this as a IPLD object, we get the hash `QmAvatarObj` and
can use that when creating other objects.

```js
{
	"username": "Victor",
	"avatar": {"/": "QmAvatarObj"}
}
```

And once we proccessed this user object as IPLD, we get the hash `QmUserVictor`.

```js
{
	"username": "David",
	"avatar": {"/": "QmAnotherAvatar"},
	"friends": ["QmUserVictor"]
}
```

Now once we have this, when can navigate objects by using paths.

```
$ ipld get QmUserDavid
{
	"username": "David",
	"avatar": {"/": "QmAnotherAvatar"},
	"friends": ["QmUserVictor"]
}
$ ipld get QmUserDavid/friends/0/
{
	"username": "Victor",
	"avatar": {"/": "QmAvatarObj"}
}
$ ipld get QmUserDavid/friends/0/avatar
{
	"filename": "avatar.jpeg",
	"content": Buffer([...]) // Binary content
}
```

Anything that using MerkleDAG in their application can be converted into IPLD
objects and then traversed via the resolvers IPLD provides. Applications can
also provide custom resolvers for new content that hasn't been added to the main
implementation yet.
