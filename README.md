
# ISP Router 1
- In Cisco IOS: <br />
`enable` <br />
`conf t` <br />
`router eigrp 100` <br />
`network  150.1.1.0 0.0.0.255` <br />
- Now exit the EIGRP config in order to set up the interface, also in IOS
`interface gigabitEthernet0/2` <br />
`ip address 150.1.1.50 255.255.255.0` <br />
- Manually turn on interface under the "config" tab

# ISP Router 2
- In Cisco IOS: <br />
`enable` <br /> 
`conf t` <br />
`router eigrp 100` <br />
`network 150.1.1.0 0.0.0.255` <br />
	 - If you mess up this step, just run the same command but with `no` at the beginning
- Same as before, we now need to exit the eigrp config in order to assign an IP address to the physical port, also in IOS: <br />
`interface gigabitEthernet0/0` <br />
`ip address 150.1.1.100 255.255.255.0` <br />
- Manually turn on interface (under config tab) 

# Checking Our Work So Far
- On ISP Router 1 via IOS: < br />
`show running-config`
