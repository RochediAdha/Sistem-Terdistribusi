# Sistem Terdistribusi

# Distributed System Communication Example

This repository demonstrates a simple Python client-server communication model for distributed systems. The server listens for incoming connections and processes messages sent by the client.

## Server Script (`server.py`)

```python
import socket

# Create a TCP/IP socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind the socket to the address and port
server_socket.bind(('localhost', 8080))

# Listen for incoming connections (max 5 queued connections)
server_socket.listen(5)
print("Server is running, waiting for connections...")

while True:
    # Accept a new connection from a client
    client_socket, client_address = server_socket.accept()
    print(f"Connection received from {client_address}")

    # Receive data from the client
    data = client_socket.recv(1024).decode()
    if data:
        print(f"Message from client: {data}")

        # Send a response back to the client
        response = f"Message received: {data}"
        client_socket.send(response.encode())

    # Close the client connection
    client_socket.close()

```
