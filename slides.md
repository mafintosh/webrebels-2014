BitTorrent, Streams and JavaScript

  Mathias Buus, @mafintosh
  https://github.com/mafintosh

----

BITTORRENT!

----

Normally when you fetch content online you do this


  [client]
     ^
     | (epic movie)
     |
  [server]


----

This is nice and simple.

Scenario: What happens when 1.000.000 clients arrive?


   [client1] [client2] ... [client1M]
        \        |         /
         \       |        /
          \      |       /

              [server]

    (much traffic)
                        (so scale)

----

Server blows up because it cannot keep up with the traffic

            --_--
         (  -_    _).
       ( ~       )   )
     (( )  (    )  ()  )
      (.   )) (       )
        ``..     ..``
             | |
           (=| |=)
             | |
         (../( )\.))

      s       r      e
         e                 r
                 v

----

Idea!

Remove the server and have all clients share data
with each other

----



[client1]  --->  [client2]
  ^      \        /
  |       \      /
  |       [client3]
[client5]     \
               \
               ...


----

Now if one of the clients blows up we just fetch data from someone else


  :(    --->  [client2]
  ^       \        /
  |        \      /
  |        [client3]
[client3]      \
                \
                ...

----

Now if one of the clients blows up we just fetch data from someone else


             [client2]
                  /
                 /
[client5] -> [client3]
                 \
                  \
                  ...

----

So peer to peer seems nice...

But can we trust data coming from clients?
What if someone sends us (fail movie) instead of (epic movie)?

----

Let's start by dividing (epic movie) into pieces and hash them


piece1  --> hash of piece1
piece2  --> hash of piece2
...
piece1M --> hash of piece1M


We can use these hashes to verify data received

----

We could store this data on a trusted server


          [client]
              ^
              |
  (file-with-list-of-hashes)
              |
          [server] - (hey remember that slide of me exploding)


An OK solution but the list of hashes can be big

----

What if the server just stored a single hash
that was the hash of all piece hashes?


                       | hash of piece1
info-hash  --> hash of | ...
                       | hash of piece1M


[server] can probably handle 16 bytes of data

----

We just fetch all the piece hashes of clients


            [client3] --> (hash-of-some-piece)
                                         |
                                         v
[client1] --> (hash-of-some-piece) --> [client]
                                           ^
                                           |
              [client2] --> (hash-of-some-piece)


We use the info-hash to verify the hashes

----

So we can fetch data and verify it

But how do we find peers that share our (epic movie)?

----

 (so distributed)

  DISTRIBUTED HASH TABLE!

                   (web scale)
(much reliable)

----

Every client joins the DHT

Bootstrap through a couple of known seed nodes
Add the following data


key    = info-hash
value  = my-ip : my-port


----

The DHT is H U G E

~10.000.000 nodes at any time

https://dsn.tm.kit.edu/english/2936.php

----

SUMMARY:

Given an info hash

1. find a bunch of peers in the dht
2. connect to them and get metadata
3. verify metadata using info hash
4. get data from peers
5. verify data using metadata

----


BITTORRENT IS PRETTY SWEET FOR SHARING CONTENT!


----


I'm a node hacker


----

Some things I like about node:

- async
- community
- javascript
- npm

----

Some things I like about node:

- async
- community
- javascript
- npm
- STREAMS !!!!

----

NODE.JS STREAMS + BITTORRENT = <3 ?

----

REAL TIME + NODE.JS STREAMS + BITTORRENT = <3

----

Trival implementation:

stream wants pieces 1 - 10

peer1 --> piece1 --> stream
peer2 --> piece2 --> stream
etc

----


Risk of high latency ==> does not seem real time


----

Better implementation:

find top peers (high speed, been succesful before)

top peers --> piece1     --> stream
rest      --> piece2..10 --> stream

----


The top peers will always help fetch the most critical pieces concurrently
(usually only a couple)


----

This is how torrent-stream works!

  npm install torrent-stream

https://github.com/mafintosh/torrent-stream

----


(mathias, show them the demo of torrent-stream)


----


What if we streamed video files as well?


----


TORRENT-STREAM + VLC = <3 <3 <3


----

peerflix combines torrent-stream and vlc
(the thing that made popcorn time stream torrents)

  npm install -g peerflix

https://github.com/mafintosh/peerflix

----


(mathias, show them the demo of peerflix)


----


What if we could mount the torrent in files instantly
and just access them as any file?


----


MAD SCIENCE TIME!


----


TORRENT-STREAM + FUSE = [head-explosion-animation]


----

torrent-mount allows you to mount all files inside a torrent instantly

  npm install -g torrent-mount

https://github.com/mafintosh/torrent-mount

----


(mathias, show them the demo of torrent-mount)


----


People to follow

  feross      https://github.com/feross
  webtorrent  https://github.com/feross/webtorrent


----

Tak! Questions?

  Mathias Buus, @mafintosh
  https://github.com/mafintosh
