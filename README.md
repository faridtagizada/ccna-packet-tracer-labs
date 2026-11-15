# CCNA Packet Tracer Labs

Small labs I put together to practice CCNA switching and routing topics in Cisco Packet Tracer.

**You need Cisco Packet Tracer 9.0 or newer.** The files were saved with version 9.0, and a file saved in a newer version of Packet Tracer does not open in an older one.

## Labs

The labs are grouped by topic, in the order it makes sense to work through them.

| File | Topic |
| --- | --- |
| **Inter-VLAN routing** | |
| [Legacy.pkt](Legacy.pkt) | Legacy inter-VLAN routing: a separate router interface for every VLAN |
| [ROAST.pkt](ROAST.pkt) | Router-on-a-Stick (ROAS): one trunk to the router and a subinterface per VLAN |
| [L3-Switch-VLAN-Connection.pkt](L3-Switch-VLAN-Connection.pkt) | Routing between VLANs on a Layer 3 switch with SVIs |
| [Inter-VLAN-Routing.pkt](Inter-VLAN-Routing.pkt) | A Layer 3 switch that routes for access switches connected over trunks |
| **DHCP** | |
| [DHCP-Practice-L2.pkt](DHCP-Practice-L2.pkt) | A router as the DHCP server |
| [L3-DHCP-Server.pkt](L3-DHCP-Server.pkt) | A Layer 3 switch as the DHCP server |
| **First-hop redundancy** | |
| [HSRP.pkt](HSRP.pkt) | HSRP: two routers share one virtual gateway address |
| [HSRP-InterVLAN-DHCP.pkt](HSRP-InterVLAN-DHCP.pkt) | HSRP together with inter-VLAN routing and DHCP |
| **Layer 2 security and loops** | |
| [Port-Security.pkt](Port-Security.pkt) | Port security on access ports |
| [Full-STP.pkt](Full-STP.pkt) | Spanning Tree Protocol from start to finish ("Full" means the whole topic in one file: root bridge, port roles, PortFast, BPDU Guard, Root Guard) |

The four inter-VLAN labs show the same problem solved in the order the methods appeared: one router interface per VLAN, then router-on-a-stick, then a Layer 3 switch. The file names are the original ones; `ROAST.pkt` is the router-on-a-stick (ROAS) lab.

## How to check each lab

These are the usual verification commands for each topic.

| Lab | Commands | What to look for |
| --- | --- | --- |
| Legacy | `show ip interface brief`, `show vlan brief` | One router interface up for each routed VLAN, access ports in the right VLANs |
| ROAST (ROAS) | `show interfaces trunk`, `show ip interface brief` | The trunk to the router carries the VLANs; one subinterface (`encapsulation dot1Q`) per VLAN |
| L3-Switch-VLAN-Connection | `show vlan brief`, `show ip route` | An SVI per VLAN, and the VLAN networks as connected routes (`ip routing` must be on) |
| Inter-VLAN-Routing | `show interfaces trunk`, `show ip route` | Trunks from the Layer 3 switch to the access switches, connected routes for every VLAN |
| DHCP-Practice-L2, L3-DHCP-Server | `show ip dhcp pool`, `show ip dhcp binding` | A lease for every client; on a PC, set IP Configuration to DHCP and check the address it gets |
| HSRP, HSRP-InterVLAN-DHCP | `show standby brief` | One router Active and one Standby per group; shut down the Active router's LAN interface and the Standby one takes over |
| Port-Security | `show port-security`, `show port-security interface <port>`, `show port-security address` | The maximum, the learned (sticky) addresses and the violation counter |
| Full-STP | `show spanning-tree`, `show spanning-tree summary` | Which switch is the root bridge, which port is blocking, PortFast and the guard features on the right ports |

Ping between the PCs at the end of each lab, so you know the whole path works.

## Opening the labs

In Cisco Packet Tracer, go to File → Open and select the .pkt file you want.

The labs are meant for practice. If you want to change the topology or the configuration, work on a copy of the file.

## License

[MIT](LICENSE)
