# GVR: Task 2 - Extending the Topology

## Task Overview
In this task, you will extend the existing Mininet topology from a single-router setup (`h1-r1-h2`) to a two-router topology (`h1-r1-r2-h2`). Your objective is to adapt the flow rules for the two routers, ensuring correct packet forwarding between the hosts.

## Current Topology:
# `h1——r1——h2`

## New Topology: 
# `h1——r1——r2——h2`

### Network Configuration

| Device    | Interface  | IP Address       | MAC Address            |
|-----------|-------------|------------------|------------------------|
| **h1**    | `h1-eth0`   | `10.0.1.1/24`    | `00:04:00:00:00:01`    |
| **r1**    | `r1-eth1`   | `10.0.1.254/24`  | `aa:00:00:00:00:01`    |
|           | `r1-eth2`   | `10.0.3.254/24`  | `aa:00:00:00:00:03`    |
| **r2**    | `r2-eth1`   | `10.0.3.253/24`  | `bb:00:00:00:00:01`    |
|           | `r2-eth2`   | `10.0.2.254/24`  | `bb:00:00:00:00:02`    |
| **h2**    | `h2-eth0`   | `10.0.2.1/24`    | `00:04:00:00:00:02`    |

### Key Notes
- **No P4 Code Changes Required:** The provided P4 program for `r1` and `r2` remains the same (simple Layer 3 forwarding).
- **ARP Configuration:** ARP tables and default routes for hosts are pre-configured by the Mininet script.
- **Separate Flow Rules:** Each router (`r1` and `r2`) requires its own set of flow rules.

## Instructions
1. **Modify mininet topology:**
2. **Modify the Flow Rules:**
   - Update the flow rules for both `r1` and `r2` to handle the following routes:
     - `r1`: Forward packets between `h1` (`10.0.1.0/24`) and `r2` (`10.0.3.0/24`).
     - `r2`: Forward packets between `r1` (`10.0.3.0/24`) and `h2` (`10.0.2.0/24`).

## Load flow rules
- simple_switch_CLI --thrift-port <r1_port> < flows/r1_flows.txt
- simple_switch_CLI --thrift-port <r2_port> < flows/r2_flows.txt

## Test
- mininet> h1 ping h2 -c 5

## Debbugin
## Debugging Tips

Here are some useful commands to help troubleshoot and verify your topology:

### 1. **Wireshark (Packet Capture)**

### 2. **ARP Table Inspection**
   - **Command:** `arp -n`
   - **Usage:** Check the ARP table on any Mininet host to ensure proper IP-to-MAC Default gateway resolution.
   - Example:
     ```bash
     mininet> h1 arp -n
     ```

### 3. **Interface Information**
   - **Command:** `ip link`
   - **Usage:** Display the state and configuration of network interfaces for each host or router.
   - Example:
     ```bash
     mininet> r1 ip link
     ```

### 4. **P4 Runtime Client for Monitoring**
   - **Command:** `sudo ./tools/nanomsg_client.py --thrift-port <r1_port or r2_port>`
   - **Usage:** Interact with the P4 runtime to inspect flow tables and rules loaded on each router.
   - Example:
     ```bash
     sudo ./tools/nanomsg_client.py --thrift-port 9090
     ```

These commands will help you inspect network traffic, verify ARP entries, check interface states, and interact directly with the P4 routers.