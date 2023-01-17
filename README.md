# EIGRP
## ISP Router 1
- In Cisco IOS: <br />
`enable` <br />
`conf t` <br />
`router eigrp 100` <br />
`network  150.1.1.0 0.0.0.255` <br />
- Now exit the EIGRP config in order to set up the interface, also in IOS
`interface gigabitEthernet0/2` <br />
`ip address 150.1.1.50 255.255.255.0` <br />
- Manually turn on interface under the "config" tab

## ISP Router 2
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

## Checking Our Work
- On ISP Router 1 in IOS: < br />
`show running-config`
`show ip eigrp neighbors`
- On ISP Router 2 IOS:
`show ip eigrp neighbors`

## Saving Our Work
- On both routers: < br />
`enable` < br />
`copy running-config startup-config` < br />


# Setting Up Server 2
- On Website 2 Router in IOS: < br/>
`enable` < br/>
`conf t` < br/>
`ip address 200.1.1.10 255.255.255.0` < br/>
- On the Website 2 Server under services:
	- Enable HTTPS
- Same server but under Config -> Global -> Settings
	- Set the Gateway/DNS IPv4 to static with the Default Gateway IP address as 200.1.1.10
- Same server but now under Config -> Interface -> FastEthernet0
	- Set the IP configuration to static with IPv4 address 200.1.1.20
	- The subnet mask should autofill to 255.255.255.0. This is what we want!

