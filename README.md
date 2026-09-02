# Networking-Basics

# My Guide: Splitting One Network Into Four Subnets

A quick-reference guide for taking a single network and dividing it into multiple smaller subnets (e.g., for wireless, IoT, DMZ, and user segments).

## Why Subnet at All?

Keeping everything, wireless devices, IoT gadgets, servers, and user machines — on one flat network is a security risk. Subnetting lets you split that single network into isolated segments, each with its own address range.

## Step 1: Convert the Subnet Mask to Binary

Take your current subnet mask (e.g., `255.255.255.0`) and write it out in binary. This reveals two things:
- **Network bits (1s):** identify the network
- **Host bits (0s):** available for host addresses

## Step 2: Figure Out How Many Bits You Need to Borrow

The rule: **more networks = more network bits**, and the only place to get them is by borrowing from the host bits.

Use powers of 2 to determine how many bits to borrow:

| Bits Borrowed | Networks Created |
| 1 | 2 |
| 2 | 4 |
| 3 | 8 |
| 4 | 16 |
| 5 | 32 |

**Rule of thumb:** find the smallest power of 2 that is ≥ the number of networks you need. The exponent is the number of bits to borrow.

*Example:* Need 4 networks → 2² = 4 → borrow **2 bits**.
*Example:* Need 17 networks → 2⁴=16 is too small, 2⁵=32 works → borrow **5 bits**.

## Step 3: Flip the Bits

Starting from the leftmost host bit, flip the number of bits you calculated from 0 to 1. This gives you your new subnet mask in binary.

## Step 4: Convert the New Mask Back to Decimal

Convert each octet back to decimal. Add the CIDR notation by counting all network bits (1s) across the whole mask.

*Example:* Borrowing 2 bits from a `/24` network turns the last octet from `00000000` to `11000000` (128 + 64 = 192), giving a mask of `255.255.255.192`, or **/26**.

## Step 5: Find the Increment

The **increment** is the value of the last borrowed (rightmost network) bit. This tells you the size of each new subnet and where each range starts.

*Example:* With 2 bits borrowed in the last octet, the increment is **64**.

## Step 6: Map Out the Subnet Ranges

Starting from your base network address, each subnet spans one increment, and the range includes the "0" address:

| Subnet | Range |
|---|---|
| 1 | 192.168.1.0 – 192.168.1.63 |
| 2 | 192.168.1.64 – 192.168.1.127 |
| 3 | 192.168.1.128 – 192.168.1.191 |
| 4 | 192.168.1.192 – 192.168.1.255 |

Each subnet uses the same mask: `255.255.255.192` (/26).

## Step 7: Calculate Usable Hosts per Subnet

Count the remaining host bits (0s) in your new mask, then apply:

**Usable hosts = 2^(host bits) − 2**

*Example:* 6 host bits remain → 2⁶ = 64, minus 2 (network + broadcast address) = **62 usable hosts** per subnet.

## Quick Recap (The 4-Step Formula)

1. Convert mask to binary.
2. Determine bits to borrow (smallest power of 2 ≥ number of networks needed).
3. Flip those bits to create the new mask.
4. Convert back to decimal/CIDR, find the increment, and map out ranges.

This same process works regardless of address class (A, B, or C) — the math doesn't change, just which octet you're working in.

## Practice - 5 Network

Current
IP                   192.168.1.0 or 192.168.1.0/24
Subnet Mask          255.255.255.0

### Step 1. Convert mask to binary.

Subnet Mask(255.255.255.0)
11111111.11111111.11111111.00000000

### Step 2. Determine bits to borrow (smallest power of 2 ≥ number of networks needed).
| Col 1 | Col 2 | Col 3 | Col 4 | Col 5 | Col 6 | Col 7 | Col 8 |
| --- | --- | --- | --- | --- | --- | --- | --- |
|128 | 64 | 32 | 16 |  8 | 4 | 2 | 1 |
|256 | 128| 64 | 32 | 16 | 8 | 4 | 2 |       
| | |  |  |  | 5 | falls under 8 | so if we count the number of jumps |              
| | |  |  |  | 3 | 2 | 1 |           

**We jumped 3 steps to 8; thus, we hacked 3 bits from HOST Bits**

### Step 3. Flip those bits to create the new mask.

Hacked Bits : 11111111.11111111.11111111.XXX00000

New Subnet Mask : 11111111.11111111.11111111.11100000







