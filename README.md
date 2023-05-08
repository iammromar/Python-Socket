# Python Advanced Socket Programming
This repository contains advanced Python scripts for socket programming. Socket programming allows two or more devices to communicate with each other over a network using TCP or UDP protocols. The scripts in this repository demonstrate how to create a server and client using sockets in Python, and also includes advanced features like multi-threading, event-driven programming, and encryption.

# Prerequisites
Before running the scripts, you need to have Python installed on your machine. You can download and install Python from the official website.

# How to Run
To run the scripts, follow these steps:

> Clone the repository to your local machine. <br>
> Open a terminal window and navigate to the directory containing the scripts.<br>
> Run the server script using the command python __server.py__ .<br>
> In another terminal window, run the client script using the command python client.py.<br>

# Available Scripts
## server.py
This script creates a server socket that listens for incoming connections. Once a connection is established, the server sends a welcome message to the client and waits for a response. The server then processes the client's message and responds accordingly. The server also handles multiple connections using multi-threading and event-driven programming.

The server script also includes encryption capabilities using the TLS protocol, which can be enabled by setting the use_tls flag to True. By default, encryption is turned off.

## client.py
This script creates a client socket that connects to the server. Once a connection is established, the client sends a message to the server and waits for a response. The client then processes the server's response and sends another message if needed.

The client script also includes encryption capabilities using the TLS protocol, which can be enabled by setting the use_tls flag to True. By default, encryption is turned off.

# Additional Scripts
## chat_server.py
This script creates a chat server that allows multiple clients to connect and communicate with each other using a chat room. The chat server uses multi-threading and event-driven programming to handle multiple connections and messages. The chat server also includes encryption capabilities using the TLS protocol, which can be enabled by setting the use_tls flag to True.

## chat_client.py
This script creates a chat client that connects to the chat server. Once a connection is established, the client can join a chat room and communicate with other clients in the same room. The chat client also includes encryption capabilities using the TLS protocol, which can be enabled by setting the use_tls flag to True.
