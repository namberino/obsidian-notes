# Synchronization
- When a device sends data to a system, it needs to notify the receiver that it is sending data then send the data
- The system will need to send an acknowledgement to the device notifying that the system has received all the data

# Synchronous communication protocol
- The devices communicating with each other share the same clock pulses
- The data is shifted out from the output of the transmitter to the input of the receiver
- The transmitter needs to send a notification to the receiver that it is about to send data, then it will send a binary stream
- The receiver send some acknowledgement bits to notify the transmitter that it has received the data. If the transmitter doesn't receive any acknowledgement bits, it will resend the same binary stream
- Example: SPI, I2C

# Asynchronous communication protocol
- No clock synchronization
- This type of communication uses **baud rate**
- **baud rate** is the transmission speed of the data packet sent by the transmitter
- Before communicating, both the transmitter and the receiver needs to be configured with the same baud rate
- Usually, there's 2 bits at the beginning and the end of the data packet for indicating the start and stop of the data stream (start bit and stop bit)
- Example: UART, CAN

# Topology
- Bus topology: All devices are connected through a universal bus, where they can communicate with each other. If 1 device gets disconnected, the other devices will still be able to communicate normally (Example: CAN)
- Star topology: There's 1 master and multiple slaves. Only the master can talk to the slaves. If the master is disconnected, there won't be any communication (Example: SPI)
- Peer to peer topology: There's 2 devices connected and communicate to each other (Example: UART)