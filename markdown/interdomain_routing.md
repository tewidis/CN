# Autonomous System Relationships and Interdomain Routing

## Introduction

1. Interdomain routing
    * How does data travel between networks?
    * BGP protocol that provides the glue for interconnectivity across domains
    * Cover different interconnection types based on business relationships
    between networks
    * Learn about increasingly popular infrastructures called Internet Exchange
    Points, which primarily provide interconnection services so that the
    participant networks can directly exchange traffic

## Autonomous Systems and Internet Interconnection

1. Internet as an Ecosystem
    * Today's Internet is a complex ecosystem built of a network of networks
        - Internet Service Providers (ISPs)
            + Tier 1: Global scale ISPs form the backbone network over which
            smaller networks can connect (AT&T)
            + Tier 2: Regional ISPs
            + Tier 3: Access ISPs
        - Internet Exchange Points (IXPs)
            + Interconnection infrastructures that provide the physical
            infrastructure where multiple networks (ISPs and CDNs) can connect
            and exchange traffic locally
        - Content Delivery Networks (CDNs)
            + Networks that content providers create with the goal of having
            greater control of how the content is delivered to the end-users
            while reducing connectivity costs
            + Google and Netflix

| ![isps](images/lesson4_isps.png) |
|:--:|
| Interconnection of ISPs |

2. Competition and Cooperation Among Networks
    * Tier-1 ISPs compete with each other, but also need to cooperate to provide
    global connectivity
        - ISPs deploy multiple interconnection strategies depending on the
        number of customers in their network and the geographical location of
        these networks
    * More interconnection options in the Internet Ecosystem
        - ISPs can connect through Points of Presence, multi-homing, and peering
        - ISPs can connect with an agreement not to charge each other
    * The Internet topology: hierarchical versus flat
        - IXPs and CDNs have caused the structure to begin morphing from
        hierarchical to flat
    * Autonomous Systems
        - ISPs and CDNs may operate as an Autonomous System (AS)
        - AS: Group of routers that operate under the same administrative
        authority
    * Protocols for routing traffic between and within ASes
        - Border routers of ASes use Border Gateway Protocol (BGP) to exchange
        routing information

| ![competition](images/lesson4_competition.png) |
|:--:|
| Competition and Cooperation Among Networks |

## Quiz 1

1. The internet topology has been evolving from a hierarchical structure into a
flat structure.  
2. An Autonomous System is a group of routers that operate under the same
administrative authority.
3. Autonomous Systems implement their own set of policies, make their own
traffic engineering decisions and interconnection strategies, and determine how
traffic leaves and enters the network.
    * True
4. The BGP protocol is used within an AS and focuses on optimizing a path
metric within the network. Examples of BGP protocols are Open Shortest Paths
First (OSPF) and Routing Information Protocol.
    * False

## Autonomous System Business Relationships

1. Provider-Customer relationship
    * Based on a financial settlement that determines how much the customer will
    pay the provider
    * Provider forwards customer's traffic to destinations found in the
    provider's routing table
2. Peering relationship
    * Two ASes share access to a subset of each other's routing tables
    * Agreement holds as long as traffic is not asymmetric
3. How do providers charge customers?
    * Provider ASes have a financial incentive to forward as much of their
    customers' traffic as possible
    * Charge in one of two ways:
        - Based on a fixed price, given that the bandwidth used is within a
        predefined range
        - Based on the bandwidth used. Calculated based on periodic measurements

## BGP Routing Policies: Importing and Exporting Routes

1. Exporting Routes
    * Selective about which routes to export
    * Route filters are rules that determine which routes and AS's router should
    advertise to the routers of neighboring ASes
        - Routes learned from customers
        - Routes learned from providers
        - Routes learned from peers
2. Importing Routes
    * Selective about which routes to import as well; based on which neighboring
    AS advertises them and the type of business relationship established
    * Imported routes are ranked by customer, then peer, then provider routes
        - AS wants to ensure that routes toward its customers do not traverse
        other ASes, unnecessarily generating costs
        - AS uses routes learned from peers since these are usually "free" under
        the peering agreement
        - AS resorts to importing routes learned from providers only when
        necessary for connectivity since these will add to costs

## Quiz 2

1. In a peering relationship, the traffic exchanged between the two peers must
be highly asymmetric so that there is enough incentive for both parties to peer
with each other.
    * False
2. A customer-provider relationship between ASes is based on a financial
settlement, which determines how much the customer will pay the provider. The
provider takes care of connecting the customer network with destinations found
in the provider's routing table. The customer pays regardless of the direction
of the traffic.
    * True
3. There is no incentive for smaller ISPs to peer with each other.
    * False
4. Provider ASes have a financial incentive to forward as much of their
customers’ traffic as possible.  
    * True
5. Select the correct order for an AS to import its routes based on their
incentive.
    * Customers -> Peers -> Providers

## BGP and Design Goals

1. Scalability
    * Manage the complications as the Internet grows (more ASes) while achieving 
    convergence in reasonable timescales and providing loop-free paths
2. Express routing policies
    * Define route attributes that allow ASes to implement policies (which routes
    to import and export) through route filtering and route ranking
3. Allow cooperation among ASes
    * Each AS can make local decisions while keeping these decisions confidential
    from other ASes
4. Security
    * Not an initial design goal, but the increase in size and complexity of the
    Internet demands security measures be implemented
    * Protection and early detection for malicious attacks, misconfiguration,
    and faults
    * Methods to enhance BGP security:
        - Protocols
        - Additional infrastructure
        - Public keys for ASes

## BGP Protocol Basics

1. BGP peers exchange routing information over a semi-permanent TCP port
connection called a BGP session
    * Router sends OPEN message to another router
    * Sending and receiving routers send each other announcements from their
    routing tables
        - Few seconds to several minutes
    * eBGP: External BGP, between routers in different ASes
    * iBGP: Internal BGP, between routers in same ASes
2. After BGP peers establish a session, they can exchange BGP messages to
provide reachability information and enforce routing policies
    * UPDATE messages contain information about the routes that have changed
    since the previous update
        - Announcements: Advertise new routes and updates to existing routes
        - Withdrawals: Inform that a previously announced route is no longer
        available
    * KEEPALIVE messages are exchanged between peers to keep a session going
3. BGP Prefix Reachability
    * Destinations are represented by IP prefixes
        - Prefix represents a subnet or collection of subnets an AS can reach
4. Path Attributes and BGP Routes
    * ASPATH: Each AS is identified by its autonomous system number (ASN)
    * NEXTHOP: Refers to the next-hop router's IP address along the path towards
    the destination
        - Internal routers store IP of border router

## iBGP and eBGP

1. eBGP
    * eBGP routers learn routes to external prefixes and disseminate them to all
    routers within the AS

| ![iebgp](images/lesson4_iebgp.png) |
|:--:|
| Internal versus External BGP |

2. iBGP
    * Dissemination of routes within the AS is done by establishing a full mesh
    of iBGP sessions between the internal routers
        - Each eBGP speaking router has an iBGP session with every other BGP
        router in the AS to send updates about the routes it learns
    * iBGP is not another IGP-like protocol (RIP or OSPF)
        - IGP-like protocols are used to establish paths between the internal
        routers of an AS based on specific costs within the AS
        - iBGP is only used to disseminate external routes within the AS

## Quiz 3

1. What is the difference between iBGP and eBGP?
    * Both flavors (iBGP and eBGP) take care of disseminating external routes.
    An eBGP session is established between two border routers that belong to
    different ASes. An iBGP session is established between routers that belong
    to the same AS. Once a router hears about a route that is learned through
    eBGP, then it disseminates that route to other internal routers in the same
    AS, using iBGP. 
2. What is the difference between iBGP and IGP?
    * IGP-like protocols are used to establish paths between the internal
    routers of an AS based on specific costs within the AS. In contrast, iBGP is
    only used to disseminate external routes within the AS.

## BGP Decision Process: Selecting Routes at a Router

1. Selecting Routes
    * Router receives incoming BGP messages and processes them
    * When a router receives advertisements, it first applies the import
    policies to exclude routes from further consideration
    * Router implements the decision process to select the best routes that
    reflect the policy in place
    * Newly selected routes are installed in the forwarding table
    * Router decides which neighbors to export the route to by applying the
    export policy

| ![router](images/lesson4_router.png) |
|:--:|
| Router |

2. Router's Decision Process
    * If there are multiple route advertisements to the same destination, the
    router compares a pair of routes by going through the list of attributes
    * LocalPref attribute is used to prefer routes learned through a specific
    AS over other ASes
        - Customer: LocalPref 90-99
        - Peer: LocalPref 80-89
        - Provider: LocalPref 70-79
        - Backup Links: LocalPref 60-69

| Step | Attribute                         | Controlled by local or neighbor AS? |
|:----:|:---------------------------------:|:-----------------------------------:|
| 1.   | Highest LocalPref                 | local                               |
| 2.   | Lowest AS path length             | neighbor                            |
| 3.   | Lowest origin type                | neither                             |
| 4.   | Lowest MED                        | neighbor                            |
| 5.   | eBGP-learned over iBGP-learned    | neither                             |
| 6.   | Lowest IGP cost to border router  | local                               |
| 7.   | Lowest router ID (to break ties ) | neither                             |

3. Influencing the Route Decision Using the MED Attribute
    * Multi-Exist Discriminator (MED) value is used by ASes connected by multiple
    links to designate which links are preferred for inbound traffic

## Quiz 4

1. A router within an AS decides which route to export by first applying import
policies to exclude routes entirely from further consideration.
    * True
2. The LocalPref attribute is used to prefer routes learned through a specific
AS over other ASes for outbound traffic.
3. Assume that AS X learns of a route to the same destination a via AS Y and AS
Z. If X prefers to route its traffic through Z due to peering or business, it
can assign a higher LocalPref value to routes it learns from Z, and thus using
LocalPref, AS X can control where traffic exits the AS.
4. The MED (Multi-Exit Discriminator) value is used by ASes connected by
multiple links to designate which of those links are preferred for inbound traffic.
5. Assume that AS X prefers routes advertised to AS Y to go through R1 as
opposed to R2. For AS Y to be influenced to choose R1 to forward traffic to AS X,
R1 must have a lower MED value, assuming that all other attributes are equal.

## Challenges with BGP: Scalability and Misconfigurations

1. Misconfiguration
    * Misconfiguration can result in an excessively large number of updates,
    resulting in route instability, router processor and memory overloading,
    outages, and router failures
    * Can reduce risk by limiting routing table size and number of route changes
        - Filter long, specific prefixes to encourage route aggregation
        - Small ASes can configure default routes
    * Can limit number of routing changes using flap damping
        - AS tracks the number of updates to a specific prefix over a certain
        amount of time
        - If this value reaches a threshold, the AS can suppress that route
        until a later time

## Peering at IXPs

1. What are IXPs?
    * IXPs are physical infrastructures that provide the means for ASes to
    interconnect and directly exchange traffic with one another
        - Network of switches
2. Why have IXPs become increasingly popular, and why are they important to study?
    * IXPs are interconnection hubs handling large traffic volumes
    * An important role in mitigating DDoS attacks
    * "Real-world" infrastructures with a plethora of research opportunities
    * IXPs are active marketplaces and technology innovation hubs
3. What are the steps for an AS to peer at an IXP?
    * Each network incures a one-time cost to establish a circuit from the
    premises to the IXP
    * Monthly charge for using a chosen IXP port, higher port speeds are more
    costly
4. Why do networks choose to peer at IXPs?
    * Keeping local traffic local
    * Lower costs
    * Network performance is improved due to reduced delay
    * Critical players in today's Internet ecosystem incentivize other networks
    to connect at IXPs
5. What services are offered at IXPs?
    * Public peering
    * Private peering
    * Route servers and service level agreements
    * Remote peering through resellers
    * Mobile peering
    * DDoS blackholing
    * Free value-added services

## Quiz 5

1. One of the services offered by IXPs is protection against DDoS attacks.
2. Participation of an AS in an IXP is free.
    * False
3. IXPs handle large volumes of traffic.
    * True
4. One of the reasons why networks choose to peer at IXPs is because critical
players in today’s Internet ecosystem often “incentivize” other networks to
connect at IXPs.
    * True
5. Private peering PIs do not use the IXP’s public peering infrastructure.
    * True
6. IXPs users may use route servers for an additional cost.
    * False

## Peering at IXPs: How Does a Route Server Work?

1. Route servers
    * Used to make peering more manageable (reduce number of bilateral BGP
    sessions)
    * Does the following:
        - Collects and shares routing information from its peers or participants
        of the IXP that connect to the RS
        - Executes its own BGP decision process and re-advertises the resulting
        information to all RS's peer routers
2. How does a route server maintain multi-lateral peering sessions?
    * Routing Information Base (RIB) contains all BGP paths that it receives
    from its peers
    * Import filters are applied to ensure that each member AS only advertises
    routes that it should advertise
    * Export filters are typically triggered by the IXP members themselves to
    restrict the set of other IXP member ASes that receive their routes
    * Step-by-step
        - AS X advertises a prefix p1 to the RS, which is added to the route
        server's RIB specific to AS X
        - Route server uses the peer-specific import fitler to check whether AS
        X is allowed to advertise p1
        - Route server applies the peer-specific export filter to check if AS X
        allows AS Z to receive p1, and if true, adds the route to the AS
        Z-specific RIB
        - Route server advertises p1 to AS Z with AS X as the next hop

| ![server](images/lesson4_route_server.png) |
|:--:|
| Route Server |

## Quiz 6

1. Route Servers keep track of the BGP sessions they maintain with each
participant AS through RIBs.
    * True
2. Import filters are applied to ensure that each member AS only advertises
routes that it should advertise.

## Remote Peering

1. What is remote peering?
    * Peering at the peering point without the necessary physical presence
    * Remote peering provider is an entity that sells access to IXPs through
    their own infrastructure
2. Identifying remote peering
    * Measure RTT between inside the IXP and the IXP peering interface
    * Information about the port capacity
    * Gathering colocation information
    * Multi-IXP router inference
    * Private connectivity with multiple existing AS participants

## BGP Configuration Verification

1. Control of BGP Configuration
    * Complex, two main problems
        - Path visibility: Route destinations are correctly propagated through
        the available links in the network
        - Route validity: Traffic meant for a given destination reaches it
    * Router configuration checker (rcc) is a tool researchers propose that
    detects BGP configuration faults
