# CCNA-LAB-30-DAY-SERIES-02-Multi-vlan
# CCNA Lab Day 2 – Multi-Switch VLAN Network and Trunking

## Overview

This lab builds on Day 1 by extending VLAN communication across multiple switches. You will learn how trunk links allow VLAN traffic to travel between switches and how devices in the same VLAN can communicate even when connected to different switches.

## Objectives

After completing this lab, you will be able to:

- Create VLANs on multiple switches
- Configure access ports
- Configure trunk links
- Verify trunk operation
- Test VLAN communication across switches
- Troubleshoot common trunk issues

## Topology

```text
                 VLAN 10

PC1 -------- SW1 ===== SW2 -------- PC2
Fa0         Fa0/3     Fa0/3         Fa0


                 VLAN 20

PC3 -------- SW1 ===== SW2 -------- PC4
Fa0         Fa0/4     Fa0/4         Fa0

SW1 Fa0/1 ===== SW2 Fa0/1 (Trunk Link)
```

## Devices Used

- 2 Cisco Switches (SW1 and SW2)
- 4 PCs
- Copper Straight-Through Cables

## Connections

| Device | Interface | Connected To |
|----------|----------|----------|
| PC1 | Fa0 | SW1 Fa0/3 |
| PC3 | Fa0 | SW1 Fa0/4 |
| SW1 | Fa0/1 | SW2 Fa0/1 |
| PC2 | Fa0 | SW2 Fa0/3 |
| PC4 | Fa0 | SW2 Fa0/4 |

## IP Addressing

### VLAN 10 (HR)

| Device | IP Address |
|----------|----------|
| PC1 | 192.168.10.10/24 |
| PC2 | 192.168.10.20/24 |

### VLAN 20 (Finance)

| Device | IP Address |
|----------|----------|
| PC3 | 192.168.20.10/24 |
| PC4 | 192.168.20.20/24 |

## Configuration Steps

### Step 1: Create VLANs on SW1

```bash
enable
configure terminal

vlan 10
name HR

vlan 20
name FINANCE
```

### Step 2: Create VLANs on SW2

```bash
enable
configure terminal

vlan 10
name HR

vlan 20
name FINANCE
```

### Step 3: Configure Access Ports on SW1

```bash
interface fa0/3
switchport mode access
switchport access vlan 10

interface fa0/4
switchport mode access
switchport access vlan 20
```

### Step 4: Configure Access Ports on SW2

```bash
interface fa0/3
switchport mode access
switchport access vlan 10

interface fa0/4
switchport mode access
switchport access vlan 20
```

### Step 5: Configure Trunk Port on SW1

```bash
interface fa0/1
switchport mode trunk
```

Verify:

```bash
show interfaces trunk
```

### Step 6: Configure Trunk Port on SW2

```bash
interface fa0/1
switchport mode trunk
```

Verify:

```bash
show interfaces trunk
```

## Verification

### Verify VLANs

```bash
show vlan brief
```

### Verify Trunk Status

```bash
show interfaces trunk
```

### Verify Interface Configuration

```bash
show interfaces fa0/1 switchport
```

## Connectivity Testing

### Test 1: PC1 to PC2

```bash
ping 192.168.10.20
```

Expected Result: Success

### Test 2: PC3 to PC4

```bash
ping 192.168.20.20
```

Expected Result: Success

### Test 3: PC1 to PC3

```bash
ping 192.168.20.10
```

Expected Result: Failed

## Understanding Trunking

A trunk link carries traffic from multiple VLANs between switches.

Without a trunk link:

```text
PC1 ---- SW1    SW2 ---- PC2
```

Traffic cannot cross between switches for the same VLAN.

With a trunk link:

```text
PC1 ---- SW1 ===== SW2 ---- PC2
```

The switches add VLAN tags to frames so VLAN information is preserved while crossing the trunk.

## Data Flow

### PC1 to PC2

```text
PC1
 |
SW1
 |
TRUNK
 |
SW2
 |
PC2
```

The frame is tagged as VLAN 10 on SW1, carried across the trunk, and delivered to PC2 through SW2.

### PC3 to PC4

```text
PC3
 |
SW1
 |
TRUNK
 |
SW2
 |
PC4
```

The frame is tagged as VLAN 20 and forwarded across the trunk.

## Troubleshooting Practice

### Scenario 1: Remove Trunk on SW2

```bash
interface fa0/1
switchport mode access
```

Expected Result:

PC1 cannot communicate with PC2.

Reason:

The inter-switch link is no longer a trunk.

### Scenario 2: Delete VLAN 20 on SW2

```bash
no vlan 20
```

Expected Result:

PC3 cannot communicate with PC4.

Reason:

VLAN 20 does not exist on SW2.

### Scenario 3: Shutdown Trunk Interface

```bash
interface fa0/1
shutdown
```

Expected Result:

All communication between switches stops.

Reason:

The trunk link is down.

## Mini Challenge

Create VLAN 30 called SALES.

Requirements:

- Create VLAN 30 on both switches
- Connect one PC to SW1
- Connect one PC to SW2
- Assign both PCs to VLAN 30
- Use the existing trunk link
- Verify end-to-end connectivity

Suggested IP Addresses:

| Device | IP Address |
|----------|----------|
| PC5 | 192.168.30.10/24 |
| PC6 | 192.168.30.20/24 |

## Skills Learned

- VLAN Configuration on Multiple Switches
- Access Port Configuration
- Trunk Configuration
- VLAN Traffic Flow
- VLAN Tagging Concepts
- Trunk Verification
- Basic Trunk Troubleshooting

## Conclusion

This lab introduced trunking and demonstrated how VLAN traffic can travel between switches. Trunk links are a fundamental part of enterprise networks and are required whenever VLANs need to span multiple switches.

## Next Lab

Day 3 – Advanced Trunking

Topics:

- Native VLAN
- Allowed VLANs
- VLAN Tagging
- VLAN Mismatch Issues
- Trunk Troubleshooting

## Author

Muhammad Kausar

CCNA SRWE Hands-On Lab Series
