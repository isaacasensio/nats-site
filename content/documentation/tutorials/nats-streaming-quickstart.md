+++
date = "2016-06-18"
title = "Getting Started with NATS Streaming"
description = ""
category = "tutorials"
[menu.main]
  name = "Getting started with NATS Streaming"
  weight = 9
  identifier = "tutorial-nats-streaming"
  parent = "Tutorials"
+++

This tutorial demonstrates NATS Streaming using the example publish/subscribe clients in the [Go NATS Streaming repository](https://github.com/nats-io/go-nats-streaming.git). 

1. Prerequisites: 

- [Set up your Git environment](https://help.github.com/articles/set-up-git/).
- [Set up your Go environment](https://golang.org/doc/install).

2. Download and install the [NATS Streaming Server](https://github.com/nats-io/stan-server-preview/releases).

3. Run `git clone` for the following repositories:

- NATS Streaming Server: https://github.com/nats-io/nats-streaming-server.git
- NATS Streaming Client: https://github.com/nats-io/go-nats-streaming.git

3. Start the NATS Streaming Server.

Two options:

a. Run the binary that you downloaded:

Change directory to where you downloaded the server binary and run it from the command line using the `nats-streaming-server` command.

b. Run from source:

```
$ cd $GOPATH/src/github.com/nats-streaming-server
$ go run nats-streaming-server.go
```

You should see the following, indicating that the NATS Streaming Server is running:

```
[64741] 2016/06/14 20:26:06.043324 [INF] Starting nats-streaming-server[test-cluster] version 0.0.1.alpha
[64741] 2016/06/14 20:26:06.043669 [INF] Starting nats-server version 0.8.2
[64741] 2016/06/14 20:26:06.043689 [INF] Listening for client connections on localhost:4222
[64741] 2016/06/14 20:26:06.045406 [INF] Server is ready
[64741] 2016/06/14 20:26:06.129946 [INF] STREAM: Message store is MEMORY
[64741] 2016/06/14 20:26:06.129966 [INF] STREAM: Maximum of 1000000 will be stored
```

4. Run the publisher client and publish a few messages.

For each publication you should get a result.

```
$ cd $GOPATH/src/github.com/go-nats-streaming/examples
go run stan-pub.go foo "msg one"
Published [foo] : 'msg one'
$ go run stan-pub.go foo "msg two"
Published [foo] : 'msg two'
$ go run stan-pub.go foo "msg three"
Published [foo] : 'msg three'
$ 
```

5. Run the subscriber client.

Use the `--all` flag to receive all published messages.

```
$ go run stan-sub.go --all -c test-cluster -id myID foo
Connected to nats://localhost:4222 clusterID: [test-cluster] clientID: [myID]
subscribing with DeliverAllAvailable
Listening on [foo], clientID=[myID], qgroup=[] durable=[]
[#1] Received on [foo]: 'sequence:1 subject:"foo" data:"msg one" timestamp:1465962202884478817 '
[#2] Received on [foo]: 'sequence:2 subject:"foo" data:"msg two" timestamp:1465962208545003897 '
[#3] Received on [foo]: 'sequence:3 subject:"foo" data:"msg three" timestamp:1465962215567601196
```

6. Explore NATS streaming using different options.

Common options are shwon below. See the README for all options.

```
--last          Start receiving with last published message.
--seq <seqno> 	Start at the sequence number.
```