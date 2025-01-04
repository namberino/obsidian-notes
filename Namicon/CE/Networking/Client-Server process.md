## Client sending process:
1. Ask OS for a socket
2. Perform a DNS lookup
3. Connect the socket to IP address on a specific port
4. Send and receive data
5. Close connection

## Server listening process:
1. Ask OS for a socket
2. Bind socket to listening port
3. Listen for incoming connections
4. Accept incoming connections (Accept returns a new socket for each connections to handle multiple clients)
5. Send and receive data
6. Go back and accept another connection