# TCPSocketServer

## Introduction

TCPSocketServer is a script that creates a TCP server capable of listening for incoming client connections, receiving messages from clients, and sending responses back to them. It uses the `socket` library for network communication and manages multiple clients simultaneously using threading. This server can be used for various applications requiring TCP communication, such as chat applications, data streaming, or remote command execution.

## Features

- **TCP Communication**: Listens for and handles TCP connections.
- **Multi-client Handling**: Supports multiple clients simultaneously using threading.
- **Message Echoing**: Receives messages from clients and sends responses back.

## Prerequisites

- Python 3.6 or higher.

## Modules

- **socket**: Provides low-level networking interface.
- **threading**: Enables concurrent execution for handling multiple clients.

```python
import socket
import threading
```

## Functions

### `handle_client(client_socket)`

Handles communication with a connected client. It receives messages from the client, prints them to the server console, and sends an echo response back to the client. If the client disconnects, it closes the socket.

- **Parameters:**
  - `client_socket` (socket): The client's socket connection.
- **Returns:**
  - `None`

#### Example

```python
def handle_client(client_socket):
    while True:
        # Receive message from the client
        message = client_socket.recv(1024)
        if not message:
            break
        print(f"Received: {message.decode('utf-8')}")
        # Send response back to the client
        client_socket.send(f"Echo: {message.decode('utf-8')}".encode('utf-8'))
    client_socket.close()
```

### `start_server(server_ip, server_port)`

Starts the TCP server, binds it to the specified IP address and port, listens for incoming connections, and spawns a new thread for each connected client.

- **Parameters:**
  - `server_ip` (str): The IP address of the server.
  - `server_port` (int): The port number on which the server listens.
- **Returns:**
  - `None`

#### Example

```python
def start_server(server_ip, server_port):
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind((server_ip, server_port))
    server.listen(5)
    print(f"Server listening on {server_ip}:{server_port}")
    
    while True:
        client_socket, addr = server.accept()
        print(f"Accepted connection from {addr}")
        client_handler = threading.Thread(target=handle_client, args=(client_socket,))
        client_handler.start()
```

## Usage

1. **Set the Server IP and Port**: Modify the `server_ip` and `server_port` variables in the `start_server()` function to specify the server's IP address and port number.

    ```python
    server_ip = '127.0.0.1'
    server_port = 9999
    ```

    **Note**: Make sure to replace `'127.0.0.1'` and `9999` with the actual IP address and port number you want the server to use.

2. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

3. **Connect Clients**: Use a TCP client to connect to the server's IP address and port. The server will echo back any messages sent by the client.

## Example Output

When the server is running, it will print messages indicating its status and interactions with clients:

```plaintext
Server listening on 127.0.0.1:9999
Accepted connection from ('127.0.0.1', 54321)
Received: Hello, Server!
```

## Error Handling

- **Invalid IP/Port**: Ensure that the IP address and port number are valid and available. If the port is already in use, the server will raise an error. Choose a different port or stop the conflicting service.
- **Client Disconnection**: The server handles client disconnections gracefully, closing the client socket when the client disconnects.

## Security Considerations

- **Port Security**: Ensure the chosen port is not used by other services and is secured. Avoid using well-known ports to reduce the risk of conflicts and attacks.
- **Data Handling**: Implement appropriate data validation and handling to prevent malicious attacks. Consider adding authentication and encryption for sensitive data.

## FAQs

**Q: What happens if the port is already in use?**
A: The server will raise an error indicating the port is already in use. Choose a different port or stop the conflicting service.

**Q: Can the server handle multiple clients at once?**
A: Yes, the server uses threading to handle multiple clients simultaneously.

**Q: How does the server respond to client messages?**
A: The server echoes back the received message with an "Echo:" prefix.

## Troubleshooting

- **Port Already in Use**: Ensure the specified port is not occupied by another service. Use a different port if necessary.
- **Module Not Found Errors**: Make sure Python is installed correctly and all necessary modules are available. Install any missing modules using `pip`.

## Example TCP Client

To test the server, you can use a simple TCP client script like the one below:

```python
import socket

def tcp_client(server_ip, server_port):
    client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client.connect((server_ip, server_port))
    client.send("Hello, Server!".encode('utf-8'))
    response = client.recv(4096)
    print(f"Received from server: {response.decode('utf-8')}")
    client.close()

if __name__ == "__main__":
    server_ip = '127.0.0.1'
    server_port = 9999
    tcp_client(server_ip, server_port)
```

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
