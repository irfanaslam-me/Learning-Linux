# Cisco Switch Project Handover - Complete Guide

## Table of Contents
1. [Pre-Handover Checklist](#pre-handover-checklist)
2. [Basic Connectivity Tests](#basic-connectivity-tests)
3. [Interface Verification](#interface-verification)
4. [VLAN Configuration](#vlan-configuration)
5. [Spanning Tree Protocol](#spanning-tree-protocol)
6. [Routing & Gateway Tests](#routing-gateway-tests)
7. [Performance Monitoring](#performance-monitoring)
8. [Security Verification](#security-verification)
9. [High Availability Tests](#high-availability-tests)
10. [Documentation Requirements](#documentation-requirements)

---

## Pre-Handover Checklist

Before the handover meeting, ensure you have:
- [ ] All switch configurations backed up
- [ ] Network topology diagram ready
- [ ] IP address spreadsheet/documentation
- [ ] VLAN assignment list
- [ ] Port mapping documentation
- [ ] Login credentials documented (securely)
- [ ] Change log/configuration history
- [ ] Known issues list (if any)

---

## Basic Connectivity Tests

### 1. Ping Test to Default Gateway
**Command:**
```
ping <gateway-ip>
```

**Example:**
```
ping 192.168.1.1
```

**What it does:** Verifies that the switch can communicate with its default gateway (usually the router). This confirms basic Layer 3 connectivity.

**Expected Result:** You should see replies with minimal packet loss (0%) and reasonable response times (typically <10ms on local networks).

**Sample Output:**
```
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms
```

---

### 2. Ping Test Between VLANs
**Command:**
```
ping <device-ip-in-different-vlan>
```

**What it does:** Tests inter-VLAN routing functionality. If devices on different VLANs can ping each other, it confirms your Layer 3 routing is working.

**When this might fail:** If inter-VLAN routing is not configured, or if there are ACLs blocking traffic between VLANs.

---

### 3. Test Connectivity to End Devices
**Command:**
```
ping <end-device-ip>
```

**What it does:** Verifies that attached devices (PCs, servers, printers, access points) are reachable from the switch.

**Pro Tip:** Test at least one device from each VLAN to ensure comprehensive coverage.

---

## Interface Verification

### 4. Show Interface Status (Quick Overview)
**Command:**
```
show interface status
```

**What it does:** Displays a summary table of all interfaces showing:
- Port name
- Status (connected/notconnect/disabled)
- VLAN assignment
- Duplex setting (full/half/auto)
- Speed (10/100/1000)
- Port type (access/trunk)

**Sample Output:**
```
Port      Name               Status       Vlan       Duplex  Speed Type
Gi0/1     Server-Connection  connected    10         a-full  a-1000 10/100/1000BaseTX
Gi0/2     PC-Finance         connected    20         a-full  a-1000 10/100/1000BaseTX
Gi0/3                        notconnect   1          auto    auto   10/100/1000BaseTX
Gi0/24    Uplink-to-Core     connected    trunk      a-full  a-1000 10/100/1000BaseTX
```

**What to check:**
- All expected ports show "connected"
- Speed and duplex are appropriate (usually auto-negotiated to full duplex)
- VLANs are correctly assigned

---

### 5. Detailed Interface Information
**Command:**
```
show interface <interface-id>
```

**Example:**
```
show interface GigabitEthernet0/1
```

**What it does:** Provides detailed statistics for a specific interface including:
- Hardware details (MAC address, MTU)
- Input/output packets and bytes
- Errors (CRC, runts, giants, collisions)
- Bandwidth utilization
- Last clearing of counters

**Sample Output:**
```
GigabitEthernet0/1 is up, line protocol is up (connected)
  Hardware is Gigabit Ethernet, address is 0026.0b12.3456 (bia 0026.0b12.3456)
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Full-duplex, 1000Mb/s, media type is 10/100/1000BaseTX
  input flow-control is off, output flow-control is unsupported
  Last input 00:00:00, output 00:00:00, output hang never
  5 minute input rate 3000 bits/sec, 4 packets/sec
  5 minute output rate 2000 bits/sec, 3 packets/sec
     1234567 packets input, 987654321 bytes, 0 no buffer
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     2345678 packets output, 1234567890 bytes, 0 underruns
     0 output errors, 0 collisions, 2 interface resets
```

**Key things to explain:**
- **"is up, line protocol is up"**: Interface is physically connected and functioning
- **Input/output errors should be 0 or very low**: High errors indicate cable or hardware problems
- **CRC errors = 0**: CRC errors suggest physical layer issues
- **Full-duplex**: Confirms no duplex mismatch issues

---

### 6. Check Interface Errors
**Command:**
```
show interface counters errors
```

**What it does:** Displays error counters for all interfaces in a table format. This is quicker than checking each interface individually.

**Sample Output:**
```
Port        Align-Err    FCS-Err   Xmit-Err    Rcv-Err  UnderSize OutDiscards
Gi0/1               0          0          0          0          0           0
Gi0/2               0          0          0          0          0           0
Gi0/24              0          0          0          0          0           0
```

**What to look for:**
- All counters should ideally be 0
- Any non-zero values need investigation (could indicate cable issues, speed/duplex mismatches, or hardware problems)

---

## VLAN Configuration

### 7. Show All VLANs
**Command:**
```
show vlan brief
```

**What it does:** Displays all configured VLANs and which ports are assigned to each VLAN.

**Sample Output:**
```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/3, Gi0/4, Gi0/5
10   Servers                          active    Gi0/1, Gi0/6
20   Finance                          active    Gi0/2, Gi0/7
30   HR                               active    Gi0/8, Gi0/9
99   Management                       active    
```

**What to verify:**
- All your VLANs are listed and show "active" status
- Ports are assigned to the correct VLANs
- No ports are accidentally left in VLAN 1 (unless intended)

---

### 8. Detailed VLAN Information
**Command:**
```
show vlan id <vlan-number>
```

**Example:**
```
show vlan id 10
```

**What it does:** Shows detailed information about a specific VLAN including all member ports.

---

### 9. Show Trunk Ports
**Command:**
```
show interface trunk
```

**What it does:** Displays information about all trunk ports (ports that carry multiple VLANs). Shows:
- Which ports are trunking
- Which VLANs are allowed on the trunk
- Which VLANs are active
- Native VLAN configuration

**Sample Output:**
```
Port        Mode             Encapsulation  Status        Native vlan
Gi0/24      on               802.1q         trunking      1

Port        Vlans allowed on trunk
Gi0/24      1-4094

Port        Vlans allowed and active in management domain
Gi0/24      1,10,20,30,99

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/24      1,10,20,30,99
```

**Key points to explain:**
- **Mode**: Should be "on", "desirable", or "auto" depending on your configuration
- **Encapsulation**: Should be 802.1q for modern networks
- **Status**: Should show "trunking"
- **Native VLAN**: Usually VLAN 1, but can be changed for security
- **Vlans allowed**: Shows which VLANs can traverse the trunk

---

## Spanning Tree Protocol

### 10. Show Spanning Tree Summary
**Command:**
```
show spanning-tree summary
```

**What it does:** Provides an overview of STP configuration including:
- Which STP mode is running (PVST+, Rapid-PVST+, MST)
- Root bridge information
- Number of VLANs participating in STP

**Sample Output:**
```
Switch is in rapid-pvst mode
Root bridge for: VLAN0010, VLAN0020
Extended system ID           : enabled
Portfast Default             : disabled
PortFast BPDU Guard Default  : disabled
Portfast BPDU Filter Default : disabled
Loopguard Default            : disabled
EtherChannel misconfig guard : enabled
UplinkFast                   : disabled
BackboneFast                 : disabled
```

**What to verify:**
- Confirm the STP mode matches your design (usually rapid-pvst)
- Check which VLANs this switch is root for (if any)

---

### 11. Detailed Spanning Tree Status
**Command:**
```
show spanning-tree
```

**What it does:** Shows detailed STP information for all VLANs including:
- Root bridge ID and priority
- This switch's bridge ID
- Port roles (Root Port, Designated Port, Alternate Port)
- Port states (Forwarding, Blocking, Listening, Learning)
- Path costs

**Sample Output:**
```
VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    24586
             Address     0026.0b12.1234
             Cost        4
             Port        24 (GigabitEthernet0/24)
             
  Bridge ID  Priority    32778  (priority 32768 sys-id-ext 10)
             Address     0026.0b12.5678
             
Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.1    P2p
Gi0/24              Root FWD 4         128.24   P2p
```

**Key things to point out:**
- **Root Port**: The port with the best path to the root bridge
- **Designated Ports (Desg)**: Forwarding ports on this switch
- **FWD (Forwarding)**: Port is actively forwarding traffic
- **BLK (Blocking)**: Port is blocking to prevent loops

---

## Routing & Gateway Tests

### 12. Show IP Routing Table
**Command:**
```
show ip route
```

**What it does:** Displays the routing table showing how the switch routes traffic between networks. This is essential for Layer 3 switches performing inter-VLAN routing.

**Sample Output:**
```
Gateway of last resort is 10.0.0.1 to network 0.0.0.0

C    10.0.10.0/24 is directly connected, Vlan10
C    10.0.20.0/24 is directly connected, Vlan20
C    10.0.30.0/24 is directly connected, Vlan30
S*   0.0.0.0/0 [1/0] via 10.0.0.1
```

**What to explain:**
- **C (Connected)**: Directly connected networks
- **S (Static)**: Statically configured routes
- **S* (Default route)**: The gateway of last resort for unknown destinations
- Each VLAN should have a corresponding connected route if inter-VLAN routing is configured

---

### 13. Show IP Interface Brief
**Command:**
```
show ip interface brief
```

**What it does:** Shows a summary of all Layer 3 interfaces (SVIs - Switch Virtual Interfaces) with their IP addresses and status.

**Sample Output:**
```
Interface              IP-Address      OK? Method Status                Protocol
Vlan10                 10.0.10.1       YES manual up                    up
Vlan20                 10.0.20.1       YES manual up                    up
Vlan30                 10.0.30.1       YES manual up                    up
Vlan99                 192.168.1.10    YES manual up                    up
```

**What to verify:**
- All VLAN interfaces that should be up show "up/up"
- IP addresses are correct
- Management VLAN has proper IP configured

---

## Performance Monitoring

### 14. Check CPU Utilization
**Command:**
```
show processes cpu
```

**What it does:** Displays CPU utilization statistics including:
- Current CPU usage
- 5-second, 1-minute, and 5-minute averages
- Top processes consuming CPU

**Sample Output:**
```
CPU utilization for five seconds: 5%/0%; one minute: 4%; five minutes: 3%

 PID Runtime(ms)   Invoked      uSecs   5Sec   1Min   5Min TTY Process
   1        4524     12453        363  0.00%  0.00%  0.00%   0 Chunk Manager
   2           8        21        380  0.00%  0.00%  0.00%   0 Load Meter
  11      124356   8745231         14  0.15%  0.12%  0.11%   0 Check heaps
```

**What's healthy:**
- CPU utilization should typically be below 50% during normal operation
- Brief spikes to 70-80% during heavy traffic are normal
- Sustained high CPU (>80%) indicates a problem

---

### 15. Check Memory Utilization
**Command:**
```
show memory
```

**What it does:** Displays memory usage statistics showing free and used memory.

**Sample Output:**
```
                Head    Total(b)     Used(b)     Free(b)   Lowest(b)  Largest(b)
Processor    61C3220   258922436   102431580   156490856   151829364   155789200
      I/O    C000000    33554432    10234564    23319868    22891234    23145600
```

**What to look for:**
- Free memory should be at least 20-30% of total
- If free memory is very low (<10%), the switch may need more RAM or has a memory leak

---

### 16. Show Environment Status
**Command:**
```
show environment
```

**What it does:** Displays environmental information including:
- Temperature sensors
- Fan status
- Power supply status

**Sample Output:**
```
Temperature Value: 35 Degree Celsius
Temperature State: GREEN
Yellow Threshold : 61 Degree Celsius
Red Threshold    : 72 Degree Celsius

Fan  State
---  -----
1    OK
2    OK

Power Supply:
---  -------------
1    OK, Active
2    OK, Standby (if redundant)
```

**What's healthy:**
- Temperature in GREEN state (typically below 60°C)
- All fans showing OK
- All power supplies showing OK

---

## Security Verification

### 17. Show Running Configuration
**Command:**
```
show running-config
```

**What it does:** Displays the current active configuration of the switch. This is comprehensive and shows everything configured.

**Key sections to review:**
- Hostname and domain name
- User accounts and passwords (passwords will be encrypted)
- Interface configurations
- VLAN definitions
- Routing configuration
- Access control lists (ACLs)
- Management access (SSH, telnet, console)

**Shortened version:**
```
show running-config | section interface
show running-config | section vlan
show running-config | section router
```

---

### 18. Compare Running vs Startup Config
**Command:**
```
show running-config
show startup-config
```

**What it does:** 
- `show running-config`: Current active config in RAM
- `show startup-config`: Config that will load after reboot (saved in NVRAM)

**Why this matters:** If they don't match, changes haven't been saved and will be lost on reboot.

**To check if they match:**
```
show archive config differences nvram:startup-config system:running-config
```

**If changes need to be saved:**
```
copy running-config startup-config
```
or
```
write memory
```

---

### 19. Show Port Security (if configured)
**Command:**
```
show port-security
```

**What it does:** Displays port security configuration showing which ports have security enabled and their settings.

**Sample Output:**
```
Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
    Gi0/1              1            1                  0         Shutdown
    Gi0/2              2            1                  0         Restrict
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 2
Max Addresses limit in System (excluding one mac per port) : 8192
```

**Detailed port security info:**
```
show port-security interface <interface-id>
```

**What to explain:**
- **MaxSecureAddr**: Maximum number of MAC addresses allowed
- **CurrentAddr**: Currently learned MAC addresses
- **SecurityViolation**: Number of security violations detected
- **Security Action**: What happens when violated (shutdown, restrict, protect)

---

### 20. Show MAC Address Table
**Command:**
```
show mac address-table
```

**What it does:** Displays the MAC address table showing which MAC addresses are learned on which ports and in which VLANs.

**Sample Output:**
```
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
  10    0026.ab12.3456    DYNAMIC     Gi0/1
  10    0026.ab12.7890    DYNAMIC     Gi0/6
  20    0026.cd34.5678    DYNAMIC     Gi0/2
  20    0026.cd34.9012    DYNAMIC     Gi0/7
 All    0100.0ccc.cccc    STATIC      CPU
```

**What to verify:**
- Expected devices show up with their MAC addresses
- MACs are learned on the correct ports
- No unusual number of MACs on a single port (could indicate a loop or hub)

**To see count per port:**
```
show mac address-table count
```

---

### 21. Show Login Attempts
**Command:**
```
show login
```

**What it does:** Shows failed and successful login attempts, helping identify potential security issues.

---

### 22. Show Access Lists (if configured)
**Command:**
```
show access-lists
```

**What it does:** Displays all configured ACLs and their rules.

**To see ACL applied to interfaces:**
```
show ip interface <interface-id> | include access list
```

---

## High Availability Tests

### 23. Show EtherChannel Summary
**Command:**
```
show etherchannel summary
```

**What it does:** If you have link aggregation configured (EtherChannel/LAG), this shows the status of port channels.

**Sample Output:**
```
Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)         LACP      Gi0/23(P)   Gi0/24(P)
```

**Key indicators:**
- **S = Layer2**, **U = in use**, **P = bundled in port-channel**
- All member ports should show (P) indicating they're actively bundled
- Protocol should be LACP for dynamic negotiation

---

### 24. Show Redundant Power Supply (if applicable)
**Command:**
```
show power inline
```

**What it does:** For PoE switches, shows power consumption per port and total power budget.

---

### 25. Show Stack Status (if stacked switches)
**Command:**
```
show switch
```

**What it does:** For stacked switches, shows which switches are in the stack, their roles (master/member), and stack status.

---

## Additional Useful Commands

### 26. Show Version
**Command:**
```
show version
```

**What it does:** Displays:
- IOS version
- Switch model
- Serial number
- Uptime
- Configuration register
- Flash memory size

**Why it matters:** Confirms the correct IOS version is running and shows how long since last reboot.

---

### 27. Show Inventory
**Command:**
```
show inventory
```

**What it does:** Shows hardware inventory including:
- Chassis
- Power supplies
- Fan trays
- Module serial numbers and part numbers

**Why it matters:** Useful for warranty tracking and hardware documentation.

---

### 28. Show CDP Neighbors
**Command:**
```
show cdp neighbors
```
or for more detail:
```
show cdp neighbors detail
```

**What it does:** Shows directly connected Cisco devices (routers, switches, phones). Displays:
- Neighbor device name
- Local interface
- Remote interface
- Platform (model)
- Capability (Router, Switch, Phone)

**Sample Output:**
```
Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
Core-Switch      Gi 0/24           165             R S I  WS-C3750  Gi 1/0/1
IP-Phone-123     Gi 0/5            142              H P   IP Phone  Port 1
```

**Why it's useful:** Verifies physical connectivity to neighboring devices and confirms cabling.

---

### 29. Show LLDP Neighbors (if using LLDP)
**Command:**
```
show lldp neighbors
```

**What it does:** Similar to CDP but works with non-Cisco devices. Shows connected devices that support LLDP.

---

### 30. Show Logging
**Command:**
```
show logging
```

**What it does:** Displays system log messages showing errors, warnings, and informational messages.

**To see last few entries:**
```
show logging | include %
```

**What to look for:**
- Recent errors or critical messages
- Security-related logs (failed logins, port security violations)
- Interface up/down events

---

### 31. Test Network Connectivity
**Command:**
```
traceroute <destination-ip>
```

**What it does:** Shows the path packets take to reach a destination, hop by hop.

**Example:**
```
traceroute 8.8.8.8
```

**Why it's useful:** Helps verify routing paths and identify where connectivity breaks if there's an issue.

---

### 32. Verify NTP Synchronization
**Command:**
```
show ntp status
```
and
```
show ntp associations
```

**What it does:** Verifies the switch clock is synchronized with NTP servers.

**Why it matters:** Accurate timestamps are critical for:
- Log correlation during troubleshooting
- Security event tracking
- Certificate validation

---

## Documentation Requirements

### Network Topology Diagram
**What to include:**
- Physical diagram showing switch locations
- Logical diagram showing VLANs and IP subnets
- Uplink connections
- Key servers and devices
- Redundant paths (if any)

**Tools you can use:** Microsoft Visio, Draw.io, Lucidchart, or even PowerPoint

---

### IP Address Spreadsheet
**Columns to include:**
- Device name
- Interface/Port
- IP address
- Subnet mask
- VLAN
- Gateway
- Description/Purpose

---

### VLAN Assignment Document
**What to include:**
- VLAN ID
- VLAN name
- IP subnet
- Gateway IP
- Purpose/Department
- Ports assigned

---

### Port Mapping Document
**Columns to include:**
- Switch name
- Port number
- Connected device
- Device type
- VLAN assignment
- Port description
- Speed/Duplex
- Notes

---

### Configuration Backup
**How to backup:**
```
show running-config
```
Copy and save to a text file with timestamp.

Or use:
```
copy running-config tftp:
```

**File naming convention:**
`SwitchName_YYY-MM-DD_running-config.txt`

---

### Change Log
**What to document:**
- Date of change
- Who made the change
- What was changed
- Reason for change
- Rollback plan (if needed)

---

### Known Issues List
**What to include:**
- Description of issue
- Affected devices/ports
- Workaround (if any)
- Planned resolution
- Severity level

---

### Login Credentials Document (Secure!)
**What to include:**
- Console access passwords
- Enable password
- SSH username/password
- VTY line passwords
- SNMP community strings (if used)

**Security note:** Store this in encrypted format or password manager, never in plain text.

---

## Pre-Handover Verification Script

Run these commands in sequence during the handover:

```
! 1. Basic Status
show version
show inventory
show clock

! 2. Interface Status
show interface status
show interface counters errors

! 3. VLAN Verification
show vlan brief
show interface trunk

! 4. Spanning Tree
show spanning-tree summary
show spanning-tree root

! 5. Layer 3 (if applicable)
show ip interface brief
show ip route

! 6. Performance
show processes cpu
show memory
show environment

! 7. Security & Config
show running-config
show startup-config
show mac address-table count

! 8. Connectivity Tests
ping <gateway-ip>
ping <critical-server-ip>

! 9. Neighbor Discovery
show cdp neighbors
show lldp neighbors (if enabled)

! 10. Logs
show logging | tail 20
```

---

## Questions They Might Ask

**1. "What happens if this switch fails?"**
- Explain redundancy measures (stacking, redundant uplinks, etc.)
- Mention RTO/RPO if you have them
- Describe failover behavior

**2. "How do we add a new device/VLAN?"**
- Walk through the configuration steps
- Show them documentation

**3. "What's the recovery procedure if we lose the config?"**
- Explain backup strategy
- Show them how to restore from backup
- Mention startup-config vs running-config

**4. "How do we monitor this switch?"**
- Mention SNMP if configured
- Logging server details
- Management VLAN access

**5. "What maintenance windows have been scheduled?"**
- IOS upgrades
- Security patches
- Hardware refresh plans

**6. "Who do we contact if there's an issue?"**
- Escalation path
- Vendor support details
- Your availability for post-handover support

---

## Final Checklist Before Handover

- [ ] All configurations saved (running-config = startup-config)
- [ ] All tests passing
- [ ] Documentation printed/shared
- [ ] Credentials provided securely
- [ ] Known issues communicated
- [ ] Q&A session scheduled
- [ ] Follow-up support period defined
- [ ] Sign-off form prepared

---

## Tips for a Successful Handover

1. **Be proactive**: Run all tests before the meeting so you know there are no surprises
2. **Have outputs ready**: Pre-capture command outputs to save time
3. **Explain, don't just show**: Help them understand WHY you're running each command
4. **Document everything**: Even things that seem obvious to you
5. **Be honest about limitations**: If something isn't working perfectly, say so
6. **Offer post-handover support**: A transition period is always helpful
7. **Get sign-off in writing**: Protect yourself with formal acceptance

Good luck with your handover! 🎯