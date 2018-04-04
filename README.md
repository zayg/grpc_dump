We may need a grpc dumper to debug the GRPC request transport behavior from the network layer.

# Goals
The initial version will be able to:

* capture the GRPC HTTP2 streams on Linux.
* analyse both the HTTP2 control frames and GRPC message in DATA frames.
produce human readable output. 
* support some basic filters of RPC name, src/tgt address, packet length limitation, etc..

# Solutions
|                              Solution                              |                                      Pros                                      |                                        Cons                                       |
|:------------------------------------------------------------------:|:------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------:|
| Wireshark dissectors + tcpdump                                     | 1. reduce develop work                                                         | 1. not work in realtime<br/>2. hard to parse large grpc messages<br/>3. inflexible to use |
| Integrate in the client/server by tracing http2 frames inside grpc | 1. able to get more information like socket options, flow control window size. | 1. may have performance side effects<br/>2. inflexible to use<br/>3. hard to maintain     |
| Stand alone (pcap)                                                 | 1. flexible to use on both server/client side.                                 | 1. hard to get the whole picture of a stream. (like flow control window size)     |
We use the third solution for its flexibility and minor side effects.

# Architecture
The tool will be consist of three parts:

#### Packet Capturing

We will use libpcap to capture the TCP packets and put the reference into a queue. Filtering the TCP packets by target and packet length should be enough to get the payload only contains HTTP frames.

#### Packet Parsing

We will need a simple 

#### Format Logging

Details
Progress
Test
