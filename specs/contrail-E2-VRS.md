# 1. Introduction
The Elastic Edge - Virtual Route Server (E2 VRS) is Junipers’ first offering
of a programmable Route Server to the Internet Exchange community. E2 VRS is 
an intent driven SDN controller that instantiates public peering services at 
an Internet Exchange Point. VRS is one of the E2 suites of SDN controllers 
that provide service abstraction by translating high-level user-defined 
abstract data models, into low-level device configuration.

With the rise of SDNs and Intent Based Networking, network operators want 
to specify what they want to do instead of describing how it is to be done. 
E2 is an SDN Controller that instantiates network service instances across 
nodes of a network, whether physical or virtual.  E2 leverages intent-based 
service models to describe the resulting network service instances. E2 
Virtual Route Server brings intent based network services into the Internet 
Exchange Point (IXP) networks, which were traditionally managed by open 
source route servers.

In a typical IXP network, there are several clients and there is a route-
server; the route server establishes external BGP sessions with the clients’
peering routers. To implement a peering service, the route server needs to 
be configured with elaborate BGP policies, which define how to distribute 
routes between clients. These policies can be complex and tedious to 
configure manually, and error prone when many clients are peering with the 
route server.

E2 VRS solves this problem by simplifying and automating the route server 
configuration. E2 VRS provides Peerbook as the user-interface to manage the 
system. Using Peerbook, the IXP operator can express the peering intent using 
a simple intuitive model and leave the onus and complexity of configuring the 
clients and BGP policies to the VRS system.

![Image of Segmentation](images/contrail-E2.png)

# 2. Problem Statement
Design and implement a high level user model for public-peering and low-level
technology model to configure the Junos VRR as a route-server through Contrail.

# 3. Proposed Solution
The data model for this new controller is shown in below picture.

![Image of Segmentation](images/contrail-e2-datamodel.png)

## 3.1 Alternatives considered
Implement SDN without Contrail support
## 3.2 API schema changes
Current API schema is unaffected, except few attributes were added to
physicalRouter, physicalInterface and VirtualNetwork.
Rest of the schema objects are new and applicable to provisioning SDN,
as shown in section 3.

The flavors like P2L, and MP2MP will be covered later. For now, the focus is on
P2P model.


## 3.3 User workflow impact
None at this moment
## 3.4 UI changes
None as Contrail:E2 uses it's own UI, instead of Contrail UI.
## 3.5 Notification impact
None
## 3.6 Block diagram
![Image of Segmentation](images/contrail-e2-architecture.png)

# 4. Implementation
Only two modules in contrail are affected by Contrail:E2

1. Contrail API schema
2. Contrail device-manager

## 4.1 Work-items for Contrail API

1.  Define new data model for service provisioning use case
2.  Define new UVE types for fetching
     - Network configuration
     - Service status

## 4.2 Work-items for Contrail device manager

1.  Add low level configuration support for contrail:E2 services
2.  Add multi-vendor support(specifically Nokia)
3.  Add service-status and network configuration UVEs

# 5. Performance and scaling impact
Contrail:E2 needs support for managing upto 4000 network devices.
Performance of a single contrail controller should process 80-100
API requests/second.

# 6. Upgrade
Upgrade is seamless,no impact. Below is the layout of Contrail:E2 package

![Image of Segmentation](images/contrail-e2-installation.png)

# 7. Deprecations
None

# 8. Dependencies
1. Support from Contrail config node manager team.

# 9. Testing
## 9.1 Unit tests
1. Unit test for schema changes goes into the contrail test code
2. Same applies for device manager changes.
## 9.2 Dev tests
dev-test will follow the similar mechanism as contrail team to test Contrail:E2
## 9.3 System tests
Yet to be added.

# 10. Documentation Impact
None

# 11. References
1. http://www.juniper.net/us/en/local/pdf/whitepapers/2000535-en.pdf
2. https://github.com/Juniper/contrail-controller
