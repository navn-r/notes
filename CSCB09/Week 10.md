---
title: Week 10
created: '2020-08-02T21:40:00.627Z'
modified: '2020-08-03T18:51:53.472Z'
weight: 10
---

# Week 10

## Sockets
> Another way for two processes to communicate

### Characteristics of Sockets

- Has 2 sides: Server and Client
  - Server: has a publishable address
  - Client: contacts Server by published address
- Unrelated processes, even on different computers, can contact each other through sockets

### Socket Varieties

#### By Domain

- Unix Domain: Local to the computer, address is a filename
- IPv4: Over the network, 32-bit address + 16-bit port number
- IPv6: 128-bit address and over the network

#### By Abstraction Level

- Datagram: 
  - Per packet 
  - Packet boundary preserved, packet order is not 
  - Unnoticed packet loss
- Stream: 
  - Network stack works hard to confirm, timeout, resend 
  - Preserves data order 
  - No packet boundary

### Stream Socket Workflow

#### Client

1. Call `socket`, creates the socket fd
2. Fill in the address struct and use `connect` to connect to the server at the address
3. Use the socket fd to communicate with the server
4. Close the fd when done

#### Server

1. Call `socket` to create the server socket fd
2. Fill in the address struct and use `bind` to bind the sfd to the address
3. Call `listen` 
4. Loop
    1. Call `accept(sfd)` to wait for client to connect, gets back a client fd   
    2. Use the cfd to communicate with client, close when done
5. Close the server socket fd if no longer waiting for clients

### Creating Sockets
```C
int socket(int family, int type, int protocol); // returns socket fd > 0, -1 if error
```
- `family`: `AF_UNIX`, `AF_INET` (IPv4), `AF_INET6` (IPv6)
- `type`: `SOCK_DGRAM`, `SOCK_STREAM`
- `protocol`: `0`


### IPv4 Addresses and Port struct

- Adresses are 32-bit, and identifies network interfaces (computers)
- Each byte is seperated by dots. (ex `142.1.96.164`)
- `dig` can look up IP Addresses from domain names by asking Domain Name Servers (DNS)
- Port struct:
  ```C
  struct sockaddr_in {
    sa_family_t     sin_family;  // AF_INET
    in_port_t       sin_port;    // port, need to be in network byte order
    struct in_addr  sin_addr;    // IPv4 address, also in NBO
  };
  
  struct in_addr {
    uint32_t        s_addr;
  }
  ```
- Special addresses
  - `127.0.0.1`: Loopback (You can see this example when using `localhost` or any web server running on your computer)
  - `0.0.0.0`: Request binding to all network interfaces

### Endians and Network Byte Order

- Big Endian (Network Byte Order): Left to right (ex. 772 in decimal = 03 04)
- Little Endian: Bytes are swapped (ex. 772 in decimal = 04 03)
- Use the library functions `htonl` (32-bit) and `htons` (16-bit) to convert from Little to Big Endian

### `bind`, `accept` and `connect`

```C
int bind(int fd, const struct sockaddr *addr, socklen_t addrlen);
```
  - `sockaddr`: `sockaddr_in` (IPv4), `sockaddr_in6` (IPv6), `sockaddr_un` (UNIX) 
```C
int accept(int fd, struct sockaddr *client_addr, socklen_t addrlen);
```
- Returns new socket cfd for talking to client
- `client_addr` will recieve the address of client

```C
int connect(int fd, const struct sockaddr *server_addr, socklen_t addrlen);
```
- Returns 0 if success, -1 on error
- `fd` can now talk to the server

### Broken Pipes

- If one end of the pipe is closed before the other one, the processes gets `SIGPIPE`
- The default action is the processes gets killed
- Eg.
  ```sh
  $ uniq longFile.txt | head -1 
  # 'head' end of pipe closes after the first line
  # but 'uniq' is still running, hence the process just ends
  ```
- To override the default, set action to `SIG_IGN` (ignore)


