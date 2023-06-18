# Intradomain Routing

## Introduction

1. Routing
    * This lecture focuses on the network layer and a specific function of the
    network layer: routing within a single administrative domain
2. Intradomain routing algorithms
    * Link-state
    * Distance-vector
3. Protocols
    * OSPF: Open Shortest Path First
    * RIP: Routing Information Protocol
4. Challenges
    * Convergence delay
    * Use routing to steer traffic and avoid congestion

## Routing Algorithms

1. How does a packet know what routers to go through between two endpoints?
    * Forwarding: Transferring a packet from an incoming link to an outgoing
    link within a single router
    * Routing: How routers work together using protocols to determine good paths
    (or routes) over which the packets travel from the source to the destination
    node
    * Intradomain routing: Routers on the same administrative domain
    * Interdomain routing: Routers on different administrative domains
        - Lecture focuses on intradomain routing (Interior Gateway Protocols)
2. Classes of routing algorithms
    * Link-state
    * Distance-vector
    * Use a graph to understand these algorithms
        - Routers are nodes, links between routers are edges
        - Each edge has an associated cost

## Quiz 1

1. In this lecture, we discuss intradomain routing, where all the nodes and
subnets are owned and managed by the same organization. (In contrast,
interdomain routing is about routing between different organizations – such as
between two ISPs.) Before we begin talking about intradomain routing algorithms,
what could the weights on the graph edges represent in these diagrams, when we
are seeking the least-cost path between two nodes?
    * Length of the cable
    * Time delay to traverse the link
    * Monetary cost
    * Link capacity
    * Current load on the link
2. A packet is forwarded when it is moved from a router’s input link to the
appropriate link.  
3. Determine which action is network-wide (i.e. involves multiple routers).
    * Routing
4. Intradomain routing must involves multiple administrative domains.
    * False

## Link-state Routing Algorithm

1. Terminology
    * u: Source node
    * v: Every other node in the network
    * D(v): Cost of the current least cost path from u to v
    * p(v): Previous node along the current least cost path from u to v
    * c(u,v): Cost from u to directly attached neighbor v
    * N': Subset of nodes along the current least-cost path from u to v
2. Link-state routing
    * Use Dijkstra's algorithm

| ![linkstate](images/lesson3_link_state.png) |
|:--:|
| Link-state Routing Algorithm |

## Link-state Routing Algorithm - Example

| ![example](images/lesson3_link_state_example.png) |
|:--:|
| Link-state Routing Algorithm Example |

## Link-state Routing Algorithm - Computational Complexity

1. Algorithm is O(N^2) because we need to search through n nodes the first
iteration, n-1 the second, and so on

## Quiz 2

1. Which of the following statements are true?
    * Node x will execute the same number of iterations that node u did, as the
    number of immediate neighbors has no impact on the number of iterations the
    algorithm requires.
2.  Upon termination of Dijkstra’s algorithm, all nodes in a network are aware
of the entire network topology.  
    * True

## Distance Vector Routing

1. Distance Vector Routing Algorithm
    * Based on Bellman-Ford
    * Iterative
    * Asynchronous
    * Distributed
2. Overview
    * Each node maintains its own distance vector, with the costs to reach every
    other node in the network
    * From time to time, each node sends its own distance vector to its neighbor
    nodes
    * The neighbor nodes use it to update their own distance vectors
    * Each node x updates its own distance vector using the Bellman-Ford
    equation: Dx(y) = minv{c(x,v) + Dv(y)}

| ![dv](images/lesson3_distance_vector.png) |
|:--:|
| Distance-Vector Routing Algorithm |

## Distance Vector Routing Example

| ![dvexample](images/lesson3_distance_vector_example.png) |
|:--:|
| Distance-Vector Routing Algorithm Example |

## Link Cost Changes and Failures in DV - Count to Infinity Problem

1. The iterative nature of Distance Vector Routing means that if one link
greatly increases its weight, it can take many iterations for the new routes to
finish calculating

## Poison Reverse

1. A solution to this problem is for one node to poison a route by setting its
weight to infinity
    * This only works with two nodes, does not generalize to a general count-to-
    infinity problem involving three or more nodes that are not directly
    connected

## Quiz 3

1. Select the words that can be used to describe the distance vector algorithm.
    * Distributed
    * Iterative
    * Asynchronous
2. Determine which of the following can cause the count-to-infinity problem.
    * Routing loops

## Distance Vector Routing Protocol Example: RIP

1. Routing Information Protocol
    * Based on distance vector protocol
    * Periodically exchange RIP response messages between neighbors as opposed
    to distance vectors in the DV protocols
        - Uses hop count as a metric (assumes link cost as 1)
    * Each router maintains a routing table, which contains its own distance
    vector as well as the router's forwarding table
        - Destination subnet, next router, number of hops to destination
        - One row for each subnet in the Autonomous System (AS)
        - Update table as number of hops to destination changes
    * Routers send request and response messages over UDP, using port number 520
        - Layered on top of network-layer IP protocol
        - Implemented as an application-level process
    * If a router does not hear from its neighbor at least once every 180
    seconds, that neighbor is considered to be no longer reachable (broken link)

## Linkstate Routing Protocol Example: OSPF

1. Open Shortest Path First
    * Routing protocol that uses a link-state routing algorithm to find the best
    path between the source and destination router
    * Introduced as an advancement to the RIP protocol, operating in upper-tier
    ISPs
    * Link-state protocol that uses flooding of link-state information and a
    Dijkstra least-cost path algorithm
    * Authentication of messages exchanged between routers, option to use
    multiple same-cost paths, support for hierarchy within a single routing
    domain
2. Hierarchy
    * Can configure an AS into areas where each area runs its own OSPF link-state
    routing algorithm
    * One OSPF area in the AS is configured to be the backbone area that routes
    traffic between other areas in the AS
3. Operation
    * Graph of the entire AS is constructed
    * Each router considers itself the root and computes the shortest-path tree
    to all subnets by running Dijkstra's algorithm locally
    * Link costs are preconfigured by a network administrator
4. Link State Advertisements
    * Every router within a domain that operates on OSPF uses link state
    advertisements (LSAs)
        - Communicates the router's local routing topology to all other local
        routers in the same OSPF area
    * LSA is used for building a database (called the link state database)
    containing all the link states
    * LSAs are typically flooded to every router in the domain
5. Refresh rate for LSAs
    * OSPF typically uses a default period of 30 minutes as a refresh rate
    * If a link comes alive before this refresh period is reached, the routers
    connected to that link ensure LSA flooding

## Processing OSPF Messages in the Router

1. Router hardware
    * Route processor (main processing unit)
    * Interface cards that receive data packets which are forwarded via a
    switching fabric
2. Router processing
    * LS update packets which contain LSAs from a neighboring router reaches the
    current router's OSPF (which is the route processor)
        - First trigger for the route processor
        - As LS updates reach the router, a consistent view of the topology is
        being formed and this information is stored in the link-state database
    * Using this information from the link-state database, the current router
    calculates the shortest path using shortest path first (SPF) algorithm
        - Result is fed to the Forwarding Information Base (FIB)
    * Information in the FIB is used when a data packet arrives at an interface
    card of the router, where the next hop for the packet is decided and is
    forwarded to the outgoing interface card

| ![ospf](images/lesson3_ospf.png) |
|:--:|
| Open Shortest Path First |

## Quiz 4

1. Dijkstra’s algorithm is a global  routing algorithm, which is also referred
to as a link-state algorithm.
2. The Bellman Ford equation is used by the distance vector algorithm.
3. The Routing Information Protocol is an example of
    * A distance vector algorithm
    * An intradomain routing algorithm

## Hot Potato Routing

1. Routers rely on both intradomain and interdomain routing algorithms
    * When determining which egress point to use for interdomain routing, we
    pick the one with lower Interior Gateway Protocol (IGP) cost
        - Called hot potato routing
    * Hot potato routing reduces the network's resource consumption by getting
    the traffic out as soon as possible

## Quiz 5

1. There may be multiple egress points from an administrative domain to an
external destination.
    * True
2. Hot potato routing always selects the egress point that is geographically
closest to the ingress point. 
    * False

## An Example Traffic Engineering Framework

1. Measure
    * Efficient assignment of link weights depends on the real time view of the
    network state
        - Operational routers and links
        - Link capacity and IGP parameters configuration
    * Status of network elements can be obtained using Simple Network
    Management Protocol (SNMP)
    * Network manager requires an estimate of the traffic in the network,
    obtained using one of the following techniques
        - Directly from the SNMP Management Information Buses (MIBs)
        - By combining packet-level measurements at the network edge using the
        information in routing tables
        - Network tomography which involves observing the aggregate load on the
        links along with the routing data
        - Direct observation of the traffic using new packet sampling techniques
2. Model
    * Involves predicted the traffic flow through the network based on the IGP
    configuration
    * Best path is selected by calculating the shortest path between them when
    all the links belong to the same OSPF/IS-IS area
3. Control
    * New link weights are applied on the affected routers by connecting to the
    router using telnet or ssh
        - Results in a transient period in the network while shortest paths are
        recomputed
        - Changing link weights is infrequent
            + Only in scenarios where there is new hardware, equipment failures,
            or changes in traffic demands
