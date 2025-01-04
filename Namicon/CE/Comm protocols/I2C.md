# Inter-intergrated circuits
- Bus topology based, multi-master protocol. Used for comm between various ICs
- The 2 bus wires: SDA (data line) and SCL (Control line/clock line)
	- SDA: Send data to other devices
	- SCL: Synchronization
- More than 1 master can exist
- Clock is controlled by a master
- Only 1 master can access the bus at a time and there's bus arbitration
- A half duplex comm protocol (only 1 line for comm) between slave and master to transfer and receive data
- Each slaves has a slave address. If a master wants to communicate with a certain slave, it will have to call the receiver first

# Modes
- Standard: up to 100 kbps
- Fast: up to 400 kbps
- Fast+: up to 1 mbps
- High speed: up to 3.4 mbps

- I2C mode must be configured to match the required I2C bus speed for a particular slave
- Example:
![[i2c-bus-speed-slaves.png]]
- Here, the mode will be standard as all slaves can support 100 kbps

- Each master and slave devices has an open drain/open collector configuration pin. These are connected to both SDA and SCL
- SDA and SCL are both connected to VDD

![[i2c-open-drain-collector-bus.png]]

- If the buses are high, they are on idle
- When the devices need to talk, the I2C buses will be pulled to ground using the MOSFETs in the master

# Frame structure
