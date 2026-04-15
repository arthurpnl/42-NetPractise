# NetPractice

A network configuration project from the 42 curriculum. The goal is to solve 10 levels of increasingly complex networking puzzles by correctly assigning IP addresses, subnet masks, and routes so that all hosts can communicate.

No coding involved — this one is pure networking logic.

## What it's about

Each level presents a broken network topology. You get a mix of hosts, switches, routers, and interfaces with some fields already filled in and others left blank. Your job is to figure out the missing values so that every connection works.

The concepts you actually need to understand:

- **IP addresses** — how devices identify each other on a network
- **Subnet masks** — how the network figures out which devices can talk directly vs. which ones need a router
- **Routing tables** — how packets know where to go when the destination isn't on the local network

That's it. There's no trick. If you understand these three things deeply, you can solve every level.

## Key concepts (quick rundown)

### IP addresses

An IPv4 address is 4 numbers separated by dots, each between 0 and 255 (e.g., `192.168.1.42`). It's split into two parts: the **network portion** (which network the device belongs to) and the **host portion** (which specific device on that network).

The subnet mask determines where the split happens.

### Subnet masks

A subnet mask like `255.255.255.0` (also written `/24`) means the first 24 bits identify the network and the last 8 bits identify the host.

Two devices can communicate directly (without a router) **only if they share the same network address**. To check: apply the mask to both IPs with a bitwise AND. If the results match, same network. If not, you need routing.

Common masks you'll use:

| CIDR | Mask | Usable hosts |
|------|------|-------------|
| /30 | 255.255.255.252 | 2 |
| /28 | 255.255.255.240 | 14 |
| /26 | 255.255.255.192 | 62 |
| /25 | 255.255.255.128 | 126 |
| /24 | 255.255.255.0 | 254 |

Formula: usable hosts = 2^(32 - prefix) - 2 (you lose one address for the network ID and one for broadcast).

### Routing

When a device wants to reach an IP that's not on its local subnet, it forwards the packet to its **default gateway** (usually a router interface on the same subnet). The router then checks its own routing table to decide where to send it next.

A route of `0.0.0.0/0` (or `default`) means "send everything you don't have a specific route for to this next hop." Most hosts only need this default route pointing at their gateway.

### Reserved addresses

Some ranges you can't use for regular hosts:
- `127.0.0.0/8` — loopback (localhost)
- `0.0.0.0` — not a valid host address
- Network address and broadcast address of any subnet are also off limits

## Tips that saved me time

1. **Start with what's locked.** Read the fixed values first. They constrain everything else.
2. **Check subnet membership before anything else.** Two interfaces connected by a link must be on the same subnet. Period.
3. **Watch out for /30 subnets.** They only allow 2 hosts — perfect for point-to-point router links, but easy to mess up because there's almost no room for error.
4. **Trace the packet path mentally.** Pick a source and destination, then walk the packet hop by hop. Does each device along the way know where to send it? If not, fix the routing table.
5. **Don't overthink it.** The later levels look intimidating, but they're just combinations of the same basic rules.

## How to use

1. Access the NetPractice training interface through the 42 intra
2. Work through levels 1 to 10
3. Export your configuration files once all levels are solved
4. Submit via git as usual

## Resources that helped

- [NetPractice: An Intro to IP Addresses and Subnets](https://www.youtube.com/watch?v=HQUw0CfQWAM&t=1229s) — solid video walkthrough of the core concepts
- [42 NetPractice walkthrough (lpaube)](https://github.com/lpaube/NetPractice) — visual breakdowns of each level
