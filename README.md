# CCNA Packet Tracer Labs

Small labs I put together to practice CCNA switching and routing topics in Cisco Packet Tracer.

**You need Cisco Packet Tracer 9.0 or newer.** Every file was saved with version 9.0.0, and older versions cannot open them.

## Labs

| File | Topic |
| --- | --- |
| [Port-Security.pkt](Port-Security.pkt) | Port security, voice VLAN, DHCP and PAT |
| [L3-Switch-VLAN-Connection.pkt](L3-Switch-VLAN-Connection.pkt) | VLANs routed by one Layer 3 switch (SVIs) |
| [Legacy-Inter-VLAN-Routing.pkt](Legacy-Inter-VLAN-Routing.pkt) | Legacy inter-VLAN routing: one router port per VLAN |
| [Router-on-a-Stick.pkt](Router-on-a-Stick.pkt) | Router-on-a-stick: one trunk, a subinterface per VLAN |
| [Inter-VLAN-Routing.pkt](Inter-VLAN-Routing.pkt) | Inter-VLAN routing with a Layer 3 switch and access switches |
| [DHCP-Practice-L2.pkt](DHCP-Practice-L2.pkt) | A router as DHCP server for two LANs |
| [L3-DHCP-Server.pkt](L3-DHCP-Server.pkt) | A Layer 3 switch as DHCP server for three departments |
| [HSRP-InterVLAN-DHCP.pkt](HSRP-InterVLAN-DHCP.pkt) | HSRP gateways for three VLANs with a DHCP relay |
| [HSRP.pkt](HSRP.pkt) | The same HSRP design, second build |
| [Full-STP.pkt](Full-STP.pkt) | Spanning Tree: loops, root bridge, PortFast, BPDU Guard, Root Guard |

The three inter-VLAN labs (legacy, router-on-a-stick, Layer 3 switch) solve the same problem in the order the methods appeared, with the same departments: SMM, HR and IT.

## What is in each lab

### Port-Security.pkt

- **Devices:** R1 (1941), an "Internet" router, SW1 and SW2 (2960), IP phones and PCs.
- **Configured:** R1 routes VLAN 10 STAFF, 20 FINANCE, 50 VOICE and 99 MGMT (native) on subinterfaces, hands out addresses from three DHCP pools and does PAT towards 203.0.113.0/24. The access ports carry a data VLAN and the voice VLAN and are protected with port security (a maximum, sticky addresses, one static address for the printer). Unused ports sit in VLAN 999 and are shut down.
- **Check:** `show port-security`, `show port-security interface fa0/1`, `show port-security address`.

### L3-Switch-VLAN-Connection.pkt

- **Devices:** one 3560 (L3S1) and six PCs.
- **Configured:** VLAN 2 SMM, 3 HR and 4 IT, an SVI for each (192.168.2.1, 192.168.3.1, 192.168.4.1) and `ip routing`.
- **Check:** `show vlan brief`, `show ip route`, then ping from one VLAN to another.

### Legacy-Inter-VLAN-Routing.pkt

- **Devices:** R1 (1941), S1 and S2 (2960), ten PCs and an admin laptop.
- **Configured:** VLAN 2, 3, 4 and 99. The router has one physical interface in VLAN 2 and one in VLAN 3. With two router ports only those two VLANs get a gateway, which is the reason this method was replaced by router-on-a-stick. SSH is set up on S1.
- **Check:** `show ip interface brief` on R1, `show vlan brief` on S1.

### Router-on-a-Stick.pkt

- **Devices:** R1 (1941), S1 and S2 (2960), ten PCs and an admin laptop.
- **Configured:** the same VLANs, now all on one trunk to R1: subinterfaces Gi0/1.2, .3, .4 and .99 with `encapsulation dot1Q`. A second trunk joins the two switches.
- **Check:** `show interfaces trunk` on S1, `show ip interface brief` on R1, then ping across VLANs.

### Inter-VLAN-Routing.pkt

- **Devices:** one 3560, two 2960 access switches, ten PCs and a laptop.
- **Configured:** SVIs for VLAN 2, 3, 4 and 99 on the 3560 with `ip routing`; trunks to the access switches that allow VLAN 2-4 and 99.
- **Check:** `show interfaces trunk`, `show ip route`.

### DHCP-Practice-L2.pkt

- **Devices:** R1 (1941), two 2960 switches, PCs, an IP phone, a printer and a file server.
- **Configured:** R1 is the DHCP server for both LANs: pool SMM (192.168.1.0/24) on Gi0/0 and pool HR (10.10.10.0/24) on Gi0/1, each with excluded addresses.
- **Check:** `show ip dhcp pool`, `show ip dhcp binding`; on a PC switch IP Configuration to DHCP.

### L3-DHCP-Server.pkt

- **Devices:** one 3560 and one 2960 per department (SMM, HR, IT) with PCs.
- **Configured:** three routed ports on the 3560 (`no switchport`): 192.168.1.1, 10.10.10.1 and 172.16.1.1, and a DHCP pool for each network.
- **Check:** `show ip dhcp binding`, `show ip interface brief`.

### HSRP-InterVLAN-DHCP.pkt and HSRP.pkt

- **Devices:** R1 and R2, each with its own ISP router, one 2960, a DHCP server and six PCs.
- **Configured:** both routers are gateways for VLAN 10 SMM, 20 HR and 30 IT on subinterfaces. HSRP gives each VLAN one virtual gateway address (192.168.10.1, 192.168.20.1, 192.168.30.1), and `ip helper-address` relays DHCP requests to the server at 192.168.1.2.
- **The difference:** `HSRP-InterVLAN-DHCP.pkt` uses 2911 routers and the uplinks 100.0.0.0/30 and 200.0.0.0/30. `HSRP.pkt` is a second build of the same design with 1941 routers and other ISP addresses.
- **Check:** `show standby brief`; shut down the active router's LAN interface and watch the other one take over.

### Full-STP.pkt

Five small scenarios on one canvas:

- two switches joined by two links, and a triangle of three switches, both with default STP: find the root bridge and the blocked port;
- an access port with PortFast and BPDU Guard;
- two switches with STP switched off (`no spanning-tree vlan 1`) to see what a loop does;
- a triangle with VLAN 2-5 where one switch is made root (`priority 0`), the other two use Root Guard, and a rogue switch tries to take over as root.

**Check:** `show spanning-tree`, `show spanning-tree summary`.

## Opening the labs

In Cisco Packet Tracer, go to File → Open and select the .pkt file you want.

The labs are meant for practice. If you want to check or change the topology and configuration, work on a copy of the file.
