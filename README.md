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


<hr> <br>

# Here's an example code that demonstrates the use of multi-threading, event-driven programming, and encryption in Python socket programming:
```ruby
import socket
import threading
import select
import ssl

# Server settings
HOST = 'localhost'
PORT = 8000
BACKLOG = 5

# Enable encryption
USE_TLS = True

class ChatServer:
    def __init__(self):
        self.host = HOST
        self.port = PORT
        self.backlog = BACKLOG
        self.clients = []
        self.running = True
        self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.server_socket.bind((self.host, self.port))
        self.server_socket.listen(self.backlog)
        self.ssl_context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
        self.ssl_context.check_hostname = False
        self.ssl_context.load_cert_chain(certfile="server.crt", keyfile="server.key")
        self.ssl_clients = {}
        print(f"Chat server started on {self.host}:{self.port}...")

    def broadcast(self, message, sender=None):
        for client in self.clients:
            if client != sender:
                client.send(message)

    def handle_client(self, client):
        while self.running:
            try:
                message = client.recv(1024)
                if message:
                    print(f"Received message: {message.decode('utf-8')}")
                    self.broadcast(message, sender=client)
                else:
                    client.close()
                    self.clients.remove(client)
                    print(f"Client {client.getpeername()} disconnected.")
                    break
            except:
                continue

    def handle_ssl_client(self, client):
        while self.running:
            try:
                message = client.recv(1024)
                if message:
                    print(f"Received encrypted message: {message.decode('utf-8')}")
                    self.broadcast(message, sender=client)
                else:
                    client.close()
                    self.clients.remove(client)
                    print(f"Client {client.getpeername()} disconnected.")
                    break
            except:
                continue

    def start(self):
        inputs = [self.server_socket]
        outputs = []
        while self.running:
            try:
                readable, writable, exceptional = select.select(inputs, outputs, inputs)
                for s in readable:
                    if s is self.server_socket:
                        client_socket, address = self.server_socket.accept()
                        print(f"Client {address} connected.")
                        if USE_TLS:
                            ssl_client = self.ssl_context.wrap_socket(client_socket, server_side=True)
                            self.clients.append(ssl_client)
                            self.ssl_clients[ssl_client] = threading.Thread(target=self.handle_ssl_client, args=(ssl_client,))
                            self.ssl_clients[ssl_client].start()
                        else:
                            self.clients.append(client_socket)
                            client_thread = threading.Thread(target=self.handle_client, args=(client_socket,))
                            client_thread.start()
                    else:
                        pass
            except:
                break

        self.server_socket.close()
        for client in self.clients:
            client.close()
        if USE_TLS:
            for thread in self.ssl_clients.values():
                thread.join()
        print("Chat server stopped.")

if __name__ == "__main__":
    chat_server = ChatServer()
    chat_server.start()

```

This code creates a chat server that allows multiple clients to connect and communicate with each other using a chat room. The server uses multi-threading and event-driven programming to handle multiple connections and messages. The server also includes encryption capabilities using the TLS protocol, which can be enabled by setting the USE_TLS flag to True. The handle_client and handle_ssl_client methods handle theprocessing of incoming messages from clients, and the broadcast method sends messages to all connected clients except for the sender.

To start the chat server, simply run the script:

```ruby
python chat_server.py
```
The server will listen on the specified host and port, and wait for clients to connect. Once a client connects, it will be added to the list of clients and can start sending messages to the chat room.

Note that the server.crt and server.key files are required for enabling TLS encryption. These files should be created and stored in the same directory as the chat_server.py script.
