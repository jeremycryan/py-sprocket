# py-sprocket
A Python socket abstraction for quick and easy networking prototypes

### Overview
This library is for getting socket communication up and running in a few
minutes in Python, without having to directly deal with threading, error
handling, and text parsing. It is not meant to be maximally efficient,
be completely watertight, or replace whatever your existing system is if
you already know what you're doing.

It's also a work in progress! Let me know via issues if there are any
requested features or caught bugs.

### Packets
This framework is built around sending `Packet` objects that map strings
to arbitrary (picklable) Python objects.

``` 
>>> from sprocket import Packet
>>> p = Packet(x=3, y=2, id="Player 1")
>>> p.x, p.y
(3, 2)
>>> p.name
'Player 1'
```

### Sprockets
The `Sprocket` object serves as the interface point for clients. A
Sprocket connects to the server via IP address, and can send and receive
packets.

There's threading happening under the hood, but the Sprocket object
itself never blocks, so you can interface with it in your main loop.

```
s = Sprocket("127.0.0.1")

while True:

    for packet in s.get():
        my_script.process_packet(packet)
    
    for packet in my_script.outgoing():
        s.send_packet(packet)
        
    state = {"x": my_script.get_x(),
             "y": my_script.get_y()}
    s.send(**position}
```

`Socket.get` returns a list of all packets that have been received since
it was last called.

`Socket.send_packet` sends a packet to the connected Servo.

`Socket.send` constructs a packet directly from keyword arguments.

### Servos
The `Servo` object acts as a central server which can accept multiple
Sprockets. It has a generally similar interface to a Sprocket.

```
s = Servo()

while True:

    for packet in s.get():
        my_server.process_packet(packet)
        
    for client in s.clients:
        print(client.ip, client.port)
        
    s.sendall(**server_state)
```

It's probably worth noting that I personally plan on using this library
for little multiplayer games. In any practical application, you probably
wouldn't want to send the server's entire internal state to all clients
every loop.

### Example application
Coming soon...?
