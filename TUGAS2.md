# Step 1: Set Up the Environment
  1. Install Wireshark:
  If you haven't already, install Wireshark:
## Code
    sudo apt update
    sudo apt install wireshark -y
# 2. Install Python and Dependencies:
   You will use Python for creating the client-server model. Ensure that Python is installed:
   1. Create the Server Script (server.py):
## CODE
    import socket

    # Create a TCP/IP socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # Bind the socket to the server address and port
    server_address = ('localhost', 8000)
    server_socket.bind(server_address)
    
    # Listen for incoming connections
    server_socket.listen(1)

    print("Waiting for connection...")
    connection, client_address = server_socket.accept()
    print(f"Connection established with {client_address}")
    
    try:
        while True:
            data = connection.recv(1024)
            if data:
                print(f"Received data: {data.decode()}")
                connection.sendall(data)  # Echo the data back to client
            else:
                break
    finally:
        connection.close()
        server_socket.close()

  2. Create the Client Script (client.py):
python
Copy code
## CODE
    import socket
    
    # Create a TCP/IP socket
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Connect to the server
    server_address = ('localhost', 8000)
    client_socket.connect(server_address)
    
    try:
        message = 'A' * 479  # Simulating the first data send
        print(f"Sending: {message}")
        client_socket.sendall(message.encode())
        
    response = client_socket.recv(1024)
    print(f"Received response: {response.decode()}")
    
    # Sending more data
    message2 = 'B' * 1380
    print(f"Sending: {message2}")
    client_socket.sendall(message2.encode())
    
    response2 = client_socket.recv(1024)
    print(f"Received response: {response2.decode()}")

    finally:
    client_socket.close()
    
# Step 3: Capture Packets Using Wireshark
1. Start Wireshark:
Open Wireshark and start capturing on the relevant network interface (e.g., lo for localhost).
2. Filter HTTP or TCP Traffic:
Use TCP port filter to capture the packets exchanged between the client and server:
### Run the server script:
    python3 server.py
### Run the client script:
    python3 client.py

  ![](https://github.com/bilal0198/UAS/blob/f4b7f20f0b589d75ac251cb575f2fa6f526650a2/README/Three-Way%20Handshake.png)
  # Step 4: Analyze TCP Packets in Wireshark
Three-Way Handshake (Steps 1-3):

SYN: Look for the packet from the client to the server with the SYN flag.
SYN-ACK: The server will respond with a SYN-ACK packet.
ACK: Finally, the client will send an ACK to establish the connection.
You can see this in the TCP Sequence Diagram in Wireshark.

Data Transfer (Steps 4-7):

PSH + ACK: Look for packets where the client sends data. These packets will have the PSH and ACK flags.
ACK: The server will acknowledge the receipt of data with an ACK.
Wireshark will show the sequence numbers and acknowledgment numbers for each data exchange:

The first data packet will have Seq=1, Ack=1.
The server will acknowledge it with Seq=1, Ack=480, confirming receipt.
Connection Termination (Steps 31-34):

FIN + ACK: Look for the packet where the server sends a FIN packet to indicate it wants to close the connection.
ACK: The client confirms with an ACK packet.
FIN + ACK: The client sends a FIN packet to indicate it also wants to terminate the connection.
ACK: The server responds with an ACK to confirm termination.






      

