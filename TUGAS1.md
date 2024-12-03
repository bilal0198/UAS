# TCP SOCKET PROGRAMMING SERVER/CLIENT
 ## Step 1: Install Python and Wireshark
  ### 1. Update Ubuntu
    Open the terminal and update your system:
    sudo apt update && sudo apt upgrade -y
  ### 2. Install Python3
    Check if Python3 is installed:
    sudo apt install python3 python3-pip -y
    python3 --version
  ### 3. Install Wireshark
    Install Wireshark using:
    sudo apt install wireshark -y
 ## Step 2: Create the Server and Client Code
  2. Write the Server Code
  ### Create a file named server.py:
    nano server.py
  1. Create a Working Directory
  ### Navigate to your desired location and create a directory:
    mkdir tcp_communication && cd tcp_communication
  2. Write the Server Code
  ### Create a file named server.py:
    nano server.py
  ### Paste the following code:
    import socket

    def start_server():
    host = '127.0.0.1'  # Localhost
    port = 65432        # Port to listen on

    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((host, port))
    server_socket.listen(1)  # Listen for 1 connection
    print(f"Server listening on {host}:{port}")

    conn, addr = server_socket.accept()
    print(f"Connected by {addr}")

    while True:
        data = conn.recv(1024).decode('utf-8')  # Receive up to 1024 bytes
        if not data:
            break
        print(f"Received from client: {data}")
        conn.send("Message received".encode('utf-8'))  # Send acknowledgment

    conn.close()
    server_socket.close()

    if __name__ == "__main__":
    start_server()
 ## 3. Write the Client Code
  ### Create another file named client.py:
     nano client.py
  ### NEXT
     import socket

    def start_client():
    host = '127.0.0.1'  # Server IP
    port = 65432        # Server port

    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect((host, port))

    while True:
        message = input("Enter message (type 'exit' to quit): ")
        if message.lower() == 'exit':
            break

        client_socket.send(message.encode('utf-8'))  # Send message to server
        response = client_socket.recv(1024).decode('utf-8')  # Receive response
        print(f"Received from server: {response}")

    client_socket.close()

    if __name__ == "__main__":
    start_client()
  ## Step 3: Run the Server and Client
### 1. Start the Server
In one terminal, navigate to the project directory and run:

bash
### Copy code
    python3 server.py
2. Start the Client
In another terminal, navigate to the same directory and run:

bash
### Copy code
    python3 client.py
3. Exchange Messages
Send messages from the client.
Observe the messages received by the server.






