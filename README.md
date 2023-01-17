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

# What's Next?
- At this point we might be tempted to set up a desktop | laptop | workstation ASAP so we can test connection to the website 
	- This can be accomplished using the same commands and approach we've used thus far. Meaning it's not a difficult-enough of a task for the purpose of this exercise. We will therefore hold off on this step! 
- The next step will be to set up a VLAN between offices 1 and 3. The decision for this is not completely arbitrary. Notice how these two offices have similar types of computers on their network: desktops, workstations, and laptops. For the purposes of our scenario, let these be geographically separated offices which need to act as one network!
- Before we can worry about VLAN configurations, which is something we'll accomplish on the switches, we must first set up the intermediary routers.

# Office Router Configuration
# OSPF Configuration
## Office 1 Router
- On Office 1 Router: < br/>
`enable` < br/>
`conf t` < br/>
`interface gigabitEthernet 0/1` (interface in non-backbone area) < br/> 
`ip address 10.1.23.2 255.255.255.0` < br/>
`exit`
`interface gigabitEthernet 0/0` (interface in backbone area)
`ip address 10.1.12.2 255.255.255.0`
`exit`
`router ospf 1`
	- We now get the following error: `OSPF process 1 cannot start. There must be at least one "up" IP interface`
	- We can fix this by going into the config GUI panel and toggling on our interfaces
`exit`
`router ospf 1`
`network 10.0.0.0 0.255.255.255 area 23`
`router-id 2.2.2.2`
	- We get a message saying `Reload or use "clear ip ospf process" command, for this to take effect`
	- Let's exit to the user exec terminal, do a `copy running-config startup-config` and then `reload`
## Office 2 Router
- In IOS:
`enable`
`configure terminal`
`interface gigabitEthernet 0/0`
`ip address 10.1.23.3 255.255.255.0`
`exit`
`interface gigabitEthernet0/1`
`ip address 10.1.13.3 255.255.255.0`
`exit`
`interface loopback 0` (We do this to set the Router ID)
`ip address 3.3.3.3 255.255.255.0`
`exit`
`router ospf 1`
`network 10.0.0.0 0.255.255.255 area 23`
`exit`
`exit`
`copy running-config startup-config`
- After saving, once again enable both gigabit ethernet interfaces in the config GUI

## Office 3 Router
`enable`
`conf t`
`interface gigabitEthernet0/0` (internal interface)
`ip address 10.1.4.4 255.255.255.0`
`exit`
`interface gigabitEthernet 0/1`
`ip address 10.1.14.4 255.255.255.0`
`exit`
- Now enable the interfaces in the GUI like before
- Next, we set up ospf on this router, back in IOS again
`exit`
`router ospf 1`
`network 10.0.0.0 0.255.255.255 area 4`
`passive-interface gigabitEthernet0/0` (internal interface)
`exit`
`exit`
`copy running-config startup-config`

## ISP Router 1
- In the GUI, let's first make sure all the gigabitEthernet interfaces are on
- Now in IOS:
`enable`
`config t`

