# TCP Context

In VCE, each TCP connection is managed using two structures.

The TCP context (tcpcontext_t) manages multiple TCP connections (conn_t).

Each conn_t corresponds to a single socket.

## Types of TCP Contexts

There are server-specific configuration items (such as the address to bind the socket to), client-specific configuration items (such as the destination IP address), and items common to both. All of these are collectively called the TCP context.

Because there are some differences between server-side and client-side implementations in TCP/IP networking, VCE provides two types of TCP contexts: server context and client context.

If you specify 1 as the first argument to the vce_tcpcontext_create function, it becomes a server; if you specify 0, it becomes a client.

## Server Context

The server context manages the TCP server.

One server context corresponds to one bind-listen server socket and provides network services using multiple connections.
To use the server context, follow these steps:

1. Define protocol processing routines as needed.
2. Call vce_tcpcontext_create with 1 as the first argument.
3. Use the vce_tcpcontext_set_conn_parser function to register a parser that divides input data from the network into records, and the processing routine (callback function) defined in step 1 that is called by that parser.
4. Continuously call vce_heartbeat.
5. When processing is complete, call vce_tcpcontext_cleanup. For servers, this usually runs in an infinite loop without termination.

## Client Context

The client context manages the TCP client. To use the client context, basically follow these steps:

1. Define protocol processing routines as needed.
2. Call vce_tcpcontext_create with 0 as the first argument.
3. Use the vce_tcpcontext_set_conn_parser function to register a parser that divides input data from the network into records, and the processing routine defined in step 1 that is called by that parser.
4. Connect to the server using vce_tcpcontext_connect.
5. Continuously call vce_heartbeat.
6. When processing is complete, call vce_tcpcontext_cleanup.

The server and client are almost the same except for the argument passed to vce_tcpcontext_create.