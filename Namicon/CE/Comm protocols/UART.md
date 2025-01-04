# Universal Asynchronous Receiver/Transmitter
- An asynchronous communication protocol
- There are at least 2 connections between the 2 communicating devices: transmission line (*TX*) and reception line (*RX*)
- The TX of device 1 will be connected to the RX of device 2 and the TX of device 2 will be connected to the RX of device 1
![[uart-connections.png]]
- Data is sent **byte-by-byte**

# Data packet
- The sequence of bits that indicates the starting point and ending point of the communication to the receiver
- For UART, the start bit is 0 and end bit is 1. The start and end bit last for 1 clock pulse
![[uart-data-frame.png]]
- The parity bit is optional. If the parity bit is used then the data frame will be 11 bits
- So UART will have 1 start bit, 5 to 9 data bits, 0 or 1 parity bit and 1 stop bit (2 stop bit for older systems) in a single data packet

# RS232 protocol
- Based on UART
- Uses a 9 pin connector
- There's multiple handshaking requirements to communicate the devices' statuses (ready to receive / transmit)

## Voltage level
- 1s are -3V to -25V
- 0s are +3V to +25V

- The voltage difference between the 1s and 0s are so large that data corruption (signals get suppressed) won't destroy the information and the receiver will still be able to read the information
- These high voltage levels are too much for embedded systems. So they are converted to 0 and 5V by a *level shifter* IC. 
![[level-shifter-ic.png]]

# Baud rate
- To get good handshaking in asynchronous comm protocol, baud rate is important. 
- Both the RX and TX need to be configured with the same baud rate for proper handshaking

# Bit banging
- Let's say we transfer character 'A' (0b01000001). We transfer with a baud rate of 1 bit/sec
- The GPIO for UART will initially be high, when it wants to talk, it will go low for 1 clock pulse (1s) to notify other device that it is about to send data. 
- The LSB (least significant bit) will be sent first.
- At the end, the GPIO will go high to stop communication
![[uart-bit-banging.png]]

# UART peripherals
- The transmit register and the receive register are connected to their on shift register on a device
- Both shift registers will be connected with a clock generator
- Transmit register sends the full data stream to shift register (parallel in), then the data will be sent to the other device's shift register, that will be sent up to the receive register (serial out)
![[uart-peripheral-registers.png]]

# Configuration for UART
1. Clocks in UART devices need to be configured with the same baud rate
2. Load data into transmit register 
3. Turn on timer so data will be put in the shift register 
4. Monitor data transmission

# Data monitoring
- Looping: When data needs to be sent, we need to monitor the status bit which tells us whether 8 bits was sent or not (continuous)
- Interrupt: We don't need to monitor the state continuously because when a data is transmitted or received successfully, an interrupt is issued to an ISR (interrupt service routine)
- Interrupt method allows parallelism

# Advantages
- Easy to interface 
- Less hardware, only 2 wires
- No software complication (like addressing slaves)

# Drawbacks
- Synchronizing baud rates
- Ad-hoc comm topology (only 2 devices can be connected)
- No acknowledgement from receiver