---
title: NETCONF Intergation Guide
category: how-to
author: Lee Cowdrey <lee@cowdrey.net>
creator: Cowdrey Consulting UK
url: https://www.cowdrey.net/
paginate: true
lang: en_GB
date: 2023-05-24
version: 1.0.0
draft: false
keywords:
  - netconf
  - yang
  - bbf
  - xml
  - xgs-pon
---

- [1. Target Audience](#1-target-audience)
- [2. Topology \& Functional Views](#2-topology--functional-views)
  - [2.1. OLT NNI, BNG, PE and P Interconnections](#21-olt-nni-bng-pe-and-p-interconnections)
  - [2.2. `XPON` Transport Scope Interface Model Relationship](#22-xpon-transport-scope-interface-model-relationship)
- [3. Sequence of Configuration](#3-sequence-of-configuration)
  - [3.1. Combined-NE](#31-combined-ne)
  - [3.2. Separated-NE (with vOLT Layer)](#32-separated-ne-with-volt-layer)
- [4. Compliance](#4-compliance)
  - [4.1. NETCONF](#41-netconf)
  - [4.2. PON](#42-pon)
- [5. NETCONF Protocol Access](#5-netconf-protocol-access)
- [6. NETCONF Recommendations](#6-netconf-recommendations)
  - [6.1. Notifications](#61-notifications)
    - [6.1.1. Streams](#611-streams)
    - [6.1.2. `XPON` Available Notifications](#612-xpon-available-notifications)
  - [6.2. Keepalive](#62-keepalive)
  - [6.3. Call Home](#63-call-home)
    - [6.3.1. SSH](#631-ssh)
    - [6.3.2. TLS](#632-tls)
- [7. Example Service Configuration Use Cases](#7-example-service-configuration-use-cases)
  - [7.1. Interfaces](#71-interfaces)
  - [7.2. Classifiers](#72-classifiers)
    - [7.2.1. Traffic Shaping](#721-traffic-shaping)
  - [7.3. Service Flow](#73-service-flow)
  - [7.4. Inband Management](#74-inband-management)
  - [7.5. Point-to-Point Protocol over Ethernet (PPPoE)](#75-point-to-point-protocol-over-ethernet-pppoe)
  - [7.6. Adding of DHCP Relay](#76-adding-of-dhcp-relay)
  - [7.7. Adding Multicast VLAN](#77-adding-multicast-vlan)
  - [7.8. Deleting VLAN](#78-deleting-vlan)
  - [7.9. OLT Restart](#79-olt-restart)
  - [7.10. OLT Software Upgrade](#710-olt-software-upgrade)
  - [7.11. ONU Software Upgrade](#711-onu-software-upgrade)
  - [7.12. NETCONF Call Home](#712-netconf-call-home)
  - [7.13. MAC Forwarder Table](#713-mac-forwarder-table)
  - [7.14. OLT ONUs interfaces \& GEMPORT states](#714-olt-onus-interfaces--gemport-states)
  - [7.15. OLT Port Statistics](#715-olt-port-statistics)
  - [7.16. List of OLT Ethernet Interfaces](#716-list-of-olt-ethernet-interfaces)
  - [7.17. List of ONUs By Name](#717-list-of-onus-by-name)
  - [7.18. OLT Metadata](#718-olt-metadata)
  - [7.19. Persist Running Configuration](#719-persist-running-configuration)
  - [7.20. Configurable Notifications](#720-configurable-notifications)
  - [7.21. Example Full Simple Configuration](#721-example-full-simple-configuration)
  - [7.22. Example Full Complex Configuration](#722-example-full-complex-configuration)
- [8. `XPON` YANG Data Models](#8-xpon-yang-data-models)
  - [8.1. Common, `XPON` \& IETF Model Relationship](#81-common-xpon--ietf-model-relationship)
  - [8.2. bbf-dot1q-cfm-interfaces](#82-bbf-dot1q-cfm-interfaces)
  - [8.3. bbf-dot1q-cfm-interfaces-state](#83-bbf-dot1q-cfm-interfaces-state)
  - [8.4. bbf-dot1q-cfm-l2-forwarding](#84-bbf-dot1q-cfm-l2-forwarding)
  - [8.5. bbf-dot1q-cfm](#85-bbf-dot1q-cfm)
  - [8.6. bbf-equipment-inventory](#86-bbf-equipment-inventory)
  - [8.7. bbf-ethernet-performance-management](#87-bbf-ethernet-performance-management)
  - [8.8. bbf-frame-processing-profiles](#88-bbf-frame-processing-profiles)
  - [8.9. bbf-hardware-cpu](#89-bbf-hardware-cpu)
  - [8.10. bbf-hardware](#810-bbf-hardware)
  - [8.11. bbf-hardware-storage-drives](#811-bbf-hardware-storage-drives)
  - [8.12. bbf-hardware-transceivers](#812-bbf-hardware-transceivers)
  - [8.13. bbf-hardware-transceivers-xpon](#813-bbf-hardware-transceivers-xpon)
  - [8.14. bbf-interfaces-performance-management](#814-bbf-interfaces-performance-management)
  - [8.15. bbf-interfaces-remote-hardware-state](#815-bbf-interfaces-remote-hardware-state)
  - [8.16. bbf-interfaces-statistics-management](#816-bbf-interfaces-statistics-management)
  - [8.17. bbf-interface-usage](#817-bbf-interface-usage)
  - [8.18. bbf-l2-dhcpv4-relay-forwarding](#818-bbf-l2-dhcpv4-relay-forwarding)
  - [8.19. bbf-l2-dhcpv4-relay](#819-bbf-l2-dhcpv4-relay)
  - [8.20. bbf-l2-forwarding-base](#820-bbf-l2-forwarding-base)
  - [8.21. bbf-l2-forwarding-flooding-policies](#821-bbf-l2-forwarding-flooding-policies)
  - [8.22. bbf-l2-forwarding-forwarders](#822-bbf-l2-forwarding-forwarders)
  - [8.23. bbf-l2-forwarding-forwarding-databases](#823-bbf-l2-forwarding-forwarding-databases)
  - [8.24. bbf-l2-forwarding-mac-learning-control](#824-bbf-l2-forwarding-mac-learning-control)
  - [8.25. bbf-l2-forwarding-mac-learning](#825-bbf-l2-forwarding-mac-learning)
  - [8.26. bbf-l2-forwarding](#826-bbf-l2-forwarding)
  - [8.27. bbf-l2-forwarding-shared-fdb](#827-bbf-l2-forwarding-shared-fdb)
  - [8.28. bbf-l2-forwarding-split-horizon-profiles](#828-bbf-l2-forwarding-split-horizon-profiles)
  - [8.29. bbf-l2-terminations](#829-bbf-l2-terminations)
  - [8.30. bbf-ldra](#830-bbf-ldra)
  - [8.31. bbf-link-table](#831-bbf-link-table)
  - [8.32. bbf-mgmd](#832-bbf-mgmd)
  - [8.33. bbf-mgmd-mrd](#833-bbf-mgmd-mrd)
  - [8.34. bbf-network-map](#834-bbf-network-map)
  - [8.35. bbf-olt-vomci-grpc-client](#835-bbf-olt-vomci-grpc-client)
  - [8.36. bbf-olt-vomci-grpc-server](#836-bbf-olt-vomci-grpc-server)
  - [8.37. bbf-olt-vomci](#837-bbf-olt-vomci)
  - [8.38. bbf-olt-vomci-state](#838-bbf-olt-vomci-state)
  - [8.39. bbf-pppoe-intermediate-agent](#839-bbf-pppoe-intermediate-agent)
  - [8.40. bbf-qos-classifiers](#840-bbf-qos-classifiers)
  - [8.41. bbf-qos-composite-filters](#841-bbf-qos-composite-filters)
  - [8.42. bbf-qos-enhanced-scheduling](#842-bbf-qos-enhanced-scheduling)
  - [8.43. bbf-qos-enhanced-scheduling-state](#843-bbf-qos-enhanced-scheduling-state)
  - [8.44. bbf-qos-filters](#844-bbf-qos-filters)
  - [8.45. bbf-qos-policer-envelope-profiles](#845-bbf-qos-policer-envelope-profiles)
  - [8.46. bbf-qos-policies](#846-bbf-qos-policies)
  - [8.47. bbf-qos-policies-state](#847-bbf-qos-policies-state)
  - [8.48. bbf-qos-policies-sub-interface-rewrite](#848-bbf-qos-policies-sub-interface-rewrite)
  - [8.49. bbf-qos-policies-sub-interfaces](#849-bbf-qos-policies-sub-interfaces)
  - [8.50. bbf-qos-policing](#850-bbf-qos-policing)
  - [8.51. bbf-qos-policing-state](#851-bbf-qos-policing-state)
  - [8.52. bbf-qos-rate-control](#852-bbf-qos-rate-control)
  - [8.53. bbf-qos-shaping](#853-bbf-qos-shaping)
  - [8.54. bbf-qos-traffic-mngt](#854-bbf-qos-traffic-mngt)
  - [8.55. bbf-qos-traffic-mngt-state](#855-bbf-qos-traffic-mngt-state)
  - [8.56. bbf-software-management](#856-bbf-software-management)
  - [8.57. bbf-software-management-voice](#857-bbf-software-management-voice)
  - [8.58. bbf-sub-interfaces](#858-bbf-sub-interfaces)
  - [8.59. bbf-sub-interface-tagging](#859-bbf-sub-interface-tagging)
  - [8.60. bbf-subscriber-profiles](#860-bbf-subscriber-profiles)
  - [8.61. bbf-xponani-ani-body](#861-bbf-xponani-ani-body)
  - [8.62. bbf-xponani-base](#862-bbf-xponani-base)
  - [8.63. bbf-xponani](#863-bbf-xponani)
  - [8.64. bbf-xponani-power-management](#864-bbf-xponani-power-management)
  - [8.65. bbf-xponani-v-enet-body](#865-bbf-xponani-v-enet-body)
  - [8.66. bbf-xpon-base](#866-bbf-xpon-base)
  - [8.67. bbf-xpon-channel-group-body](#867-bbf-xpon-channel-group-body)
  - [8.68. bbf-xpon-channel-pair-body](#868-bbf-xpon-channel-pair-body)
  - [8.69. bbf-xpon-channel-partition-body](#869-bbf-xpon-channel-partition-body)
  - [8.70. bbf-xpon-channel-termination-body](#870-bbf-xpon-channel-termination-body)
  - [8.71. bbf-xpongemtcont-base](#871-bbf-xpongemtcont-base)
  - [8.72. bbf-xpongemtcont-gemport-body](#872-bbf-xpongemtcont-gemport-body)
  - [8.73. bbf-xpongemtcont-gemport-performance-management](#873-bbf-xpongemtcont-gemport-performance-management)
  - [8.74. bbf-xpongemtcont](#874-bbf-xpongemtcont)
  - [8.75. bbf-xpongemtcont-qos](#875-bbf-xpongemtcont-qos)
  - [8.76. bbf-xpongemtcont-tcont-body](#876-bbf-xpongemtcont-tcont-body)
  - [8.77. bbf-xpongemtcont-traffic-descriptor-profile-body](#877-bbf-xpongemtcont-traffic-descriptor-profile-body)
  - [8.78. bbf-xpon](#878-bbf-xpon)
  - [8.79. bbf-xpon-multicast-distribution-set-body](#879-bbf-xpon-multicast-distribution-set-body)
  - [8.80. bbf-xpon-multicast-gemport-body](#880-bbf-xpon-multicast-gemport-body)
  - [8.81. bbf-xpon-onu-state](#881-bbf-xpon-onu-state)
  - [8.82. bbf-xpon-performance-management](#882-bbf-xpon-performance-management)
  - [8.83. bbf-xponvani-base](#883-bbf-xponvani-base)
  - [8.84. bbf-xponvani](#884-bbf-xponvani)
  - [8.85. bbf-xponvani-power-management](#885-bbf-xponvani-power-management)
  - [8.86. bbf-xponvani-v-ani-body](#886-bbf-xponvani-v-ani-body)
  - [8.87. bbf-xponvani-v-enet-body](#887-bbf-xponvani-v-enet-body)
  - [8.88. bbf-xpon-wavelength-profile-body](#888-bbf-xpon-wavelength-profile-body)
  - [8.89. iana-ssh-encryption-algs](#889-iana-ssh-encryption-algs)
  - [8.90. iana-ssh-key-exchange-algs](#890-iana-ssh-key-exchange-algs)
  - [8.91. iana-ssh-mac-algs](#891-iana-ssh-mac-algs)
  - [8.92. iana-ssh-public-key-algs](#892-iana-ssh-public-key-algs)
  - [8.93. iana-symmetric-algs](#893-iana-symmetric-algs)
  - [8.94. iana-tls-cipher-suite-algs](#894-iana-tls-cipher-suite-algs)
  - [8.95. ieee802-dot1ax](#895-ieee802-dot1ax)
  - [8.96. ieee802-dot1q-cfm](#896-ieee802-dot1q-cfm)
  - [8.97. ieee802-ethernet-interface](#897-ieee802-ethernet-interface)
  - [8.98. ietf-access-control-list](#898-ietf-access-control-list)
  - [8.99. ietf-alarms](#899-ietf-alarms)
  - [8.100. ietf-alarms-x733](#8100-ietf-alarms-x733)
  - [8.101. ietf-dhcp](#8101-ietf-dhcp)
  - [8.102. ietf-hardware](#8102-ietf-hardware)
  - [8.103. ietf-hardware-state](#8103-ietf-hardware-state)
  - [8.104. ietf-interfaces](#8104-ietf-interfaces)
  - [8.105. ietf-ip](#8105-ietf-ip)
  - [8.106. ietf-keystore](#8106-ietf-keystore)
  - [8.107. ietf-netconf-acm](#8107-ietf-netconf-acm)
  - [8.108. ietf-netconf](#8108-ietf-netconf)
  - [8.109. ietf-netconf-monitoring](#8109-ietf-netconf-monitoring)
  - [8.110. ietf-netconf-nmda](#8110-ietf-netconf-nmda)
  - [8.111. ietf-netconf-notifications](#8111-ietf-netconf-notifications)
  - [8.112. ietf-netconf-partial-lock](#8112-ietf-netconf-partial-lock)
  - [8.113. ietf-netconf-with-defaults](#8113-ietf-netconf-with-defaults)
  - [8.114. ietf-network-bridge](#8114-ietf-network-bridge)
  - [8.115. ietf-network-instance](#8115-ietf-network-instance)
  - [8.116. ietf-network](#8116-ietf-network)
  - [8.117. ietf-network-topology](#8117-ietf-network-topology)
  - [8.118. ietf-ntp](#8118-ietf-ntp)
  - [8.119. ietf-routing](#8119-ietf-routing)
  - [8.120. ietf-subscribed-notifications](#8120-ietf-subscribed-notifications)
  - [8.121. ietf-subscribed-notif-receivers](#8121-ietf-subscribed-notif-receivers)
  - [8.122. ietf-system-aaa](#8122-ietf-system-aaa)
  - [8.123. ietf-system-capabilities](#8123-ietf-system-capabilities)
  - [8.124. ietf-system-dns-resolver](#8124-ietf-system-dns-resolver)
  - [8.125. ietf-system](#8125-ietf-system)
  - [8.126. ietf-system-tacacs-plus](#8126-ietf-system-tacacs-plus)
  - [8.127. ietf-truststore](#8127-ietf-truststore)
  - [8.128. ietf-yang-library](#8128-ietf-yang-library)
  - [8.129. ietf-yang-schema-mount](#8129-ietf-yang-schema-mount)
  - [8.130. nc-notifications](#8130-nc-notifications)
  - [8.131. notifications](#8131-notifications)
- [9. Abbreviations](#9-abbreviations)
- [10. Terminology](#10-terminology)

---

# 1. Target Audience

It is expected Network Engineering IT/Developer resources will undertake any integrations and such resources are proficient in the following areas:

- NETCONF protocol
- YANG
- SSH
- XML
- Broadband Forum PON Specifications

---

# 2. Topology & Functional Views

The following section aims to provide a mapping from the physical OLT (pOLT) of generic XGS-PON OLT and the transport scope interface model contained within the `xPON` YANG.

```text
    ┌─────────┐
    │┌────────┴┐
    └┤ OSS/BSS ├───┐
     └────┬────┘   │
  ┌───┐ ┌─┴──┐   ┌─┴──┐
├─┤ P ├─┤ PE ├───┤ PE │                  PON IP/Ethernet Service
  └─┬─┘ └──┬─┘╲ ╱└─┬──┘    ┌─────────────────────────────────────────────┐
    │      │   ╳   │       ┌─────────────────┐ splitter┌─────────────────┐    ┌────────┐
  ┌─┴─┐ ┌──┴─┐╱ ╲┌─┴──┐    │┌──────┐┌───────┐│    ╱│   │┌───────┐┌──────┐│    │┌──────┐│
├─┤ P ├─┤ PE ├───┤ PE ├─────┤IP/ETH││PON/MAC├────┤ ├────┤PON/MAC││IP/ETH├┼─────┤IP/ETH││
  └───┘ └─┬──┘   └─┬──┘ NNI│└──────┘└───────┘│ANI ╲│ANI│└───────┘└──────┘│UNI │└──────┘│
     ┌────┴────┐   │       │┌────┐           │         │┌────┐           │    └────────┘
     │ Gateway ├───┘       ││SVID│           │         ││CVID│           │
     └────┬────┘           │└────┘           │         │└────┘           │
       ┌──┴──┐             └─────────────────┘         └─────────────────┘
       │ IXP │                            ─► ╱───────╱─╲ ─► GEMPORT
       │www/@│                           ──► │T-CONT │ │ ──► GEMPORT
       └─┬┬┬─┘                            ─► ╲───────╲─╱ ─► GEMPORT
         ...
    └──────────┘└─────────┘└─────────────────┘└────────┘└────────────────┘    └────────┘
         DCN      CIN/AGG          OLT           ODN           ONU                RG
```

- DCN: Data Center Network incorpating Provider Edge (PE), Provider Core (P) networks, and external networks through Internet Exchange Point (IXP) connections
- OSS/BSS: Operational Support Systems and Business Support Systems of the provider
- CIN/AGG: Converged Interconnect Network & Aggregation Network
- OLT: Optical Line Terminal providing numerous PON ports towards the Optical Distribution Network (ODN)
- ONU: Optical Network Unit providing IP/Ethernet termination from the ODN
- RG: Residential Gateway (Customer Premise Equipment) providing subscriber service access

---

## 2.1. OLT NNI, BNG, PE and P Interconnections

```text
      ┬           ┬
    ┌─┴─┐       ┌─┴─┐
  ├─┤ P ├───────┤ P ├─┤
    └─┬─┘       └─┬─┘
  ┌───┴───┐   ┌───┴───┐
  │  PE   │   │  PE   │
  └───┬───┘╲ ╱└───┬───┘
┌-----│-----╳-----│-----┐
│ ┌───┴───┐╱ ╲┌───┴───┐ │
│ │Access │   │ Access│ │
│ │Leaf   │L2X│   Leaf│ │VRRP
└-┤  BNG  ├───┤  BNG  ├-┘
  └───┬───┘   └──┬────┘
      │ NNI (HA) │
   ┌──┴──────────┴──┐ MAC Address Learning
   │┌──────────────┐│
   ││NETCONF Server││
   │└──────────────┘│
   │      OLT       │
   └───────┬────────┘
           │ANI
           │
 splitter ╱ ╲
          ─┬─
           │ANI
      ┌────┴─┐
     ┌┴─────┐│
     │ ONUs ├┘
     └─┬────┘
       │UNI
     ┌─┴┐
     │RG│
     └──┘
```

- OLT NNI in High Availability (HA) to pair of Access Leaf router incorporating the Broadband Network Gateway (BNG)
- BNG in HA using Virtual Router Redundancy Protocol (VRRP) to provide resilient routable path to the Provider Edge (PE)
- Spine/Aggregation network elements form the Provider Edge (PE) and Provider (P) core networks
- OLT: Optical Line Terminal providing numerous PON ports towards the Optical Distribution Network (ODN)
- ONU: Optical Network Unit providing IP/Ethernet termination from the ODN
- RG: Residential Gateway (Customer Premise Equipment) providing subscriber service access

## 2.2. `XPON` Transport Scope Interface Model Relationship

```text
                ┌─────────────────┐       ┌────────────────────────────┐
                │   OLT HW Model  │       │        ONU HW Model        │
                │    ┌────┐PON    │       │    ┌────┐PON    ┌────┐user │
                │    │port│side   │       │    │port│side   │port│PHY  │
                │    └──▲─┘       │       │    └──▲─┘       └──▲─┘UNI  │
                └───────│─────────┘       └───────│────────────│───────┘
                  ┌─────┴─────┐                   │            │
                  │  channel  │                   │            │
                  │termination│                   │            │
                  └─────♦─────┘                   │            │
                        │1..2                     │            │
                  ┌─────┴─────┐                   │            │
      ┌───────────♦  channel  │                   │            │
      │       0..n│    pair   │                   │            │
      │           └─────♦─────┘                   │            │
      │                 │0..n                     │            │
┌─────♦─────┐     ┌─────┴─────┐                   │            │
│  channel  ├─────♦  channel  │                   │            │
│ partition │0..n │   group   │                   │            │
└─────♦─────┘     └───────────┘       link        │            │
      │0..n         ┌──────┐          table    ┌──┴──┐         │
      └─────────────♦ vANI │◄──────────────────│ ANI │         │
                    └──♦───┘                   └──♦──┘         │
                       │0..n                      │0..n        │
┌──────────────────────┴────────────┐ link        │            │
│ ┌───────────┐                     │ table ┌─────┴─────┐      │
│ │ OLT-vENET │◄────────────────────│───────│ ONU-vENET │      │
│ └─────♦─────┘      ┌───────────┐  │       └─────♦─────┘  ┌───┴──────┐
│       │            │ OLT-vENET │◄─│─────────────│────────│ Ethernet │
│       │            └─────♦─────┘  │ link        │        └──────♦───┘
└───────┼──────────────────┼────────┘ table       │               │
        |0..n              |0..n                  |0..n           |0..n
   ┌────│──────┐      ┌────│──────┐          ┌────│──────┐   ┌────│──────┐
  ┌┴────┴─────┐│     ┌┴────┴─────┐│         ┌┴────┴─────┐│  ┌┴────┴─────┐│
  │    sub    ││     │    sub    ││         │    sub    ││  │    sub    ││
  │ interface │┘     │ interface │┘         │ interface │┘  │ interface │┘
  └───────────┘      └───────────┘          └───────────┘   └───────────┘
┌────────────────────────────────────────────────────────────────────────┐
│                         other common YANG models                       │
└────────────────────────────────────────────────────────────────────────┘
```

---

# 3. Sequence of Configuration

The general sequence of enabling an OLT and onboarding one or more ONUs can be summarised as:

```text
                                         ┌────────────────┐
   ┌─────────┐                           │┌──────────────┐│
   │ NETCONF │                           ││NETCONF Server││
   │ Client  │                           │└──────────────┘│
   └────┬────┘                           │      OLT       │
        │                                └───────┬────────┘
        │ [1] Add pOLT interfaces/CoS/QoS etc.   │
      ┌►│───────────────────────────────────────►│
      │ │          [2]<rpc-reply>...</rpc-reply> │
      │ │◄───────────────────────────────────────│
      │ │─┐ [3]repeat as necessary               │
      └─┼─┘                                      │
        │ [4] Add pOLT root structure            │
        │───────────────────────────────────────►│
        │          [5]<rpc-reply>...</rpc-reply> │
        │◄───────────────────────────────────────│
        │ [6] Add ONU management                 │
    ┌──►│───────────────────────────────────────►│
    │   │          [7]<rpc-reply>...</rpc-reply> │
    │   │◄───────────────────────────────────────│
    │   │ [8] Add ONU interfaces                 │
    │ ┌►│───────────────────────────────────────►│
    │ │ │          [9]<rpc-reply>...</rpc-reply> │
    │ │ │◄───────────────────────────────────────│
    │ │ │─┐ [10]repeat as necessary              │
    │ └─┼─┘                                      │
    │   │─┐ [11]repeat as necessary              │
    └───┼─┘                                      │
        ┴                                        ┴
```

---

The XGS-PON OLTs have two deployment modes available that include:

- Combined-NE
- Separated-NE (with vOLT Layer)

## 3.1. Combined-NE

> Managing the OLT and subtending ONUs as one single combined (OLT-ONUs) network element (NE), i.e., having a single NETCONF management interface. The model and its NETCONF management interface are hosted by the OLT. The management interface between the OLT and the ONU physical entities; it can be OMCI or other protocols.

```text
       NETCONF/YANG
    ┌────────│───────┐
    │┌───────▼──────┐│
    ││NETCONF Server││
    ││┌─────────┐   ││
    │││OLT Mngt │   ││
    │││Functions│   ││
    ││└┬────────┘   ││          ┌─────────────────┐
────┤│ │ ┌─────────┐││   OMCI   │┌───────┐┌──────┐│
 NNI││ │ │ONU Mngt ├────────────►│PON/MAC││IP/ETH│├─┤ UNI
    ││ │ │Functions││├──────────┤│       ││      │├──
    ││ │ └─────────┘││ xPON ODN ││       ││      │├──
    │└─│────────────┘│ ◄──────  │└───────┘└──────┘│
    │  │             │          └─────────────────┘
    │  ▼    OLT      │
    └────────────────┘
```

> **Note:** Combined-NE mode can be classified as a *traditional self-contained* network element.

## 3.2. Separated-NE (with vOLT Layer)

> **Note:** Separated-NE mode (with vOLT Layer) - The vOLT layer provides a set of device aggregation YANG models that "put the scarecrow back together."  i.e., the pOLT, ONUs, and all other (477) disaggregated AN, (459) BNG, and other value-added functions are managed and operated here as one logical entity.  This is true regardless of how disaggregated or aggregated each customer chooses to deploy their functions.  

```text
                            NETCONF/YANG
                                  │
    ┌─────────────────────────────▼─────────────────────────────┐
    │                        vOLT Layer                         │
    └────────┬───────────────────────────┬──────────────────────┘
    ┌────────│───────┐          ┌────────│──────────────────────┐
    │┌───────▼──────┐│          │┌───────▼──────┐               │
    ││NETCONF Server││          ││NETCONF Server│               │
    ││┌─────────┐   ││          ││┌─────────┐   │               │
    │││OLT Mngt │   ││          │││ONU Mngt │   │               │
    │││Functions│   ││          │││Functions│   │               │
    ││└┬────────┘   ││          ││└┬────────┘   │               │
    │└─│────────────┘│          │└─│────────────┘               │
────┤  │             │   OMCI   │  │        ┌─────────────────┐ │
 NNI│  │         ┌─────────────────┘        │┌───────┐┌──────┐│ │
    │  │         └──────────────────────────►│PON/MAC││IP/ETH│├─┼┼ UNI
    │  │             ├──────────┼───────────┤│       ││      │├─┼─
    │  │             │ xPON ODN │           ││       ││      │├─┼─
    │  │             │ ◄─────── │           │└───────┘└──────┘│ │
    │  ▼    OLT      │          │           │       ONU       │ │
    └────────────────┘          │           └─────────────────┘ │
                                └───────────────────────────────┘
```

> **Note:** Separated-NE mode can be classified as a *disaggregated* network element.

---

# 4. Compliance

## 4.1. NETCONF

Any NETCONF Client should be compliant with the following RFCs:

| **Document**                                                 | **Title**                                                    | **Source** | **Year** |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------- | -------- |
| [RFC2616](https://datatracker.ietf.org/doc/html/rfc2616)     | Protocol Version 1.1 (HTTP/1.1)                              | IETF       | 1999     |
| [RFC5246](https://www.rfc-editor.org/rfc/rfc5246.html)       | The Transport Layer Security (TLS)  Protocol                 | IETF       | 2008     |
| [RFC5277](https://www.rfc-editor.org/rfc/rfc5277.html)       | NETCONF Event Notifications                                  | IETF       | 2008     |
| [RFC5905](https://www.rfc-editor.org/rfc/rfc5905.html)       | Network Time Protocol                                        | IETF       | 2010     |
| [RFC6020](https://www.rfc-editor.org/rfc/rfc6020.html)       | YANG – A Data Modelling Language for Network Configuration Protocol (NETCONF)  | IETF       | 2010     |
| [RFC6241](https://datatracker.ietf.org/doc/html/rfc6241.htm) | Network Configuration Protocol (NETCONF)                     | IETF       | 2011     |
| [RFC7231](https://datatracker.ietf.org/doc/html/rfc7231)     | Hypertext Transfer Protocol (HTTP/1.1):  Semantics and Content | IETF       | 2014     |
| [RFC7540](https://www.rfc-editor.org/rfc/rfc7540.html)       | Hypertext Transfer Protocol Version 2  (HTTP/2)              | IETF       | 2011     |
| [RFC7895](https://www.rfc-editor.org/rfc/rfc7895.html)       | YANG Module Library                                          | IETF       | 2016     |
| [RFC7950](https://www.rfc-editor.org/rfc/rfc7950.html)       | YANG 1.1 Data Modelling Language                             | IETF       | 2016     |
| [RFC7951](https://www.rfc-editor.org/rfc/rfc7951.html)       | JSON Encoding of Data Modelled with YANG                     | IETF       | 2016     |
| [RFC8071](https://www.rfc-editor.org/rfc/rfc8071.html)       | NETCONF Call Home and RESTCONF Call Home                     | IETF       | 2017     |
| [RFC8259](https://www.rfc-editor.org/rfc/rfc8259.html)       | The JavaScript Object Notation (JSON) Data Interchange Format  | IETF       | 2017     |
| [RFC8342](https://datatracker.ietf.org/doc/html/rfc8342)     | Network Management Datastore Architecture  (NMDA)            | IETF       | 2018     |
| [RFC8525](https://datatracker.ietf.org/doc/html/rfc8525)     | YANG Library                                                 | IETF       | 2019     |
| [RFC8526](https://datatracker.ietf.org/doc/html/rfc8526)     | NETCONF Extensions to Support the Network Management Datastore Architecture  | IETF       | 2019     |
| [RFC8528](https://www.rfc-editor.org/rfc/rfc8528.html)       | YANG Schema Mount [^6]                                       | IETF       | 2019     |
| [RFC8632](https://www.rfc-editor.org/rfc/rfc8632.html)       | A YANG Data Model for Alarm Management                       | IETF       | 2019     |

[^6] Only required to support the OLT with a deployment mode of *Separated-NE (with vOLT Layer)*

## 4.2. PON

Any integration should be undertaken after reviewing the following Broadband Forum Technical Reports

| **Document**                                                 | **Title**                                                    | **Source** | **Year** |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------- | -------- |
| TR-156                                                       | Using GPON Access in the context of TR-101                   | BBF        | 2017     |
| TR-355                                                       | xDSL, gFAST, line test, power, bonding                       | BBF        | 2022     |
| TR-370                                                       | Fixed Access Network Sharing -  Architecture and Nodal Requirements | BBF        | 2020     |
| TR-383                                                       | Common YANG Modules for Access Networks                      | BBF        | 2020     |
| TR-384                                                       | Cloud Central Office Reference Architectural Framework       | BBF        | 2018     |
| TR-385                                                       | ITU-T PON YANG Modules                                       | BBF        | 2020     |
| TR-386                                                       | Fixed Access Network Sharing - Access Network Sharing Interfaces | BBF        | 2019     |
| TR-411                                                       | Definition of interfaces between CloudCO Functional Modules  | BBF        | 2021     |
| TR-413                                                       | SDN Management and Control Interfaces for the CloudCO Network Functions | BBF        | 2018     |
| TR-435                                                       | NETCONF Requirements for Access Nodes and Broadband Access Abstraction | BBF        | 2020     |
| TR-459                                                       | Control and User Plane Separation for a disaggregated BNG   |  BBF        | 2020     |
| WT-435                                                       | NETCONF Requirements for Access Nodes and Broadband Access Abstraction | BBF        | 2020     |
| WT-436                                                       | Access & Home O&M Automation/Intelligence                    | BBF        | 2020     |
| WT-451                                                       | Virtual OMCI Specification                                   | BBF        | 2020     |
| WT-454                                                       | YANG Modules for Network Map & Equipment Inventory           | BBF        | 2020     |

---

# 5. NETCONF Protocol Access

XGS-PON OLTs are operated using the NETCONF protocol over Secure Shell (SSH); this is more simply known as NETCONF-over-SSH. The OLT can be operated through supplied configuration and received notifications by using a compliant NETCONF Client, SDN Controller (SDNc) or management platform.

> **Note:** Within the Broadband Forum CloudCO specifications, this is referred to as an Access SDN Manager & Controller (SDN M&C)

Out-of-band management can be summarised as:

```text
          oss/bss
    ┌────────────────┐
    │┌──────────────┐│
    ││NETCONF Client││
    │└───────┬──────┘│
    └────────│───────┘
      ┌──────┴─────┐
    ├─┤   DCN/CIN  ├─┤
      └──────┬─────┘
             │mngt
    ┌────────│───────┐
    │┌───────▼──────┐│
    ││NETCONF Server││
    ││┌─────────┐   ││
    │││OLT Mngt │   ││
    │││Functions│   ││
    ││└┬────────┘   ││
────┤│ │ ┌─────────┐││
 NNI││ │ │ONU Mngt │││
    ││ │ │Functions│││
    ││ │ └─────────┘││
    │└─│────────────┘│
    │  │             │
    │  ▼    OLT      │
    └────────────────┘
```

Inband management can be summarised as:

```text
        oss/bss        ┌────────────────┐
   ┌────────────────┐  │┌──────────────┐│
   │┌──────────────┐│  ││NETCONF Server││
   ││NETCONF Client││  ││┌─────────┐   ││
   │└──────┬───────┘│  │││OLT Mngt │   ││
   └───────│────────┘  │││Functions│   ││
     ┌─────┴────┐  vlan││└┬────────┘   ││
   ├─┤  DCN/CIN ├───────► │ ┌─────────┐││
     └────────┬─┘   NNI││ │ │ONU Mngt │││
              ┴        ││ │ │Functions│││
                       ││ │ └─────────┘││
                       │└─│────────────┘│
                       │  │             │
                       │  ▼    OLT      │
                       └────────────────┘
```

> **Warning:** during configuration operations of the OLT, additional care must be taken to preserve the management VLAN definition that operates through the NNI; failure to do so will result in the OLT not being reachable.

Irrespective of which variant of band management is in operation [^3]; NETCONF will operate in the same manner.

[^3]: band management is a decision undertaken during OLT installation

---

# 6. NETCONF Recommendations

## 6.1. Notifications

Broadband Forum has by design made XGS-PON rich in YANG notifications. This has the benefit of ensuring that no legacy or conflicting protocols are required to be included on any XGS-PON compliant OLT, and a single compliant NETCONF Client stack is sufficient for operation.

> **Note:** All notifications are modelled in the available `xPON` YANG

### 6.1.1. Streams

The OLT makes use of notification streams to classify available notifications; the list of available streams can be determined via the NETCONF `<get>` RPC:

```xml
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
 <get>
   <filter type="subtree">
     <streams xmlns="urn:ietf:params:xml:ns:yang:ietf-event-notifications"/>
   </filter>
 </get>
</rpc>
```

The OLT will return list of available streams in the NETCONF response:

```xml
<rpc-reply message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
 <data>
   <streams xmlns="urn:ietf:params:xml:ns:yang:ietf-event-notifications">
     <stream>NETCONF</stream>
     <stream>ALARM</stream>
     <stream>CONFIG_CHANGE</stream>
     <stream>STATE_CHANGE</stream>
     <stream>SYSTEM</stream>
     <stream>HA</stream>
   </streams>
 </data>
</rpc-reply>
```

The `NETCONF` stream is considered default and is mandatory as per the NETCONF protocol; there may be other streams as listed below and as such always defer to the content of the received NETCONF reply:

- `ALARM`
- `CONFIG_CHANGE`
- `STATE_CHANGE`
- `SYSTEM`
- `HA`

Below is an example to subscribe to the `ALARM` stream, that receives all major and critical alarms for interfaces `eth0` and `eth1` on OLT `deviceA`:

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="">
     <create-subscription xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
          <stream>ALARM</stream>
          <filter type="subtree">
               <alarm-notification xmlns="urn:ietf:params:xml:ns:yang:ietf-alarms">
                    <resource xmlns:baa-network-manager="urn:bbf:yang:obbaa:network-manager"
                         xmlns:if="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                         baa-network-manager:network-manager/baa-network-manager:managed-devices/baa-network-manager:device[baa-network-manager:name='deviceA']/baa-network-manager:root/if:interfaces/if:interface[if:name='eth0']</resource>
               </alarm-notification>
               <alarm-notification xmlns="urn:ietf:params:xml:ns:yang:ietf-alarms">
                    <resource xmlns:baa-network-manager="urn:bbf:yang:obbaa:network-manager"
                         xmlns:if="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                         baa-network-manager:network-manager/baa-network-manager:managed-devices/baa-network-manager:device[baa-network-manager:name='deviceA']/baa-network-manager:root/if:interfaces/if:interface[if:name='eth1']</resource>
                    <alarm-type-id
                         xmlns:vendor-alarms="urn:vendor:vendor-alarms">vendor-alarms:link-down</alarm-type-id>
               </alarm-notification>
               <alarm-notification xmlns="urn:ietf:params:xml:ns:yang:ietf-alarms">
                    <perceived-severity>major</perceived-severity>
               </alarm-notification>
               <alarm-notification xmlns="urn:ietf:params:xml:ns:yang:ietf-alarms">
                    <perceived-severity>critical</perceived-severity>
               </alarm-notification>
          </filter>
     </create-subscription>
</rpc>
```

> **Note:** it is considered good practice for a SDN M&C to subscribe to all available streams and notifications by first integrating the NETCONF Server reported capabilities during the NETCONF `hello` handshake interaction

Another example; if a subscription to the notifications associated with ONU power cycling had been issued to the OLT, the OLT would when a ONU undertook a power cycle sequence produce the following notifications and publish them to all connected subscribers:

```xml
<netconf-events>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:27:46Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>onu10_2_vAni_slot1_port1_if</name>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <defect-state-change>
            <defect>
              <type xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</type>
              <state>raised</state>
            </defect>
            <last-change>2022-06-02T14:27:46Z</last-change>
          </defect-state-change>
        </v-ani>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:27:48Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>CT_10</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>GPON1C000015</detected-serial-number>
            <last-change>2022-06-02T14:27:48Z</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</onu-presence-state>
            <onu-id>2</onu-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:27:50Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>onu10_2_vAni_slot1_port1_if</name>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <defect-state-change>
            <defect>
              <type xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</type>
              <state>raised</state>
            </defect>
            <last-change>2022-06-02T14:27:50Z</last-change>
          </defect-state-change>
        </v-ani>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:27:52Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>CT_10</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>GPON1C000015</detected-serial-number>
            <last-change>2022-06-02T14:27:52Z</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</onu-presence-state>
            <onu-id>2</onu-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:30:21Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>CT_10</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>GPON1C000015</detected-serial-number>
            <last-change>2022-06-02T14:30:21Z</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</onu-presence-state>
            <onu-id>2</onu-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:30:23Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>onu10_2_vAni_slot1_port1_if</name>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <defect-state-change>
            <defect>
              <type xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</type>
              <state>cleared</state>
            </defect>
            <last-change>2022-06-02T14:30:23Z</last-change>
          </defect-state-change>
        </v-ani>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:30:25Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>onu10_2_vAni_slot1_port1_if</name>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <defect-state-change>
            <defect>
              <type xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</type>
              <state>cleared</state>
            </defect>
            <last-change>2022-06-02T14:30:25Z</last-change>
          </defect-state-change>
        </v-ani>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:30:27Z</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>CT_10</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>GPON1C000015</detected-serial-number>
            <last-change>2022-06-02T14:30:27Z</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</onu-presence-state>
            <onu-id>2</onu-id>
            <detected-registration-id>000000000000000000000000000000000000000000000000000000000000000000000000</detected-registration-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-06-02T14:30:33Z</eventTime>
    <alarm-notification xmlns="urn:ietf:params:xml:ns:yang:ietf-alarms">
      <resource>/hardware/component[name=onu10_2_uni_slot1_port1_if]</resource>
      <alarm-type-qualifier>11:257:0</alarm-type-qualifier>
      <time>2022-06-02T14:30:33+00:00</time>
      <perceived-severity>critical</perceived-severity>
      <alarm-text>LOS</alarm-text>
      <alarm-type-id xmlns:bbf-alt="urn:bbf:yang:bbf-alarm-types">bbf-alt:bbf-alarm-type-id</alarm-type-id>
    </alarm-notification>
  </notification>
</netconf-events>
```

In the example case of unknown ONU being detected by the OLT, any subscriptions to `bbf-xpon-onu` `onu-presence-state-change` notification would receive a notification similar to:

```xml
<notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2022-08-25T11:41:28.994330716+00:00</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
        <interface>
            <name>olt-1-1-1</name>
            <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
                <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
                    <detected-serial-number>ALPHCCE46760</detected-serial-number>
                    <last-change>2022-08-25T11:41:28+00:00</last-change>
                    <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">
                        bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</onu-presence-state>
                </onu-presence-state-change>
            </channel-termination>
        </interface>
    </interfaces-state>
</notification>
```

From this received notification, it would be simple to extract the following so that can be used in ONU enablement via subsequent OLT configuration:

- OLT (based on the identity of the connected OLT itself)
- OLT PON interface port (`<name>`)
- ONU Serial Number (`<detected-serial-number>`)

> **Note:** the ONU hardware make, model, revision is not known at this stage as no vANI has been established to allow for interrogation to occur

### 6.1.2. `XPON` Available Notifications

The `XPON` YANG modules have the following notifications available:

| **YANG Module** | **YANG Notification**                                            | **Comment**                                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| bbf-alarm | onu-presence-state                                 | Raised if ONU presence state is not present and on intended channel termination |
| bbf-hardware | component-reset                                 | Indicates that the component was reset. The reset can have been forced by the client using the action 'reset', or it can be an autonomous reset triggered inside the managed device. |
| bbf-obbaa-netconf-stack| netconf-state-change                 | Generated when the NETCONF server detects that a state data in the `<running>` datastore has changed. |
| bbf-obbaa-network-manager | device-state-change                | The notification triggered when the device state changed     |
| bbf-olt-vomci | remote-endpoint-status-change                  | A notification traceable to a client remote-endpoint and access-point. This notification is sent when a remote-endpoint is connected or disconnected. |
| bbf-software-management | revision-deleted                     | Indicates that a particular software revision was deleted from the list of available revisions. |
| bbf-software-management | revision-aborted                     | Indicates that a particular software revision was deleted from the list of available revisions. |
| bbf-software-management | download-revision-aborted            | Indicates that the download of the specified software revision has been successfully aborted. |
| bbf-software-management | abort-failed                         | Indicates that the system failed to abort an ongoing download activity. |
| bbf-software-management | revision-activated                   | Indicates that the specified revision has successfully activated on the target component. |
| bbf-software-management | revision-committed                   | Indicates that the specified revision has successfully committed on the target component. |
| bbf-software-management | commit-failed                        | indicates that the revision software failed to commit on its target component. |
| bbf-software-management | revision-downloaded                  | Indicates that the specified software revision has been successfully downloaded to the target component. |
| bbf-software-management | download-revision-failed             | Indicates that the specified software revision failed to download to its target component. |
| bbf-voltmf-entity | onu-discovery-result                       | The notification that reports an ONU discovery by the vOLTMF |
| bbf-vomci-function | onu-alignment-result                      | The notification that the vOMCI function has completed an alignment on an ONU |
| bbf-vomci-function | onu-alarm-misalignment                    | The notification that the vOMCI function has completed an alignment on an ONU |
| bbf-vomci-function | untranslated-omci-notification            | The vOMC Message function cannot translate an OMCI Alarm or AVC message into a specific YANG notification |
| bbf-vomci-function | unsupported-version-vomci-function-message | The vOMCI function does not support the version of the message sent by the vOLTMF. |
| bbf-vomci-proxy | remote-endpoint-status-change                |                                                              |
| bbf-xpon-onu-state | onu-presence-state-change                 | A notification traceable to a channel termination which signifies that an Optical Network Unit (ONU) has transitioned into the indicated presence state. This notification considers not only Optical Network Units (ONUs) for which a corresponding virtual Access Network Interface (vANI) is configured in the Optical Line Termination (OLT) but also ONUs for which no corresponding vANI is configured in the OLT. Such notifications are to be generated only when an ONU enters one of the presence states listed in 'notifiable-onu-presence-states' leaf-list. |
| bbf-xponvani-power-management | onu-power-state-change         | A notification traceable to a virtual Access Network Interface (vANI) which signifies that an Optical Network Unit (ONU)  has transitioned into the indicated power management state. |
| ietf-alarms | operator-action                                  | This notification is used to report that an operator acted upon an alarm. |
| ietf-alarms | alarm-notification                               | This notification is used to report a state change for an alarm. The same notification is used for reporting a newly raised alarm, a cleared alarm, or changing the text and/or severity of an existing alarm. |
| ietf-alarms | alarm-inventory-changed                          | This notification is used to report that the list of possible alarms has changed. This can happen when, for example, a new software module is installed or a new physical card is inserted. |
| ietf-hardware-state | hardware-state-change                    | A hardware-state-change notification is generated when the value of /hardware/last-change changes in the operational state. |
| ietf-hardware-state | hardware-state-oper-enabled              | A hardware-state-oper-enabled notification signifies that a component has transitioned into the 'enabled' state. |
| ietf-hardware-state | hardware-state-oper-disabled             | A hardware-state-oper-disabled notification signifies that a component has transitioned into the 'disabled' state. |
| ietf-hardware | hardware-state-change                          | A hardware-state-change notification is generated when the value of /hardware/last-change changes in the operational state. |
| ietf-hardware | hardware-state-oper-enabled                    | A hardware-state-oper-enabled notification signifies that a component has transitioned into the 'enabled' state. |
| ietf-hardware | hardware-state-oper-disabled                   | A hardware-state-oper-disabled notification signifies that a component has transitioned into the 'disabled' state. |
| ietf-keystore | certificate-expiration                        | Indicates that the server implements the  'certificate-expiration' notification |
| ietf-netconf-notifications | netconf-config-change             | Generated when the NETCONF server detects that the `<running>` or `<startup>` configurationdatastore has been changed by a management session. The notification summarizes the edits that have been detected. The server MAY choose to also generate this notification while loading a datastore during the boot process for the device |
| ietf-netconf-notifications | netconf-capability-change         | Generated when the NETCONF server detects that the server capabilities have changed. Indicates which capabilities have been added, deleted, and/or modified. The manner in which a server capability is changed is outside the scope of this document. |
| ietf-netconf-notifications | netconf-session-start             | Generated when a NETCONF server detects that a NETCONF session has started. A server MAY generate this event for non-NETCONF management sessions. Indicates the identity of the user that started the session. |
| ietf-netconf-notifications | netconf-session-end               | Generated when a NETCONF server detects that a NETCONF session has terminated. A server MAY optionally generate this event for non-NETCONF management sessions. Indicates the identity of the user that owned the session, and why the session was terminated. |
| ietf-netconf-notifications | netconf-confirmed-commit          | Generated when a NETCONF server detects that a confirmed-commit event has occurred. Indicates the event and the current state of the confirmed-commit procedure in progress. |
| ietf-truststore | certificate-expiration                       | Indicates that the server implements the  'certificate-expiration' notification |
| ietf-yang-library | yang-library-update                        | Generated when any YANG library information on the server has changed. |
| ietf-yang-library | yang-library-change                        | Generated when the set of modules and submodules supported by the server has changed. |
| nc-notifications | replayComplete                              | This notification is sent to signal the end of a replay portion of a subscription. |
| nc-notifications | notificationComplete                        | This notification is sent to signal the end of a notification subscription. It is sent in the case that stopTime was specified during the creation of the subscription. |

> **Note:** The NETCONF Client can subscribe to any of above notifications that will be of interest; it is not necessary to subscribe to them all.

## 6.2. Keepalive

As per the NETCONF protocol specifications, the connection from NETCONF Client to NETCONF server must be kept open in order for YANG notifications to be published to any subscribed (i.e. connected) NETCONF Clients. Various approaches exist to facilitate the keepalive activity but it is recommended to select an approach with the minimum impact (RPC execution duration and response payload size etc.):

```text
                                         ┌────────────────┐
   ┌─────────┐                           │┌──────────────┐│
   │ NETCONF │                           ││NETCONF Server││
   │ Client  │                           │└──────────────┘│
   └────┬────┘                           │      OLT       │
        │                                └───────┬────────┘
        │ [1]<rpc>...</rpc>                      │
      ┌►│───────────────────────────────────────►│
      │ │          [2]<rpc-reply>...</rpc-reply> │
      │ │◄───────────────────────────────────────│
      │ │─┐ [3]delay                             │
      │ │◄┘                                      │
      │ │─┐ [4]repeat                            │
      └─┼─┘                                      │
        ┴                                        ┴
```

Such approaches include:

1. Perform a NETCONF RPC `<get-config>` of the `<running>` datastore
2. Perform a NETCONF RPC `<get-schema>` of the YANG model `ietf-netconf-monitoring@2010-10-04` [^4] [^5]

- Delay (e.g.) 2 minutes, detecting timeouts after (e.g.) 5 minutes, repeat.

[^4]: adjust the YANG module revision to match that listed within the returned NETCONF capabilities of the OLT
[^5]: recommend using the NETCONF RPC `<get-schema>` approach

## 6.3. Call Home

NETCONT Call Home, as per RFC8071, provides a secure mechanism for the OLT to call out to a designated endpoint to receive an operational configuration. The endpoint is required to be a compliant NETCONF Client supporting Call Home over one or both of the protocols `SSH` or `TLS`.

### 6.3.1. SSH

- Transport: `TCP`
- Protocol: `SSH`
- Port: `4334`

> **Note:** depending on the capabilities of the NETCONF Client, the SSH host key and any other credentials of the NETCONF Server should be loaded in advanced to allow authorized (whitelisted) OLTs to be determined during initial SSH open session operation, thus blocking any unauthorized or unknown OLTs:

```text
                                         ┌────────────────┐
   ┌─────────┐                           │┌──────────────┐│
   │ NETCONF │                           ││NETCONF Server││
   │ Client  │                           │└──────────────┘│
   └────┬────┘                           │      OLT       │
        │                                └───────┬────────┘
        │               [1]TCP connect port 4334 │
        │◄───────────────────────────────────────│
        │ [2]SSH open session                    │
        │───────────────────────────────────────►│
        │                             [3]<hello> │
        │◄───────────────────────────────────────│
        │                      [4]<capabilities> │
        │◄───────────────────────────────────────│
        │ [5]<hello>                             │
        │───────────────────────────────────────►│
        │ [6]<capabilities>                      │
        │───────────────────────────────────────►│
      ┌►│ [7]<get-schemas>...</get-schemas>      │
      │ │───────────────────────────────────────►│
      │ │          [8]<rpc-reply>...</rpc-reply> │
      │ │◄───────────────────────────────────────│
      │ │─┐ [9]repeat                            │
      └─┼─┘                                      │
        │ [10]<edit-config>...</edit-config>     │
        │───────────────────────────────────────►│
        │          [11]<rpc-reply>ok</rpc-reply> │
        │◄───────────────────────────────────────│
        │ [12]<system-restart>...                │
        │───────────────────────────────────────►│
        │          [13]<rpc-reply>ok</rpc-reply> │
        │◄───────────────────────────────────────│
        │ [14]<close-session/>                   │
        │───────────────────────────────────────►│
        │ [15]SSH close session                  │
        │───────────────────────────────────────►│
        │ [16]TCP disconnect port                │
        │───────────────────────────────────────►│
        │─┐ [17]delay/wait                       │
        │◄┘                                      │
        │ [18]TCP connect port 10830             │
        │───────────────────────────────────────►│
        │ [19]SSH open session                   │
        │───────────────────────────────────────►│
        │                            [20]<hello> │
        │◄───────────────────────────────────────│
        │ ...                                    │
        ┴                                        ┴
```

> **Note:** Steps `12` and `13` above are optional

### 6.3.2. TLS

- Transport: `TCP`
- Protocol: `TLS`
- Port: `4335` (NETCONF-over-TLS)

> **Note:** As per RFC8071 port `4336` is used for RESTCONF-over-TLS and is not applicable to this guide.
> **Note:** the TLS Certificate Authority (CA), certificates and keys are used authorized (whitelisted) OLTs, thus blocking any unauthorized or unknown OLTs - the configuration and subsequent management of CA, certificates and keys is beyond the scope of this guide as is highly dependent on the NETCONF Client architecture and operation:

```text
                                         ┌────────────────┐
   ┌─────────┐                           │┌──────────────┐│
   │ NETCONF │                           ││NETCONF Server││
   │ Client  │                           │└──────────────┘│
   └────┬────┘                           │      OLT       │
        │                                └───────┬────────┘
        │               [1]TCP connect port 4335 │
        │◄───────────────────────────────────────│
        │ [2]TLS open session                    │
        │───────────────────────────────────────►│
        │   [3]validate mutual CA chain/authent  │
        │◄──────────────────────────────────────►│
        │                             [4]<hello> │
        │◄───────────────────────────────────────│
        │                      [5]<capabilities> │
        │◄───────────────────────────────────────│
        │ [6]<hello>                             │
        │───────────────────────────────────────►│
        │ [7]<capabilities>                      │
        │───────────────────────────────────────►│
      ┌►│ [8]<get-schemas>...</get-schemas>      │
      │ │───────────────────────────────────────►│
      │ │          [9]<rpc-reply>...</rpc-reply> │
      │ │◄───────────────────────────────────────│
      │ │─┐ [10]repeat                           │
      └─┼─┘                                      │
        │ [11]<edit-config>...</edit-config>     │
        │───────────────────────────────────────►│
        │          [12]<rpc-reply>ok</rpc-reply> │
        │◄───────────────────────────────────────│
        │ [13]<system-restart>...                │
        │───────────────────────────────────────►│
        │          [14]<rpc-reply>ok</rpc-reply> │
        │◄───────────────────────────────────────│
        │ [15]<close-session/>                   │
        │───────────────────────────────────────►│
        │ [16]TLS close session                  │
        │───────────────────────────────────────►│
        │ [17]TCP disconnect port                │
        │───────────────────────────────────────►│
        │─┐ [18]delay/wait                       │
        │◄┘                                      │
        │ [19]TCP connect port 10830             │
        │───────────────────────────────────────►│
        │ [20]SSH open session                   │
        │───────────────────────────────────────►│
        │                            [21]<hello> │
        │◄───────────────────────────────────────│
        │ ...                                    │
        ┴                                        ┴
```

> **Note:** Steps `13` and `14` above are optional

---

# 7. Example Service Configuration Use Cases

## 7.1. Interfaces

> **Mode:** Combined-NE
> **Use Case:** 1:1 double-tagged VLAN service where the ONU/OLT accepts VLAN-tagged frames from CPE (QVID) sets CVID and SVID to operator-provided values (translating QVID to CVID) copies Pbits from vlan-tagged frame

```xml
<interface>
  <!--  VLAN sub-interface for ONU UNI -->
  <name>%%_vlan-subif-for-onu-uni-interface_%%</name>
  <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
  <enabled>true</enabled>
  <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <interface>%%_onu-uni-interface_%%</interface>
  </subif-lower-layer>
  <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <ingress-rule>
      <rule>
        <name>rule_1</name>
        <priority>100</priority>
        <flexible-match>
          <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
            <tag>
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>%%_QVID_%%</vlan-id>
                <pbit>any</pbit>
                <dei>any</dei>
              </dot1q-tag>
            </tag>
          </match-criteria>
        </flexible-match>
        <ingress-rewrite>
          <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
            <index>0</index>
            <dot1q-tag>
              <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
              <vlan-id>%%_CVID_%%</vlan-id>
              <pbit-from-tag-index>0</pbit-from-tag-index>
              <dei-from-tag-index>0</dei-from-tag-index>
            </dot1q-tag>
          </push-tag>
        </ingress-rewrite>
      </rule>
    </ingress-rule>
    <egress-rewrite>
      <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
      <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
        <index>0</index>
        <dot1q-tag>
          <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
          <vlan-id>%%_QVID_%%</vlan-id>
          <pbit-from-tag-index>0</pbit-from-tag-index>
          <dei-from-tag-index>0</dei-from-tag-index>
        </dot1q-tag>
      </push-tag>
    </egress-rewrite>
  </inline-frame-processing>
  <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">%%_qos-policy-profile_%%</ingress-qos-policy-profile>
</interface>
<interface>
  <!-- VLAN sub-interface for OLT v-enet corresponding to ONU UNI -->
  <name>%%_vlan-subif-for-olt-venet-interface_%%</name>
  <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
  <enabled>true</enabled>
  <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <interface>%%_olt-venet-interface_%%</interface>
  </subif-lower-layer>
  <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
    <interface-usage>user-port</interface-usage>
  </interface-usage>
  <egress-tm-objects xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
    <root-if-name>%%_channel-partition-interface_%%</root-if-name>
    <scheduler-node-name>NODE_DEF</scheduler-node-name>
  </egress-tm-objects>
  <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <ingress-rule>
      <rule>
        <name>rule_1</name>
        <priority>100</priority>
        <flexible-match>
          <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
            <tag>
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>%%_CVID_%%</vlan-id>
                <pbit>any</pbit>
                <dei>any</dei>
              </dot1q-tag>
            </tag>
          </match-criteria>
        </flexible-match>
        <ingress-rewrite>
          <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
        </ingress-rewrite>
      </rule>
    </ingress-rule>
    <egress-rewrite>
      <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
    </egress-rewrite>
  </inline-frame-processing>
  <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">%%_qos-policy-profile_%%</ingress-qos-policy-profile>
  <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">%%_qos-policy-profile_%%</egress-qos-policy-profile>
</interface>
<interface>
  <!-- VLAN sub-interface for OLT NNI -->
  <name>%%_vlan-subif-for-olt-nni-interface_%%</name>
  <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
  <enabled>true</enabled>
  <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <interface>%%_olt-nni-interface_%%</interface>
  </subif-lower-layer>
  <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
    <interface-usage>network-port</interface-usage>
  </interface-usage>
  <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
    <ingress-rule>
      <rule>
        <name>rule_1</name>
        <priority>100</priority>
        <flexible-match>
          <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
            <tag>
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                <vlan-id>%%_SVID_%%</vlan-id>
                <pbit>any</pbit>
                <dei>any</dei>
              </dot1q-tag>
            </tag>
            <tag>
              <index>1</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>%%_CVID_%%</vlan-id>
                <pbit>any</pbit>
                <dei>any</dei>
              </dot1q-tag>
            </tag>
          </match-criteria>
        </flexible-match>
        <ingress-rewrite>
          <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
        </ingress-rewrite>
      </rule>
    </ingress-rule>
    <egress-rewrite>
      <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
      <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
        <index>0</index>
        <dot1q-tag>
          <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
          <vlan-id>%%_SVID_%%</vlan-id>
          <pbit-from-tag-index>0</pbit-from-tag-index>
          <dei-from-tag-index>0</dei-from-tag-index>
        </dot1q-tag>
      </push-tag>
    </egress-rewrite>
  </inline-frame-processing>
</interface>
```

## 7.2. Classifiers

> **Mode:** Combined-NE
> **Use Case:** Set of Classifiers that roll up to a Policy and QOS Policy Profile supporting 8 total Traffic Classes and a 1-to-1 mapping between a Pbit value and a Traffic Class. QOS Policy Profile can be applied to the ingress or egress of a VLAN sub-interface as appropriate

```xml
<classifiers xmlns="urn:bbf:yang:bbf-qos-classifiers">
  <classifier-entry>
    <name>classifier-8tc.tc0.pbit0</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>0</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>0</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc1.pbit1</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>1</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>1</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc2.pbit2</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>2</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>2</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc3.pbit3</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>3</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>3</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc4.pbit4</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>4</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>4</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc5.pbit5</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>5</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>5</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc6.pbit6</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>6</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>6</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
  <classifier-entry>
    <name>classifier-8tc.tc7.pbit7</name>
    <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
    <match-criteria>
      <tag>
        <index>0</index>
        <in-pbit-list>7</in-pbit-list>
      </tag>
    </match-criteria>
    <classifier-action-entry-cfg>
      <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
      <scheduling-traffic-class>7</scheduling-traffic-class>
    </classifier-action-entry-cfg>
  </classifier-entry>
</classifiers>
<policies xmlns="urn:bbf:yang:bbf-qos-policies">
  <policy>
    <name>policy-8tc-pbits</name>
    <classifiers>
      <name>classifier-8tc.tc0.pbit0</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc1.pbit1</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc2.pbit2</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc3.pbit3</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc4.pbit4</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc5.pbit5</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc6.pbit6</name>
    </classifiers>
    <classifiers>
      <name>classifier-8tc.tc7.pbit7</name>
    </classifiers>
  </policy>
</policies>
<qos-policy-profiles xmlns="urn:bbf:yang:bbf-qos-policies">
  <policy-profile>
      <name>policy-profile-8tc-pbits</name>
      <policy-list>
        <name>policy-8tc-pbits</name>
      </policy-list>
  </policy-profile>
</qos-policy-profiles>
```

### 7.2.1. Traffic Shaping

> **Mode:** Combined-NE
> **Use Case:** Addition of new traffic shaping profile for exisint 1:1 single tagged connection

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <edit-config>
        <target>
            <running/>
        </target>
        <default-operation>merge</default-operation>
        <config>
            <tm-profiles xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
                <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
                    <name>shaper-pir.100M-pbs.10MB</name>
                    <single-token-bucket>
                        <pir>100000</pir>
                        <pbs>10000000</pbs>
                    </single-token-bucket>
                </shaper-profile>
                <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
                    <name>shaper-pir.500M-pbs.50MB</name>
                    <single-token-bucket>
                        <pir>500000</pir>
                        <pbs>50000000</pbs>
                    </single-token-bucket>
                </shaper-profile>
            </tm-profiles>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                <interface>
                    <name>cpart-1-1-1</name>
                    <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
                        <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
                            <name>NODE_DEF</name>
                            <scheduling-level>1</scheduling-level>
                            <queue xc:operation="delete">
                                <local-queue-id>0</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>1</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>2</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>3</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>4</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>5</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>6</local-queue-id>
                            </queue>
                            <queue xc:operation="delete">
                                <local-queue-id>7</local-queue-id>
                            </queue>
                            <child-scheduler-nodes>
                                <name>scheduler-onu001000-uni-1-1-1-venet-vlan.10.100-0</name>
                            </child-scheduler-nodes>
                        </scheduler-node>
                        <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
                            <name>scheduler-onu001000-uni-1-1-1-venet-vlan.10.100-0</name>
                            <scheduling-level>2</scheduling-level>
                            <shaper-name>shaper-pir.100M-pbs.10MB</shaper-name>
                            <queue>
                                <local-queue-id>0</local-queue-id>
                            </queue>
                        </scheduler-node>
                        <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
                            <name>NODE_DEF</name>
                        </child-scheduler-nodes>
                    </tm-root>
                </interface>
                <interface>
                    <name>onu001000-uni-1-1-1-venet-vlan.10.100-0</name>
                    <egress-tm-objects xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
                        <root-if-name>cpart-1-1-1</root-if-name>
                        <scheduler-node-name>scheduler-onu001000-uni-1-1-1-venet-vlan.10.100-0</scheduler-node-name>
                    </egress-tm-objects>
                </interface>
            </interfaces>
        </config>
    </edit-config>
</rpc>
```

## 7.3. Service Flow

> **Mode:** Combined-NE
> **Use Case:** Addition of new service flow and VLAN sub-interface operating over VLAN id `500`

```xml
<rpc message-id="9" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <edit-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <config>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces" xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                <interface>
                    <name>onu001000-uni-10-1-1-vlan.500</name>
                    <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
                    <enabled>true</enabled>
                    <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
                        <enable>true</enable>
                    </performance>
                    <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
                        <interface>onu001000-uni-10-1-1</interface>
                    </subif-lower-layer>
                    <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
                        <ingress-rule>
                            <rule>
                                <name>rule_0</name>
                                <priority>100</priority>
                                <flexible-match>
                                    <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                                        <ethernet-frame-type>any</ethernet-frame-type>
                                        <untagged/>
                                    </match-criteria>
                                </flexible-match>
                                <ingress-rewrite>
                                    <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                                    <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                                        <index>0</index>
                                        <dot1q-tag>
                                            <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                                            <vlan-id>500</vlan-id>
                                            <write-dei-0/>
                                            <write-pbit>0</write-pbit>
                                        </dot1q-tag>
                                    </push-tag>
                                </ingress-rewrite>
                            </rule>
                        </ingress-rule>
                        <egress-rewrite>
                            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                        </egress-rewrite>
                    </inline-frame-processing>
                    <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">policy-profile-ing-n1-td-GCKT5HVJNFGDZKOGMDCLBGSRJ4</ingress-qos-policy-profile>
                </interface>
            </interfaces>
        </config>
        <default-operation>merge</default-operation>
        <test-option>test-then-set</test-option>
        <error-option>rollback-on-error</error-option>
        <target>
            <running/>
        </target>
    </edit-config>
</rpc>
```

---

## 7.4. Inband Management

> **Mode:** Combined-NE
> **Use Case:** Set of LAG, NNI and VLAN that permit Inband management to occur through the OLT NNI. LAG is operating on `nni-1-1-1` and `nni-1-2-2` over VLAN id `2321`.

```xml
<rpc message-id="9" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<edit-config>
<target>
  <running/>
</target>
<default-operation>merge</default-operation>
<test-option>set</test-option>
<error-option>stop-on-error</error-option>
<config>
  <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
    <component>
      <name>mgt-port-1-1</name>
      <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
      <parent>slot-1</parent>
      <parent-rel-pos>41</parent-rel-pos>
    </component>
  </hardware>
  <lag-system xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
    <aggregating-system>
      <agg-system>STATIC-LAG-SYSTEM</agg-system>
      <system-id>AB-CD-12-34-56-60</system-id>
      <system-priority>1</system-priority>
    </aggregating-system>
  </lag-system>
  <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
    <interface>
        <name>LAG-1</name>
        <description>NNI_LAG</description>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ieee8023adLag</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
            <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregator xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
            <name>LAGAGG-1</name>
            <agg-system-name>STATIC-LAG-SYSTEM</agg-system-name>
            <admin-state>up</admin-state>
            <link-up-down-notification>enabled</link-up-down-notification>
            <aggregator-lacp>
                <actor-admin-key>1</actor-admin-key>
            </aggregator-lacp>
        </aggregator>
    </interface>
    <interface>
      <name>nni-1-3-1</name>
      <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
        <aggregation-port-lacp>
          <actor-system-priority>1</actor-system-priority>
          <actor-admin-key>1</actor-admin-key>
          <actor-port-priority>1</actor-port-priority>
        </aggregation-port-lacp>
      </aggregation-port>
     </interface>
    <interface>
      <name>nni-1-4-1</name>
      <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
        <aggregation-port-lacp>
          <actor-system-priority>1</actor-system-priority>
          <actor-admin-key>1</actor-admin-key>
          <actor-port-priority>1</actor-port-priority>
        </aggregation-port-lacp>
      </aggregation-port>
     </interface>
      <!-- Default logical Management Interface -->
      <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
        <name>mgt-int-1-1</name>
        <description>Default Inband management logical interface</description>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:other</type>
        <enabled>false</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>mgt-port-1-1</port-layer-if>
        </hardware-component>
      </interface>
    <!-- Default VSI on NNI-->
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <name>mgt-int-1-1-2321</name>
      <description>Default Inband management VLAN Sub-Interface facing mgt-int-1-1</description>
      <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
      <enabled>false</enabled>
      <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
        <enable>false</enable>
      </performance>
      <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
        <interface-usage>user-port</interface-usage>
      </interface-usage>
      <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <interface>mgt-int-1-1</interface>
      </subif-lower-layer>
      <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <ingress-rule>
          <rule>
            <name>mgmt-nni-cvlan</name>
            <priority>100</priority>
            <flexible-match>
              <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                <tag>
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>2321</vlan-id>
                    <pbit>any</pbit>
                    <dei>any</dei>
                  </dot1q-tag>
                </tag>
              </match-criteria>
            </flexible-match>
          </rule>
        </ingress-rule>
      </inline-frame-processing>
    </interface>
    <!-- Default VSI on NNI-->
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <name>mgt-int-lag-1-1-2321</name>
      <description>Default Inband management VLAN Sub-Interface facing NNI</description>
      <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
      <enabled>false</enabled>
      <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
        <enable>false</enable>
      </performance>
      <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
        <interface-usage>network-port</interface-usage>
      </interface-usage>
      <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <interface>LAG-1</interface>
      </subif-lower-layer>
      <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <ingress-rule>
          <rule>
            <name>mgmt-nni-cvlan</name>
            <priority>100</priority>
            <flexible-match>
              <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                <tag>
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>2321</vlan-id>
                    <pbit>any</pbit>
                    <dei>any</dei>
                  </dot1q-tag>
                </tag>
              </match-criteria>
            </flexible-match>
          </rule>
        </ingress-rule>
      </inline-frame-processing>
    </interface>
  </interfaces>
  <!-- Internal Managment VLAN forwarder -->
  <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
    <forwarders>
      <forwarder xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
        <name>forwarder-intmgt-1-2321</name>
        <ports>
          <port>
            <name>fwd-mgt-int-1-1-2321</name>
            <sub-interface>mgt-int-1-1-2321</sub-interface>
          </port>
          <port>
            <name>fwd-mgt-int-lag-1-1-2321</name>
            <sub-interface>mgt-int-lag-1-1-2321</sub-interface>
          </port>
        </ports>
      </forwarder>
    </forwarders>
  </forwarding>
</config>
</edit-config>
</rpc>
```

> **Mode:** Combined-NE
> **Use Case:** Removal of existing Inband management configuration using VLAN id `2321`

```xml
<rpc message-id="9" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<edit-config>
<target>
  <running/>
</target>
<default-operation>merge</default-operation>
<test-option>set</test-option>
<error-option>stop-on-error</error-option>
<config>
  <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
    <forwarders>
        <forwarder xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
            <name>forwarder-intmgt-1-2321</name>
        </forwarder>
    </forwarders>
  </forwarding>
  <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-1-1</name>
    </interface>
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-1-1-2321</name>
    </interface>
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-lag-1-1-2321</name>
    </interface>
  </interfaces>
</config>
</edit-config>
</rpc>
```

> **Mode:** Combined-NE
> **Use Case:** Deleting NNI and adding `LAG2` for Inband management configuration using VLAN id `2321`

```xml
<rpc message-id="9" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<edit-config>
<target>
  <running/>
</target>
<default-operation>merge</default-operation>
<test-option>set</test-option>
<error-option>stop-on-error</error-option>
<config>
  <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
    <forwarders>
        <forwarder xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
            <name>forwarder-intmgt-1-1220</name>
        </forwarder>
    </forwarders>
  </forwarding>
  <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-1-1</name>
    </interface>
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-1-1-1220</name>
    </interface>
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="delete">
      <name>mgt-int-nni-1-1-1220</name>
    </interface>
  </interfaces>
  <lag-system xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
    <aggregating-system>
      <agg-system>STATIC-LAG-SYSTEM</agg-system>
      <system-id>AB-CD-12-34-56-60</system-id>
      <system-priority>1</system-priority>
    </aggregating-system>
  </lag-system>
  <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
    <interface>
        <name>LAG-2</name>
        <description>NNI_LAG</description>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ieee8023adLag</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
            <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregator xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
            <name>AGG-LAG-2</name>
            <agg-system-name>STATIC-LAG-SYSTEM</agg-system-name>
            <admin-state>up</admin-state>
            <link-up-down-notification>enabled</link-up-down-notification>
            <aggregator-lacp>
                <actor-admin-key>2</actor-admin-key>
            </aggregator-lacp>
        </aggregator>
    </interface>
    <interface>
      <name>nni-1-1-1</name>
      <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
        <aggregation-port-lacp>
          <actor-system-priority>1</actor-system-priority>
          <actor-admin-key>2</actor-admin-key>
          <actor-port-priority>4</actor-port-priority>
        </aggregation-port-lacp>
      </aggregation-port>
     </interface>
    <interface>
      <name>nni-1-2-1</name>
      <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
        <aggregation-port-lacp>
          <actor-system-priority>1</actor-system-priority>
          <actor-admin-key>2</actor-admin-key>
          <actor-port-priority>4</actor-port-priority>
        </aggregation-port-lacp>
      </aggregation-port>
     </interface>
    <!-- Default logical Management Interface -->
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <name>mgt-int-1-1</name>
      <description>Default Inband management logical interface</description>
      <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:other</type>
      <enabled>false</enabled>
      <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
        <port-layer-if>mgt-port-1-1</port-layer-if>
      </hardware-component>
    </interface>
    <!-- Default VSI on MGT -->
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <name>mgt-int-1-1-1220</name>
      <description>Default Inband management VLAN Sub-Interface facing mgt-int-1-1</description>
      <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
      <enabled>false</enabled>
      <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
        <enable>false</enable>
      </performance>
      <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
        <interface-usage>user-port</interface-usage>
      </interface-usage>
      <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <interface>mgt-int-1-1</interface>
      </subif-lower-layer>
      <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <ingress-rule>
          <rule>
            <name>mgmt-nni-cvlan</name>
            <priority>100</priority>
            <flexible-match>
              <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                <tag>
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>1220</vlan-id>
                    <pbit>any</pbit>
                    <dei>any</dei>
                  </dot1q-tag>
                </tag>
              </match-criteria>
            </flexible-match>
          </rule>
        </ingress-rule>
      </inline-frame-processing>
    </interface>
    <!-- Default VSI on NNI -->
    <interface xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
      <name>mgt-int-lag-2-1-1220</name>
      <description>Default Inband management VLAN Sub-Interface facing NNI</description>
      <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
      <enabled>false</enabled>
      <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
        <enable>false</enable>
      </performance>
      <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
        <interface-usage>network-port</interface-usage>
      </interface-usage>
      <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <interface>LAG-2</interface>
      </subif-lower-layer>
      <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
        <ingress-rule>
          <rule>
            <name>mgmt-nni-cvlan</name>
            <priority>100</priority>
            <flexible-match>
              <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                <tag>
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>1220</vlan-id>
                    <pbit>any</pbit>
                    <dei>any</dei>
                  </dot1q-tag>
                </tag>
              </match-criteria>
            </flexible-match>
          </rule>
        </ingress-rule>
      </inline-frame-processing>
    </interface>
  </interfaces>
  <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
      <forwarders>
        <forwarder xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0">
          <name>forwarder-intmgt-1-1220</name>
          <ports>
            <port>
              <name>fwd-mgt-int-1-1-1220</name>
              <sub-interface>mgt-int-1-1-1220</sub-interface>
            </port>
            <port>
              <name>fwd-mgt-int-lag-2-1-1220</name>
              <sub-interface>mgt-int-lag-2-1-1220</sub-interface>
            </port>
          </ports>
        </forwarder>
      </forwarders>
  </forwarding>
</config>
</edit-config>
</rpc>
```

## 7.5. Point-to-Point Protocol over Ethernet (PPPoE)

> **Mode:** Combined-NE
> **Use Case:** Addition of PPPoE Intermediae Agent Profile

```xml
<rpc message-id="10" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<edit-config>
<target>
  <running/>
</target>
<default-operation>merge</default-operation>
<test-option>set</test-option>
<error-option>stop-on-error</error-option>
<config>
<!-- 2.Configures a PPPoE Intermediate Agent profile. -->
<bbf-pppoe-ia:pppoe-profiles xmlns:bbf-pppoe-ia="urn:bbf:yang:bbf-pppoe-intermediate-agent">
  <bbf-pppoe-ia:pppoe-profile>
    <bbf-pppoe-ia:name>PPPoE_Default</bbf-pppoe-ia:name>
    <bbf-pppoe-ia:pppoe-vendor-specific-tag>
      <bbf-pppoe-ia:subtag>remote-id</bbf-pppoe-ia:subtag>
      <bbf-pppoe-ia:default-circuit-id-syntax>Access_Node_ID eth Slot/Port:S-VID:C-VID</bbf-pppoe-ia:default-circuit-id-syntax>
      <bbf-pppoe-ia:default-remote-id-syntax>Access_Node_ID eth Slot/Port:S-VID:C-VID</bbf-pppoe-ia:default-remote-id-syntax>
      <bbf-pppoe-ia:access-loop-subtags/>
      <bbf-pppoe-ia:start-numbering-from-zero>false</bbf-pppoe-ia:start-numbering-from-zero>
      <bbf-pppoe-ia:use-leading-zeroes>false</bbf-pppoe-ia:use-leading-zeroes>
    </bbf-pppoe-ia:pppoe-vendor-specific-tag>
  </bbf-pppoe-ia:pppoe-profile>
</bbf-pppoe-ia:pppoe-profiles>
</config>
</edit-config>
</rpc>
```

> **Mode:** Combined-NE
> **Use Case:** During ONU configuration, the PPPoE profile can be referenced in the VLAN sub-interface

```xml
<!-- olt-side vlan sub-interface pppoe intermediate agent -->
<pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
    <enable>true</enable>
    <profile-ref>PPPoE_Default</profile-ref>
</pppoe>
```

## 7.6. Adding of DHCP Relay

> **Mode:** Combined-NE
> **Use Case:** Addition of DHCPv4 Relay profile specifing max packet size (MTU) and Option-82 sub-options

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1">
    <edit-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <config>
            <l2-dhcpv4-relay-profiles xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay" xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                <l2-dhcpv4-relay-profile>
                    <name>profile-dhcp-2</name>
                    <max-packet-size>1500</max-packet-size>
                    <option82-format>
                        <access-loop-suboptions/>
                        <default-circuit-id-syntax/>
                        <default-remote-id-syntax>C-VID:S-VID</default-remote-id-syntax>
                        <start-numbering-from-zero>false</start-numbering-from-zero>
                        <use-leading-zeroes>false</use-leading-zeroes>
                        <suboptions>remote-id</suboptions>
                    </option82-format>
                </l2-dhcpv4-relay-profile>
            </l2-dhcpv4-relay-profiles>
        </config>
        <default-operation>merge</default-operation>
        <test-option>test-then-set</test-option>
        <error-option>rollback-on-error</error-option>
        <target>
            <running/>
        </target>
    </edit-config>
</rpc>
```

## 7.7. Adding Multicast VLAN

> **Mode:** Combined-NE
> **Use Case:** Addition of a multicast VLAN on the OLT, supporting VLAN id `500`, traffic class `0` for GEMPORTS `8959` and `4092`, operating over interface `uplink_port_intf5` with flexible ingress filtering

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1">
    <edit-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <config>
            <xpon xmlns="urn:bbf:yang:bbf-xpon">
                <multicast-distribution-set xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                    <multicast-set>
                        <name>multicast-set-cgroup-1-1-1.CPart_1.CPair_gpon-set-0</name>
                        <multicast-gemport-ref>multicast-gemport-cgroup-1-1-1.CPart_1.CPair_gpon-gem-0</multicast-gemport-ref>
                        <vlan-list>
                            <multicast-vlan-id>500</multicast-vlan-id>
                        </vlan-list>
                    </multicast-set>
                    <multicast-set>
                        <name>multicast-set-cgroup-1-2-1.CPart_1.CPair_gpon-set-0</name>
                        <multicast-gemport-ref>multicast-gemport-cgroup-1-2-1.CPart_1.CPair_gpon-gem-0</multicast-gemport-ref>
                        <vlan-list>
                            <multicast-vlan-id>500</multicast-vlan-id>
                        </vlan-list>
                    </multicast-set>
                </multicast-distribution-set>
                <multicast-gemports xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                    <multicast-gemport>
                        <name>multicast-gemport-cgroup-1-1-1.CPart_1.CPair_gpon-gem-0</name>
                        <gemport-id>8959</gemport-id>
                        <interface>cgroup-1-1-1.CPart_1.CPair_gpon</interface>
                        <traffic-class>0</traffic-class>
                        <is-broadcast>true</is-broadcast>
                    </multicast-gemport>
                    <multicast-gemport>
                        <name>multicast-gemport-cgroup-1-2-1.CPart_1.CPair_gpon-gem-0</name>
                        <gemport-id>4092</gemport-id>
                        <interface>cgroup-1-2-1.CPart_1.CPair_gpon</interface>
                        <traffic-class>0</traffic-class>
                        <is-broadcast>true</is-broadcast>
                    </multicast-gemport>
                </multicast-gemports>
            </xpon>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces" xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                <interface>
                    <name>uplink_port_intf5-vlan.500</name>
                    <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
                    <enabled>true</enabled>
                    <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
                        <enable>true</enable>
                    </performance>
                    <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
                        <interface-usage>network-port</interface-usage>
                    </interface-usage>
                    <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
                        <interface>uplink_port_intf5</interface>
                    </subif-lower-layer>
                    <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
                        <ingress-rule>
                            <rule>
                                <name>rule_1</name>
                                <priority>100</priority>
                                <flexible-match>
                                    <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                                        <tag>
                                            <index>0</index>
                                            <dot1q-tag>
                                                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                                                <vlan-id>500</vlan-id>
                                                <pbit>any</pbit>
                                                <dei>any</dei>
                                            </dot1q-tag>
                                        </tag>
                                    </match-criteria>
                                </flexible-match>
                                <ingress-rewrite>
                                    <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                                </ingress-rewrite>
                            </rule>
                        </ingress-rule>
                    </inline-frame-processing>
                </interface>
            </interfaces>
            <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
                <forwarders xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                    <forwarder>
                        <name>forwarder-vlan.500</name>
                        <mac-learning>
                            <forwarding-database>forwarding-database-vlan.500</forwarding-database>
                        </mac-learning>
                        <ports>
                            <port>
                                <name>forwarder-uplink_port_intf5-vlan.500</name>
                                <sub-interface>uplink_port_intf5-vlan.500</sub-interface>
                            </port>
                        </ports>
                    </forwarder>
                </forwarders>
                <forwarding-databases xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
                    <forwarding-database>
                        <name>forwarding-database-vlan.500</name>
                        <max-number-mac-addresses>16384</max-number-mac-addresses>
                        <aging-timer>300</aging-timer>
                        <shared-forwarding-database>false</shared-forwarding-database>
                    </forwarding-database>
                </forwarding-databases>
            </forwarding>
        </config>
        <default-operation>merge</default-operation>
        <test-option>test-then-set</test-option>
        <error-option>rollback-on-error</error-option>
        <target>
            <running/>
        </target>
    </edit-config>
</rpc>
```

## 7.8. Deleting VLAN

> **Mode:** Combined-NE
> **Use Case:** Deleting existing VLAN (doubled tagged) and related interfaces; example below VLAN id `100` (outer) and `10` (inner)

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1">
  <edit-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <config>
      <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
        <forwarders>
          <forwarder xc:operation="delete">
            <name>forwarder-onu001000-0</name>
          </forwarder>
        </forwarders>
      </forwarding>
      <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
        <interface  xc:operation="delete">
          <name>nni-1-1-1-vlan.10.100-0</name>
        </interface>
        <interface  xc:operation="delete">
          <name>onu001000-uni-1-1-1-venet-vlan.10.100-0</name>
        </interface>
        <interface  xc:operation="delete">
          <name>onu001000-uni-1-1-1-vlan.10.100</name>
        </interface>
      </interfaces>
    </config>
  </edit-config>
</rpc>
```

## 7.9. OLT Restart

> **Mode:** Combined-NE
> **Use Case:** Restart OLT, configuration is persistent.

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1">
  <system-restart xmlns="urn:ietf:params:xml:ns:yang:ietf-system"/>
</rpc>
```

> **Note:** This form of restart is classified as a warm restart

## 7.10. OLT Software Upgrade

> **Mode:** Combined-NE
> **Use Case:** Upgrade running software release on the pOLT; example assumes hardware component of type `iana-hardware:chassis` and with name `chassis` is added to represent the pOLT identity as far as the upgrade pOLT YANG is concerned. Operation shown is `create-or-replace` but could also be simply `replace`. The software name is hardcoded as `OLT-FIRMWARE`.

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1001">
  <action xmlns="urn:ietf:params:xml:ns:yang:1">
    <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
      <component>
        <name>chassis</name>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>OLT-FIRMWARE</name>
            <revisions>
              <download>
                <download>
                  <source>
                    <url>http://ponlab/images/pon-bulkrelease/1.0.0/</url>
                  </source>
                  <target>
                    <selected-by-system>
                      <operation>create-or-replace</operation>
                    </selected-by-system>
                    <alias>1.0.0</alias>
                  </target>
                </download>
              </download>
            </revisions>
          </software>
        </software>
      </component>
    </hardware>
  </action>
</rpc>
```

## 7.11. ONU Software Upgrade

> **Mode:** Combined-NE
> **Use Case:** Upgrade running software release of a connected ONU via the pOLT; example assumes hardware component of type `iana-hardware:chassis` and with name `onu001000-chassis` is added to represent the pOLT identity as far as the upgrade pOLT YANG is concerned. The software name is hardcoded as `ONU-FIRMWARE`.

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="1001">
  <action xmlns="urn:ietf:params:xml:ns:yang:1">
    <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
      <component>
        <name>onu001000-chassis</name>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
            <revisions>
              <download>
                <download>
                  <source>
                    <url>http://ponlab/images/pon-onu/CIG/XG-99C/R4.4.22.012</url>
                  </source>
                  <target>
                    <selected-by-system>
                      <operation>replace</operation>
                    </selected-by-system>
                    <alias>R4.4.22.012</alias>
                  </target>
                </download>
              </download>
            </revisions>
          </software>
        </software>
      </component>
    </hardware>
  </action>
</rpc>
```

## 7.12. NETCONF Call Home

> **Mode:** Combined-NE
> **Use Case:** Instruct OLT to activate Call Home functionality of the pOLT; pOLT will attempt to reach specified remote host `10.45.104.22` over TCP port `4334` using the SSH protocol.

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="34566750">
  <edit-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
    <target>
      <running/>
    </target>
    <config>
      <netconf-server xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-server">
        <call-home>
          <netconf-client>
            <name>default-client</name>
            <endpoints>
              <endpoint>
                <name>default-ssh</name>
                <ssh>
                  <tcp-client-parameters>
                    <remote-address>10.45.104.22</remote-address>
                    <remote-port>4334</remote-port>
                    <keepalives>
                      <idle-time>1</idle-time>
                      <max-probes>10</max-probes>
                      <probe-interval>5</probe-interval>
                    </keepalives>
                  </tcp-client-parameters>
                  <ssh-server-parameters>
                    <server-identity>
                      <host-key>
                        <name>default-key</name>
                        <public-key>
                          <keystore-reference>genkey</keystore-reference>
                        </public-key>
                      </host-key>
                    </server-identity>
                    <client-authentication>
                      <supported-authentication-methods>
                        <publickey/>
                        <password/>
                      </supported-authentication-methods>
                      <users/>
                    </client-authentication>
                  </ssh-server-parameters>
                </ssh>
              </endpoint>
            </endpoints>
            <connection-type>
              <persistent/>
            </connection-type>
          </netconf-client>
        </call-home>
      </netconf-server>
    </config>
  </edit-config>
</rpc>
```

> **Mode:** Combined-NE
> **Use Case:** Instruct OLT to disabe Call Home functionality of the pOLT; during a successful Call Home operation it is recommended the supplied configuration includes disabling the pOLT Call Home function

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="31415926535">
  <edit-config xmlns:nb="urn:ietf:params:xml:ns:netconf:base:1.0">
    <target>
      <running/>
    </target>
    <config>
      <netconf-server xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-server">
        <call-home>
          <netconf-client nb:operation="delete">
            <name>default-client</name>
          </netconf-client>
        </call-home>
      </netconf-server>
    </config>
  </edit-config>
</rpc>
```

## 7.13. MAC Forwarder Table

> **Mode:** Combined-NE
> **Use Case:** Retrieve known MAC addresses in the bridge (forwarding) table

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
  <get>
    <filter type="subtree">
      <forwarding-state xmlns="urn:bbf:yang:bbf-l2-forwarding">
        <forwarding-databases/>
      </forwarding-state>
    </filter>
  </get>
</rpc>
```

> **Mode:** Combined-NE
> **Use Case:** Clear the MAC address table. Ensure `<forwarding-database>` `<name>` exists and change `<aging-timer>` value each time when you use it so this leaf is sent down to pOLT

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
  <edit-config>
    <target>
      <running/>
    </target>
    <default-operation>merge</default-operation>
    <config>
      <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
        <forwarding-databases>
          <forwarding-database>
            <name>alpha-olt-6-composite_ALLONUS_DEFAULT</name>
            <aging-timer>1</aging-timer>
          </forwarding-database>
        </forwarding-databases>
      </forwarding>
    </config>
  </edit-config>
</rpc>
```

> **Note:** Aging-timer >= 300 seconds is a no operation and the MAC address table will not be cleared

## 7.14. OLT ONUs interfaces & GEMPORT states

> **Mode:** Combined-NE
> **Use Case:** Retrieve OLT ONUs interfaces & GEMPORT states

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
  <get>
    <input xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
      <filter type="subtree">
        <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
          <component>
            <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis
      </class>
            <name/>
            <serial-num/>
          </component>
        </hardware>
        <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
          <interface>
            <name/>
            <type/>
          </interface>
        </interfaces>
        <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
          <interface>
            <name/>
            <type/>
            <performance xmlns="urn:bbf:yang:bbf-interfaces-performance-management">
              <intervals-15min>
                <history>
                  <interval-number>1</interval-number>
                </history>
              </intervals-15min>
            </performance>
          </interface>
        </interfaces-state>
        <xpongemtcont-state xmlns="urn:bbf:yang:bbf-xpongemtcont">
          <gemports>
            <gemport>
              <name/>
              <actual-gemport-id/>
              <performance xmlns="urn:bbf:yang:bbf-xpongemtcont-gemport-performance-management">
                <intervals-15min>
                  <history>
                    <interval-number>1</interval-number>
                  </history>
                </intervals-15min>
              </performance>
            </gemport>
          </gemports>
        </xpongemtcont-state>
      </filter>
    </input>
  </get>
</rpc>
```

## 7.15. OLT Port Statistics

> **Mode:** Combined-NE
> **Use Case:** Retrieve OLT port statistics

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <get xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <filter>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                <interface>
                    <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
                </interface>
                <interface>
                    <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
                </interface>
            </interfaces>
        </filter>
    </get>
</rpc>
```

## 7.16. List of OLT Ethernet Interfaces

> **Mode:** Combined-NE
> **Use Case:** Retrieve OLT Ethernet-like interfaces regardless of speed by specifing type of `ethernetCsmacd`. To retrieve Link Aggregation interfaces specify `ieee8023adLag` as the filtered type.

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <get xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <filter>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                <interface>
                    <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
                </interface>
            </interfaces>
        </filter>
    </get>
</rpc>
```

## 7.17. List of ONUs By Name

> **Mode:** Combined-NE
> **Use Case:** Retrieve list of onboarded ONUs where the ONU name starts with `onu001000`

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <get xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <filter xmlns:hw="urn:ietf:params:xml:ns:yang:ietf-hardware" type="xpath" select="/hw:hardware/hw:component[starts-with(hw:name, 'onu001000')]"/>
    </get>
</rpc>
```

## 7.18. OLT Metadata

> **Mode:** Combined-NE
> **Use Case:** Retrieve OLT metadata such as vendor-specific information for identifying the system platform and operating system (OS) including name/release/version and a vendor-specific identifier string representingthe hardware in use

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <get xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <filter>
            <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
                <interface>
                    <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
                </interface>
            </interfaces>
        </filter>
    </get>
</rpc>
```

## 7.19. Persist Running Configuration

> **Mode:** Combined-NE
> **Use Case:** Save the current `running` configuration of the OLT to the persistent store for subsequent use upon next `startup`

```xml
<rpc xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="12345">
    <copy-config xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <source>
            <running/>
        </source>
        <target>
            <startup/>
        </target>
    </copy-config>
</rpc>
```

## 7.20. Configurable Notifications

> **Mode:** Combined-NE
> **Use Case:** The Broadband Forum `xPON` YANG models allow for vANI PON specific defects to be configured to generate YANG notifications. Example assumes ONU named `onu001001`, ONU id `1`, serial number `CIGG00210777` and channel partition of `cgroup-1-1-1.CPart_1`. The notifiable defects are part of the YANG sub-module `bbf-xponani-ani-body`, the available defects are defined as YANG identifiers in YANG module `bbf-xpon-defects` and as that module only contains definitive type constructs (i.e. YANG `identity`) is not included in [11. `XPON` YANG Data Models](#11-xpon-yang-data-models)

```xml
<rpc message-id="9" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <edit-config>
    <target>
        <running/>
    </target>
    <config>
        <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces" xmlns:xc="urn:ietf:params:xml:ns:netconf:base:1.0" xc:operation="merge">
            <interface>
                <name>onu001001</name>
                <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
                <enabled>true</enabled>
                <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
                    <channel-partition>cgroup-1-1-1.CPart_1</channel-partition>
                    <onu-id>1</onu-id>
                    <expected-serial-number>CIGG00210777</expected-serial-number>
                    <preferred-channel-pair>cgroup-1-1-1.CPart_1.CPair_gpon</preferred-channel-pair>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lcdgi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:rdii</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:misi</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
                    <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
                </v-ani>
            </interface>
        </interfaces>
    </config>
    <default-operation>merge</default-operation>
    <test-option>test-then-set</test-option>
    <error-option>rollback-on-error</error-option>
  </edit-config>
</rpc>
```

For convience an explaination of the above defect types are shown below:

| Defect               | Summary                                                      | Description                                                  |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `bbf-xpon-def:lobi`  | lost of burst for ONUi                                       | Failure to delineate, for any reason, the specified number,Clobi, of consecutive scheduled bursts from ONUi |
| `bbf-xpon-def:tiwi`  | Transmission interference warning for ONUi                   | The Optical Network Unit (ONU) transmission drift exceeds the outer (TIW) threshold, and remains outside the threshold after three consecutive attempts to correct it with a Ranging_Time Physical Layer Operations, Administrations and Maintenance |
| `bbf-xpon-def:dfi`   | Disable failure of ONUi                                      | The Optical Network Unit (ONU) continues to respond to the upstream allocations after an attempt to disable the ONU using its serial number (with one or more Disable_Serial_Number Physical Layer Operations, Administrations and Maintenance (PLOAM) messages) which may have been preceded by a failed attempt to deactivate the ONU (with one or more Dectivate_ONU PLOAM messages). |
| `bbf-xpon-def:lopci` | Loss of Physical Layer Operations, Administrations and Maintenance (PLOAM) channel with ONUi | Generic defect indicating breakage of the PLOAM protocol: persistent MIC failure in the upstream; lack of acknowledgements or proper PLOAM responses from the Optical Network Unit (ONU). Persistent means that the same irregular condition is observed consecutively at least three times. |
| `bbf-xpon-def:looci` | loss of OMCC channel with ONUi                               | Recognized by the Optical Line Termination's (OLT) Optical Network Unit Management and Control Interface (OMCI) processing engine (based on the persistent MIC failure in the upstream). |
| `bbf-xpon-def:dowi`  | Drift of window of ONUi                                      | In N sequential bursts from the given Optical Network Unit (ONU), the transmission drift exceeds the lower of two specified drift thresholds. This condition indicates that while the transmission phase has shifted, it remains correctable via EqD update. |
| `bbf-xpon-def:sfi`   | Signal fail of ONUi                                          | When the upstream Bit Error Rate (BER) of ONUi becomes >= 10^-Y, this state is entered. Y is configurable. |
| `bbf-xpon-def:sdi`   | Signal degraded of ONUi                                      | When the upstream Bit Error Rate (BER) of ONUi becomes >= 10^-X, this state is entered. Y is configurable and must be higher than Y (the SFi threshold). |
| `bbf-xpon-def:lcdgi` | Loss of G-PON Encapsulation Method (GEM) channel delineation | When GEM fragment delineation of ONUi is lost.               |
| `bbf-xpon-def:rdii`  | Remote defect indication of ONUi                             | When the RDI field of ONUi is asserted. The Optical Line Termination (OLT) transmission is received with defects at the ONUi. |
| `bbf-xpon-def:loai`  | Loass of acknowledge with ONUi                               | The Optical Line Termination (OLT) does not receive an acknowledgement from ONUi after a set of downstream messages that imply an upstream acknowledge. |
| `bbf-xpon-def:dgi`   | Receive dying gasp (DG) of ONUi                              | When the Optical Line Termination (OLT) receives DG message from ONUi, DGi is asserted. |
| `bbf-xpon-def:memi`  | Message_Error message from ONUi                              | When the Optical Line Termination (OLT) receives an unknown message from ONUi. |
| `bbf-xpon-def:misi`  | Link mismatch of ONUi                                        | The Optical Line Termination (OLT) detects that the received PSTi and the transmitted PST are different. |
| `bbf-xpon-def:peei`  | Physical equipment error (PEE) of ONUi                       | When the Optical Line Termination (OLT) receives a PEE message from the Optical Network Unit (ONU). |
| `bbf-xpon-def:loki`  | Loss of key synch with ONUi                                  | Key transmission from the Optical Network Unit (ONU) in response to Request_Key message fails three times. |

> **Note:** For further details refer to external specification *ITU-T G.984.3 (01/2014)*
> **Note:** For additional notifiable defects, refer to YANG module `bbf-xpon-defects`

## 7.21. Example Full Simple Configuration

> **Mode:** Combined-NE
> **Use Case:** NETCONFIG get-config RPC requested by client from server running datastore, with configuration supporting three ONUs (serial numbers listed below), ingress untagged (VLAN id `1`) over static LAG (not LACP), egress (C-VLAN) stacked VLAN id `101`

- CIG
  - CIGG-21b0126a
  - CIGG-21b012f4
- Adtran
  - ADTN-2017e038

```xml
<rpc-reply xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="get-running-10">
  <data>
    <l2-dhcpv4-relay-profiles xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
      <l2-dhcpv4-relay-profile>
        <name>DHCP_Default_DoubleTag</name>
        <max-packet-size>1500</max-packet-size>
        <option82-format>
          <suboptions>remote-id</suboptions>
          <suboptions>circuit-id</suboptions>
          <default-circuit-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-circuit-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </option82-format>
      </l2-dhcpv4-relay-profile>
      <l2-dhcpv4-relay-profile>
        <name>DHCP_Default_SingleTag</name>
        <max-packet-size>1500</max-packet-size>
        <option82-format>
          <suboptions>remote-id</suboptions>
          <suboptions>circuit-id</suboptions>
          <default-circuit-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-circuit-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </option82-format>
      </l2-dhcpv4-relay-profile>
    </l2-dhcpv4-relay-profiles>
    <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
      <forwarders>
        <forwarder>
          <name>ONU-1-1_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-1-1_fwd_user_port_1</name>
              <sub-interface>ONU-1-1_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-1_fwd_net_port_1</name>
              <sub-interface>ONU-1-1_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>onu-1-2_forwarder_1</name>
          <ports>
            <port>
              <name>onu-1-2_fwd_user_port_1</name>
              <sub-interface>onu-1-2_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>onu-1-2_fwd_net_port_1</name>
              <sub-interface>onu-1-2_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>onu-1-3_forwarder_1</name>
          <ports>
            <port>
              <name>onu-1-3_fwd_user_port_1</name>
              <sub-interface>onu-1-3_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>onu-1-3_fwd_net_port_1</name>
              <sub-interface>onu-1-3_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
      </forwarders>
      <forwarding-databases>
        <forwarding-database>
          <name>gregson_ALLONUS_DEFAULT</name>
          <max-number-mac-addresses>5</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
      </forwarding-databases>
    </forwarding>
    <dhcpv6-ldra-profiles xmlns="urn:bbf:yang:bbf-ldra">
      <dhcpv6-ldra-profile>
        <name>DHCPv6_Default_DoubleTag</name>
        <options>
          <option>remote-id</option>
          <option>interface-id</option>
          <option>subscriber-id</option>
          <option>enterprise-number</option>
          <option>vendor-specific-information</option>
          <default-interface-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-interface-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-remote-id-syntax>
          <enterprise-number>23330</enterprise-number>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </options>
      </dhcpv6-ldra-profile>
      <dhcpv6-ldra-profile>
        <name>DHCPv6_Default_SingleTag</name>
        <options>
          <option>remote-id</option>
          <option>interface-id</option>
          <option>subscriber-id</option>
          <option>enterprise-number</option>
          <option>vendor-specific-information</option>
          <default-interface-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-interface-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-remote-id-syntax>
          <enterprise-number>23330</enterprise-number>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </options>
      </dhcpv6-ldra-profile>
    </dhcpv6-ldra-profiles>
    <link-table xmlns="urn:bbf:yang:bbf-link-table">
      <link-table>
        <from-interface>ONU-1-1-ani-2-1-1</from-interface>
        <to-interface>ONU-1-1_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-1-uni-1-1-1</from-interface>
        <to-interface>ONU-1-1_venet_uni-port-1-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>onu-1-2-ani-1-1-1</from-interface>
        <to-interface>onu-1-2_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>onu-1-2-uni-10-1-1</from-interface>
        <to-interface>onu-1-2_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>onu-1-3-ani-1-1-1</from-interface>
        <to-interface>onu-1-3_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>onu-1-3-uni-10-1-1</from-interface>
        <to-interface>onu-1-3_venet_uni-port-10-1_if</to-interface>
      </link-table>
    </link-table>
    <pppoe-profiles xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
      <pppoe-profile>
        <name>PPPoE_doubleTag</name>
        <pppoe-vendor-specific-tag>
          <subtag>circuit-id</subtag>
          <subtag>remote-id</subtag>
          <default-circuit-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-circuit-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:N2VID:ONUID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
      <pppoe-profile>
        <name>PPPoE_singleTag</name>
        <pppoe-vendor-specific-tag>
          <subtag>circuit-id</subtag>
          <subtag>remote-id</subtag>
          <default-circuit-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-circuit-id-syntax>
          <default-remote-id-syntax>gregson eth Slot/Port:N-VID:ONUID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
    </pppoe-profiles>
    <classifiers xmlns="urn:bbf:yang:bbf-qos-classifiers">
      <classifier-entry>
        <name>onuIngressDefault_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>1</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>3</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>3</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier4</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>4</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>4</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier5</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>5</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier6</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>6</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>6</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier7</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>7</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0-1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2-4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5-6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_1class_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0-7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>1</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>3</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>3</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier4</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>4</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>4</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier5</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>5</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier6</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>6</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>6</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier7</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>7</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
    </classifiers>
    <policies xmlns="urn:bbf:yang:bbf-qos-policies">
      <policy>
        <name>onuIngressDefault_policy</name>
        <classifiers>
          <name>onuIngressDefault_classifier0</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier1</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier2</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier3</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier4</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier5</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier6</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier7</name>
        </classifiers>
      </policy>
      <policy>
        <name>onuIngress_4class_policy</name>
        <classifiers>
          <name>onuIngress_4class_classifier0</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier1</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier2</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier3</name>
        </classifiers>
      </policy>
      <policy>
        <name>onuIngress_1class_policy</name>
        <classifiers>
          <name>onuIngress_1class_classifier0</name>
        </classifiers>
      </policy>
      <policy>
        <name>QPP0_policy</name>
        <classifiers>
          <name>QPP0_classifier0</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier1</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier2</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier3</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier4</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier5</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier6</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier7</name>
        </classifiers>
      </policy>
    </policies>
    <qos-policy-profiles xmlns="urn:bbf:yang:bbf-qos-policies">
      <policy-profile>
        <name>onuIngressDefault_profile</name>
        <policy-list>
          <name>onuIngressDefault_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>onuIngress_4class_profile</name>
        <policy-list>
          <name>onuIngress_4class_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>onuIngress_1class_profile</name>
        <policy-list>
          <name>onuIngress_1class_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>QPP0_profile</name>
        <policy-list>
          <name>QPP0_policy</name>
        </policy-list>
      </policy-profile>
    </qos-policy-profiles>
    <tm-profiles xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-100Mbps-pbs-10MB</name>
        <single-token-bucket>
          <pir>100000</pir>
          <pbs>10000000</pbs>
        </single-token-bucket>
      </shaper-profile>
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-1Gbps-pbs-100MB</name>
        <single-token-bucket>
          <pir>1000000</pir>
          <pbs>100000000</pbs>
        </single-token-bucket>
      </shaper-profile>
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-200Mbps-pbs-20MB</name>
        <single-token-bucket>
          <pir>200000</pir>
          <pbs>20000000</pbs>
        </single-token-bucket>
      </shaper-profile>
    </tm-profiles>
    <xpongemtcont xmlns="urn:bbf:yang:bbf-xpongemtcont">
      <traffic-descriptor-profiles>
        <traffic-descriptor-profile>
          <name>TDP_XGS_DEFAULT</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>9950000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>TDP_GPON_DEFAULT</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>1200000000</maximum-bandwidth>
        </traffic-descriptor-profile>
      </traffic-descriptor-profiles>
      <tconts>
        <tcont>
          <name>ONU-1-1_tcont_0</name>
          <alloc-id>1032</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-1_tcont_1</name>
          <alloc-id>1033</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-1_tcont_2</name>
          <alloc-id>1034</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-1_tcont_3</name>
          <alloc-id>1035</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_0</name>
          <alloc-id>1040</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_1</name>
          <alloc-id>1041</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_2</name>
          <alloc-id>1042</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_3</name>
          <alloc-id>1043</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_4</name>
          <alloc-id>1044</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_5</name>
          <alloc-id>1045</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_6</name>
          <alloc-id>1046</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-2_tcont_7</name>
          <alloc-id>1047</alloc-id>
          <interface-reference>onu-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_0</name>
          <alloc-id>1048</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_1</name>
          <alloc-id>1049</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_2</name>
          <alloc-id>1050</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_3</name>
          <alloc-id>1051</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_4</name>
          <alloc-id>1052</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_5</name>
          <alloc-id>1053</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_6</name>
          <alloc-id>1054</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>onu-1-3_tcont_7</name>
          <alloc-id>1055</alloc-id>
          <interface-reference>onu-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>TDP_XGS_DEFAULT</traffic-descriptor-profile-ref>
        </tcont>
      </tconts>
      <gemports>
        <gemport>
          <name>ONU-1-1_gem_0</name>
          <gemport-id>1032</gemport-id>
          <interface>ONU-1-1_venet_uni-port-1-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-1_gem_1</name>
          <gemport-id>1033</gemport-id>
          <interface>ONU-1-1_venet_uni-port-1-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-1_gem_2</name>
          <gemport-id>1034</gemport-id>
          <interface>ONU-1-1_venet_uni-port-1-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_0</name>
          <gemport-id>1040</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_1</name>
          <gemport-id>1041</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_2</name>
          <gemport-id>1042</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_3</name>
          <gemport-id>1043</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>3</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_3</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_4</name>
          <gemport-id>1044</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>4</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_4</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_5</name>
          <gemport-id>1045</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>5</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_5</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_6</name>
          <gemport-id>1046</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>6</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_6</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-2_gem_7</name>
          <gemport-id>1047</gemport-id>
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>7</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-2_tcont_7</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_0</name>
          <gemport-id>1048</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_1</name>
          <gemport-id>1049</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_2</name>
          <gemport-id>1050</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_3</name>
          <gemport-id>1051</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>3</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_3</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_4</name>
          <gemport-id>1052</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>4</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_4</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_5</name>
          <gemport-id>1053</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>5</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_5</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_6</name>
          <gemport-id>1054</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>6</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_6</tcont-ref>
        </gemport>
        <gemport>
          <name>onu-1-3_gem_7</name>
          <gemport-id>1055</gemport-id>
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>7</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>onu-1-3_tcont_7</tcont-ref>
        </gemport>
      </gemports>
    </xpongemtcont>
    <lag-system xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
      <aggregating-system>
        <agg-system>STATIC-LAG-SYSTEM</agg-system>
        <system-id>AB-CD-12-34-56-60</system-id>
        <system-priority>1</system-priority>
      </aggregating-system>
    </lag-system>
    <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
      <component>
        <name>chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
      </component>
      <component>
        <name>slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:slot</class>
        <parent>chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-2</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-3</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-4</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-5</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>5</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-6</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>6</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-7</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>7</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-8</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>8</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-9</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>9</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-10</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>10</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-11</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>11</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-12</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>12</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-13</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>13</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-14</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>14</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-15</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>15</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-16</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>16</parent-rel-pos>
      </component>
      <component>
        <name>olt-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>olt-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>olt-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>olt-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-transceiver-1-9</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>olt-port-1-9</parent>
        <parent-rel-pos>9</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>olt-transceiver-link-1-9-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>olt-transceiver-1-9</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-transceiver-link-1-9-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>olt-transceiver-1-9</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-2</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>5</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-3</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>9</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-4</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>13</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-1-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-1</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-1-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-1</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-1-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-1</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-2</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-2-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-2</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-2-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-2</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-2-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-2</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-2-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-2</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-3</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-3-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-3</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-3-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-3</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-3-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-3</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-3-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-3</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-4</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-4-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-4</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-4-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-4</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-4-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-4</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-4-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-4</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>mgt-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-1-1-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-1-ani-slot-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>2</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-1-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-1-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-port-2-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-1-ani-slot-2</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-1-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-transceiver-2-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-1-ani-port-2-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-1-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-transceiver-link-2-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-1-ani-transceiver-2-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>onu-1-2-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-2-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-2-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-2-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-2-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-2-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-2-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-2-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-2-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-2-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-2-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-2-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-2-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-2-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-2-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>onu-1-3-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-3-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-3-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-3-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>onu-1-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>onu-1-3-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-3-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-3-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>onu-1-3-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-3-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-3-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>onu-1-3-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-3-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-3-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>onu-1-3-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>onu-1-3-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-slot-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
    </hardware>
    <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>cgroup-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-group</type>
        <enabled>true</enabled>
        <channel-group xmlns="urn:bbf:yang:bbf-xpon">
          <polling-period>100</polling-period>
          <raman-mitigation>raman-none</raman-mitigation>
          <system-id>00000</system-id>
          <pon-pools>
            <pon-pool>
              <name>pool-1-1-1</name>
              <channel-termination-ref>cterm-1-1-1</channel-termination-ref>
              <alloc-id-values>1024-1791</alloc-id-values>
              <gemport-values>1021-8957</gemport-values>
              <onu-id-values>0-255</onu-id-values>
            </pon-pool>
          </pon-pools>
        </channel-group>
      </interface>
      <interface>
        <name>cpart-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-partition</type>
        <enabled>true</enabled>
        <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
            <scheduling-level>1</scheduling-level>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>1</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>2</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>3</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>4</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>5</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>6</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>7</local-queue-id>
            </queue>
          </scheduler-node>
          <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
          </child-scheduler-nodes>
        </tm-root>
        <channel-partition xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-1-1</channel-group-ref>
          <channel-partition-index>1</channel-partition-index>
          <downstream-fec>false</downstream-fec>
          <closest-onu-distance>0</closest-onu-distance>
          <maximum-differential-xpon-distance>20</maximum-differential-xpon-distance>
          <authentication-method>serial-number</authentication-method>
          <multicast-aes-indicator>false</multicast-aes-indicator>
        </channel-partition>
      </interface>
      <interface>
        <name>cpair-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
        <enabled>true</enabled>
        <channel-pair xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-1-1</channel-group-ref>
          <channel-partition-ref>cpart-1-1-1</channel-partition-ref>
          <channel-pair-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-pair-type>
        </channel-pair>
      </interface>
      <interface>
        <name>cterm-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>olt-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <channel-pair-ref>cpair-1-1-1</channel-pair-ref>
          <channel-termination-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-termination-type>
          <xgs-pon-id>0</xgs-pon-id>
          <pon-tag>0000000000000000</pon-tag>
          <ber-calc-period>10</ber-calc-period>
          <location xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:inside-olt</location>
          <onu-phase-drift-monitoring-control>
            <monitoring-interval>1000</monitoring-interval>
            <lower-threshold>itut-recommended-value</lower-threshold>
            <upper-threshold>itut-recommended-value</upper-threshold>
          </onu-phase-drift-monitoring-control>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:los</notifiable-defect>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-in-o5</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-emergency-stopped</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-without-v-ani</notifiable-onu-presence-states>
        </channel-termination>
      </interface>
      <interface>
        <name>cgroup-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-group</type>
        <enabled>true</enabled>
        <channel-group xmlns="urn:bbf:yang:bbf-xpon">
          <polling-period>100</polling-period>
          <raman-mitigation>raman-none</raman-mitigation>
          <system-id>00000</system-id>
          <pon-pools>
            <pon-pool>
              <name>pool-1-9-1</name>
              <channel-termination-ref>cterm-1-9-1</channel-termination-ref>
              <alloc-id-values>1024-1791</alloc-id-values>
              <gemport-values>1021-8957</gemport-values>
              <onu-id-values>0-255</onu-id-values>
            </pon-pool>
          </pon-pools>
        </channel-group>
      </interface>
      <interface>
        <name>cpart-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-partition</type>
        <enabled>true</enabled>
        <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
            <scheduling-level>1</scheduling-level>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>1</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>2</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>3</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>4</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>5</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>6</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>7</local-queue-id>
            </queue>
          </scheduler-node>
          <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
          </child-scheduler-nodes>
        </tm-root>
        <channel-partition xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-1</channel-group-ref>
          <channel-partition-index>1</channel-partition-index>
          <downstream-fec>false</downstream-fec>
          <closest-onu-distance>0</closest-onu-distance>
          <maximum-differential-xpon-distance>20</maximum-differential-xpon-distance>
          <authentication-method>serial-number</authentication-method>
          <multicast-aes-indicator>false</multicast-aes-indicator>
        </channel-partition>
      </interface>
      <interface>
        <name>cpair-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
        <enabled>true</enabled>
        <channel-pair xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-1</channel-group-ref>
          <channel-partition-ref>cpart-1-9-1</channel-partition-ref>
          <channel-pair-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-pair-type>
        </channel-pair>
      </interface>
      <interface>
        <name>cterm-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>olt-transceiver-link-1-9-1</port-layer-if>
        </hardware-component>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <channel-pair-ref>cpair-1-9-1</channel-pair-ref>
          <channel-termination-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-termination-type>
          <xgs-pon-id>0</xgs-pon-id>
          <pon-tag>0000000000000000</pon-tag>
          <ber-calc-period>10</ber-calc-period>
          <location xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:inside-olt</location>
          <onu-phase-drift-monitoring-control>
            <monitoring-interval>1000</monitoring-interval>
            <lower-threshold>itut-recommended-value</lower-threshold>
            <upper-threshold>itut-recommended-value</upper-threshold>
          </onu-phase-drift-monitoring-control>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:los</notifiable-defect>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-in-o5</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-emergency-stopped</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-without-v-ani</notifiable-onu-presence-states>
        </channel-termination>
      </interface>
      <interface>
        <name>cgroup-1-9-2</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-group</type>
        <enabled>true</enabled>
        <channel-group xmlns="urn:bbf:yang:bbf-xpon">
          <polling-period>100</polling-period>
          <raman-mitigation>raman-none</raman-mitigation>
          <system-id>00000</system-id>
          <pon-pools>
            <pon-pool>
              <name>pool-1-9-2</name>
              <channel-termination-ref>cterm-1-9-2</channel-termination-ref>
              <alloc-id-values>256-767</alloc-id-values>
              <gemport-values>256-4092</gemport-values>
              <onu-id-values>0-127</onu-id-values>
            </pon-pool>
          </pon-pools>
        </channel-group>
      </interface>
      <interface>
        <name>cpart-1-9-2</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-partition</type>
        <enabled>true</enabled>
        <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
            <scheduling-level>1</scheduling-level>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>1</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>2</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>3</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>4</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>5</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>6</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>7</local-queue-id>
            </queue>
          </scheduler-node>
          <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
          </child-scheduler-nodes>
        </tm-root>
        <channel-partition xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-2</channel-group-ref>
          <channel-partition-index>1</channel-partition-index>
          <downstream-fec>false</downstream-fec>
          <closest-onu-distance>0</closest-onu-distance>
          <maximum-differential-xpon-distance>20</maximum-differential-xpon-distance>
          <authentication-method>serial-number</authentication-method>
          <multicast-aes-indicator>false</multicast-aes-indicator>
        </channel-partition>
      </interface>
      <interface>
        <name>cpair-1-9-2</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
        <enabled>true</enabled>
        <channel-pair xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-2</channel-group-ref>
          <channel-partition-ref>cpart-1-9-2</channel-partition-ref>
          <channel-pair-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:gpon</channel-pair-type>
          <gpon-pon-id-interval>0</gpon-pon-id-interval>
        </channel-pair>
      </interface>
      <interface>
        <name>cterm-1-9-2</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>olt-transceiver-link-1-9-2</port-layer-if>
        </hardware-component>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <channel-pair-ref>cpair-1-9-2</channel-pair-ref>
          <channel-termination-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:gpon</channel-termination-type>
          <gpon-pon-id>00000000000000</gpon-pon-id>
          <ber-calc-period>10</ber-calc-period>
          <location xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:inside-olt</location>
          <onu-phase-drift-monitoring-control>
            <monitoring-interval>1000</monitoring-interval>
            <lower-threshold>itut-recommended-value</lower-threshold>
            <upper-threshold>itut-recommended-value</upper-threshold>
          </onu-phase-drift-monitoring-control>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:los</notifiable-defect>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-in-o5</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-emergency-stopped</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-without-v-ani</notifiable-onu-presence-states>
        </channel-termination>
      </interface>
      <interface>
        <name>nni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-1-2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-1-2</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-1-3</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-1-3</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-1-4</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-1-4</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-2-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-2-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-2-2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-2-2</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-2-3</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-2-3</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-2-4</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-2-4</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-3-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-3-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-3-2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-3-2</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-3-3</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-3-3</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-3-4</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-3-4</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-4-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-4-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-4-2</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-4-2</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-4-3</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-4-3</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-4-4</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-4-4</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>LAG-1</name>
        <description>NNI_LAG</description>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ieee8023adLag</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregator xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <name>AGG-LAG-1</name>
          <agg-system-name>STATIC-LAG-SYSTEM</agg-system-name>
          <admin-state>up</admin-state>
          <link-up-down-notification>enabled</link-up-down-notification>
          <aggregator-lacp>
            <actor-admin-key>1</actor-admin-key>
          </aggregator-lacp>
        </aggregator>
      </interface>
      <interface>
        <name>ONU-1-1-ani-2-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-1-ani-transceiver-link-2-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>1</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-1-1_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>1</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>ADTN2017E038</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-1-1-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-1-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-1_venet_uni-port-1-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-1-1_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-1-1_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1_venet_uni-port-1-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_uni-port-1-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1-uni-1-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>101</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-2-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-2-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>2</onu-id>
        </ani>
      </interface>
      <interface>
        <name>onu-1-2_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>2</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>CIGG21B0126A</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>onu-1-2-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-2-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>onu-1-2_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>onu-1-2_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>onu-1-2_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-2_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>onu-1-2_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-2_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>onu-1-2-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>101</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-3-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-3-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>3</onu-id>
        </ani>
      </interface>
      <interface>
        <name>onu-1-3_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>3</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>CIGG21B012F4</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>true</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>onu-1-3-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-3-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>onu-1-3_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>onu-1-3_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>onu-1-3_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-3_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>onu-1-3_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-3_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>onu-1-3-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>101</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>onu-1-2-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-2-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>onu-1-3-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>onu-1-3-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
    </interfaces>
    <keystore xmlns="urn:ietf:params:xml:ns:yang:ietf-keystore">
      <asymmetric-keys>
        <asymmetric-key>
          <name>genkey</name>
          <public-key-format xmlns:ct="urn:ietf:params:xml:ns:yang:ietf-crypto-types">ct:ssh-public-key-format</public-key-format>
          <public-key>MIIBigKCAYEAmtD8OprD9yx6576fBAHr+7Y+B2qUZB0S6pvT2qm3vP8AjkCQi7rGWRUYSwKQ+wt3iW0sFKXmtehXsKZop1E52Tyja7o4VAfmqcRLYzsjJe9k66i3bhL50jNw9mfOC6D/0qliqZ/+cu/aiq/TV8HBm9c1ZbgclTY8H+2M0W/gDAjAyeS3WSdYXkfiJOEdtSNaQ3ZKAa1bS7Y5TqUFtuvXLUEylCjsZkXwM7JT7PVifoAp6nnckjAz6o+rGPGGwUfzv8Qi4lgaqFkGC79HuEjE4moNyWmJuyg7mwbxTmsA4+gjpB13UYc1CYjd0GcvKzUYBdVccK7ca+fEFZNr+hjZwwWAQH3es381ef8lsgxrc1FY3gnHLGyYZxyfbQ/ilh4DEBKYD9Gb1WqkfZmpmuhh2a51LL0iqrfTUnu77WMfuUM42bDrqrGKDiQMH3NlQzV7hU69FZGsDZYHKGyct+cs+jHj00JiAILSY/XZv5MzRoPivfFQidNPohF2j6nTtjPbAgMBAAE=</public-key>
          <private-key-format xmlns:ct="urn:ietf:params:xml:ns:yang:ietf-crypto-types">ct:rsa-private-key-format</private-key-format>
          <cleartext-private-key>MIIG4wIBAAKCAYEAmtD8OprD9yx6576fBAHr+7Y+B2qUZB0S6pvT2qm3vP8AjkCQi7rGWRUYSwKQ+wt3iW0sFKXmtehXsKZop1E52Tyja7o4VAfmqcRLYzsjJe9k66i3bhL50jNw9mfOC6D/0qliqZ/+cu/aiq/TV8HBm9c1ZbgclTY8H+2M0W/gDAjAyeS3WSdYXkfiJOEdtSNaQ3ZKAa1bS7Y5TqUFtuvXLUEylCjsZkXwM7JT7PVifoAp6nnckjAz6o+rGPGGwUfzv8Qi4lgaqFkGC79HuEjE4moNyWmJuyg7mwbxTmsA4+gjpB13UYc1CYjd0GcvKzUYBdVccK7ca+fEFZNr+hjZwwWAQH3es381ef8lsgxrc1FY3gnHLGyYZxyfbQ/ilh4DEBKYD9Gb1WqkfZmpmuhh2a51LL0iqrfTUnu77WMfuUM42bDrqrGKDiQMH3NlQzV7hU69FZGsDZYHKGyct+cs+jHj00JiAILSY/XZv5MzRoPivfFQidNPohF2j6nTtjPbAgMBAAECggGASf5Wf0AXJ1zoBTkzUTwF6NFqhirnb44By4Xc1KbHPZp3ToYHT/Fd+Ze+e6NnXcVWRaWbKuc8BHde6fwvCsEkr/JufP+NCoSYN02tZmkOXIQ1rPh/aynAozmY5PwqG57AhpQUptPkTlTbE+wDS+88NNrAF7TOXHaGeBAWfMdGwxmv4w7gnsjSUIV0zYGWrEuQSawQpQqRFveqHi97Mrk3p9aAcRW6HwuQSXUS7a8+ew2QexPxWyGUvqgZ85sEd/6GACxvb7aH6OQNrIe3B7Gd7ckakx9bmzB5vYhvDf1KEKzQhZSJqQ4a6MEr+uYKPEbV881eDCWlNCQ/4LvHJbRAoAvtjRrIGLoEmMgHY5rNV7S5kkEdlZMb9TxNxBTN0nDOI3gEXusvbPuKeaSQb9ODX+cBfdcdRJ3fgPUEgFvHtVoh2mwuJs07pEGTJXO9/1JXYD1Y4pjvdH1MaHIvA2Oe1+4NOSvR5FBp51HAkhwUf3F+V92WITLPvo1DRmSWczChAoHBAMr1K1S+fKk0MDWq++uUGzT8nnFRy2zu4UkbGdDTBYrEo1Je+iy5TX/H2DPeg1SoczsXIVjY7O1SkTvfSs2Kg11qq6Og1r0XmEKXc6MYgdixWey6k3mM/qTDO9tPBqO0XoowBzE7oFCx24thMYM37kvR33B9JT/yRxRfnkpJK3hXRUDBdMhoP24RQJob3BkkeLRgVEW1NbFEQrpT60fcJrClHeCROThb6HGkrHa+d2YKnjDgh7SsnrDFU4PuK32bMQKBwQDDRu6giXZhDj2itZmmwwR7vGYMtu3ohQrtpXwMdAuMFNZijib9AREwOi/8xyK8JnsBwJgSc8aqE0gsdQcMq5PUh/qZo/uJGW3wEzHHiU43bgpxKz4Smu+H8YT6H/nsU699IWzPpkB/FTE5Cdu92mKK/Wdx/GhBamT3CskTvwvbGtqsddBrFM9EgJxvwAGkAONpPFZK5+Ek9UiUdItv6DjwQOk6iBLWJDs6ciGGWfBso58csmMwG6Ai123fInUNZMsCgcEAkTeS5XPWZor846mPzyONw//srlBEKZFSiKhndE9I691+rnVes5lypjcrrxFLDsvohyMprRSpkbU+TYSbVS4CiFjGrrFqdKnpO9x51Py4C3/6Q7PLyXDk0qcOsQB+U6u+6UksHEH5l0NrPvMwJh9i1cU5BpfEi1ijGyS/cY+hFt36ozbIhIxytiKKArpkZWj/JLC4G5ho7olU5VUeR7BxznqWQhQmyPiZ/JZDAEOP0udOANLmxpOsh/bopsFHRPxBAoHAMi7mYCczXtnUCR52MB7p5gqShy3zkc+u8UeXy3N/DC7GsWkqp9ZAXo51ipZ6XLPe5KJj8koCge6Wm6Yve5gUU4fmZNl5aNA6KnokTs0AZspGsLKWLx3V9K+ipszU42DWNmgCmJJ2/LGrhqb765xVurZIgUiGWllHPR1ucz6jg1kxXSShvQMKCOasTSOgyE7aIk85NeLFP0QxtMUGmGmrSELGLR6PCK0i83AlIWu3l5Os7ikByHkw/AM03yTxw9FlAoHAc9VEicZJPBSAEj0THDeRn08TzYiuOmZ7ICD5cPFhlWHmQaq/raSW3BtNDO32LNmygj5MQXWtbl1XGNTYYKFRS0smUEfrUYAHILaPipz+gY0HhKGrzXRpdRXSzVmWZ1LLrE9C72IGRbkRPF4w9NhOy5/eT/5U5Jsg3Da6W9VlrTlWuFFa92KT7u//qnzlmRmgj53rt8CX4YVuVnzhfOMpGboTiOgUNvto1bc9l3XQb7z9dme5SvMUL8nyWKZ1+wdk</cleartext-private-key>
        </asymmetric-key>
      </asymmetric-keys>
    </keystore>
    <nacm xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-acm">
      <enable-nacm>false</enable-nacm>
    </nacm>
    <netconf-server xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-server">
      <listen>
        <endpoint>
          <name>default-ssh</name>
          <ssh>
            <tcp-server-parameters>
              <local-address>0.0.0.0</local-address>
              <local-port>10830</local-port>
              <keepalives>
                <idle-time>1</idle-time>
                <max-probes>10</max-probes>
                <probe-interval>5</probe-interval>
              </keepalives>
            </tcp-server-parameters>
            <ssh-server-parameters>
              <server-identity>
                <host-key>
                  <name>default-key</name>
                  <public-key>
                    <keystore-reference>genkey</keystore-reference>
                  </public-key>
                </host-key>
              </server-identity>
              <client-authentication>
                <supported-authentication-methods>
                  <publickey/>
                  <password/>
                </supported-authentication-methods>
              </client-authentication>
            </ssh-server-parameters>
          </ssh>
        </endpoint>
      </listen>
    </netconf-server>
    <system xmlns="urn:ietf:params:xml:ns:yang:ietf-system">
      <contact>Tech support</contact>
      <hostname>gregson</hostname>
      <location>local</location>
      <subscriber-management xmlns="urn:bbf:yang:bbf-subscriber-profiles">
        <access-node-id>gregson</access-node-id>
      </subscriber-management>
    </system>
  </data>
</rpc-reply>
```

Once ONUs are connected, powered-on and once ranged with the OLT, the OLT will generate presence detection notifications:

```xml
<netconf-events>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:39:28.507431217+00:00</eventTime>
    <replayComplete xmlns="urn:ietf:params:xml:ns:netmod:notification"/>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:43:07.460606626+00:00</eventTime>
    <netconf-session-start xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-notifications">
      <username>root</username>
      <session-id>3</session-id>
      <source-host>192.168.128.1</source-host>
    </netconf-session-start>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:43:08.548502748+00:00</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>cterm-1-1-1</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>CIGG21B012F4</detected-serial-number>
            <last-change>2023-04-28T02:43:08+00:00</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</onu-presence-state>
            <onu-id>3</onu-id>
            <detected-registration-id>000000000000000000000000000000000000000000000000000000000000000000000000</detected-registration-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:43:08.426529201+00:00</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>cterm-1-1-1</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>CIGG21B0126A</detected-serial-number>
            <last-change>2023-04-28T02:43:08+00:00</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</onu-presence-state>
            <onu-id>2</onu-id>
            <detected-registration-id>000000000000000000000000000000000000000000000000000000000000000000000000</detected-registration-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:43:08.304348610+00:00</eventTime>
    <interfaces-state xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>cterm-1-1-1</name>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <onu-presence-state-change xmlns="urn:bbf:yang:bbf-xpon-onu-state">
            <detected-serial-number>ADTN2017E038</detected-serial-number>
            <last-change>2023-04-28T02:43:08+00:00</last-change>
            <onu-presence-state xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</onu-presence-state>
            <onu-id>1</onu-id>
            <detected-registration-id>123456780000000000000000000000000000000000000000000000000000000000000000</detected-registration-id>
          </onu-presence-state-change>
        </channel-termination>
      </interface>
    </interfaces-state>
  </notification>
  <notification xmlns="urn:ietf:params:xml:ns:netconf:notification:1.0">
    <eventTime>2023-04-28T02:43:11.028547848+00:00</eventTime>
    <netconf-session-end xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-notifications">
      <username>root</username>
      <session-id>3</session-id>
      <source-host>192.168.128.1</source-host>
      <termination-reason>closed</termination-reason>
    </netconf-session-end>
  </notification>
</netconf-events>
```

## 7.22. Example Full Complex Configuration

> **Mode:** Combined-NE
> **Use Case:** NETCONF get-config RPC requested by client from server running datastore

```xml
<rpc-reply xmlns="urn:ietf:params:xml:ns:netconf:base:1.0" message-id="get-running-10">
  <data>
    <l2-dhcpv4-relay-profiles xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
      <l2-dhcpv4-relay-profile>
        <name>DHCP_Default_DoubleTag</name>
        <max-packet-size>1500</max-packet-size>
        <option82-format>
          <suboptions>circuit-id</suboptions>
          <default-circuit-id-syntax>baskerville eth Slot/Port:N-VID:N2VID:ONUID</default-circuit-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </option82-format>
      </l2-dhcpv4-relay-profile>
      <l2-dhcpv4-relay-profile>
        <name>DHCP_Default_SingleTag</name>
        <max-packet-size>1500</max-packet-size>
        <option82-format>
          <suboptions>circuit-id</suboptions>
          <default-circuit-id-syntax>baskerville eth Slot/Port:N-VID:ONUID</default-circuit-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </option82-format>
      </l2-dhcpv4-relay-profile>
    </l2-dhcpv4-relay-profiles>
    <forwarding xmlns="urn:bbf:yang:bbf-l2-forwarding">
      <forwarders>
        <forwarder>
          <name>ONU-1-1_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-1-1_fwd_user_port_1</name>
              <sub-interface>ONU-1-1_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-1_fwd_net_port_1</name>
              <sub-interface>ONU-1-1_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_1001</name>
          <ports>
            <port>
              <name>ONU-1-1_fwd_user_port_2</name>
              <sub-interface>ONU-1-1_venet_vlan_2_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_1001</name>
              <sub-interface>olt_uplink_nx1_vlan_1001</sub-interface>
            </port>
            <port>
              <name>ONU-1-2_fwd_user_port_2</name>
              <sub-interface>ONU-1-2_venet_vlan_2_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-3_fwd_user_port_2</name>
              <sub-interface>ONU-1-3_venet_vlan_2_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-4_fwd_user_port_2</name>
              <sub-interface>ONU-1-4_venet_vlan_2_user</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_1001</forwarding-database>
          </mac-learning>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_1002</name>
          <ports>
            <port>
              <name>ONU-1-1_fwd_user_port_3</name>
              <sub-interface>ONU-1-1_venet_vlan_3_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_1002</name>
              <sub-interface>olt_uplink_nx1_vlan_1002</sub-interface>
            </port>
            <port>
              <name>ONU-1-2_fwd_user_port_3</name>
              <sub-interface>ONU-1-2_venet_vlan_3_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-3_fwd_user_port_3</name>
              <sub-interface>ONU-1-3_venet_vlan_3_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-4_fwd_user_port_3</name>
              <sub-interface>ONU-1-4_venet_vlan_3_user</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_1002</forwarding-database>
          </mac-learning>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_501</name>
          <ports>
            <port>
              <name>ONU-1-1_fwd_user_port_4</name>
              <sub-interface>ONU-1-1_venet_vlan_4_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_501</name>
              <sub-interface>olt_uplink_nx1_vlan_501</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_501</forwarding-database>
          </mac-learning>
        </forwarder>
        <forwarder>
          <name>ONU-1-2_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-1-2_fwd_user_port_1</name>
              <sub-interface>ONU-1-2_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-2_fwd_net_port_1</name>
              <sub-interface>ONU-1-2_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>ONU-1-3_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-1-3_fwd_user_port_1</name>
              <sub-interface>ONU-1-3_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-3_fwd_net_port_1</name>
              <sub-interface>ONU-1-3_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>ONU-1-4_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-1-4_fwd_user_port_1</name>
              <sub-interface>ONU-1-4_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-1-4_fwd_net_port_1</name>
              <sub-interface>ONU-1-4_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_500</name>
          <ports>
            <port>
              <name>ONU-1-4_fwd_user_port_4</name>
              <sub-interface>ONU-1-4_venet_vlan_4_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_500</name>
              <sub-interface>olt_uplink_nx1_vlan_500</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_500</forwarding-database>
          </mac-learning>
        </forwarder>
        <forwarder>
          <name>ONU-17-1_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-17-1_fwd_user_port_1</name>
              <sub-interface>ONU-17-1_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-17-1_fwd_net_port_1</name>
              <sub-interface>ONU-17-1_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>ONU-17-2_forwarder_1</name>
          <ports>
            <port>
              <name>ONU-17-2_fwd_user_port_1</name>
              <sub-interface>ONU-17-2_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>ONU-17-2_fwd_net_port_1</name>
              <sub-interface>ONU-17-2_olt_uplink_vlan1_network</sub-interface>
            </port>
          </ports>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_3000</name>
          <ports>
            <port>
              <name>ONU-17-3_fwd_user_port_1</name>
              <sub-interface>ONU-17-3_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_3000</name>
              <sub-interface>olt_uplink_nx1_vlan_3000</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_3000</forwarding-database>
          </mac-learning>
        </forwarder>
        <forwarder>
          <name>forwarder_nx1_vlan_4000</name>
          <ports>
            <port>
              <name>ONU-17-4_fwd_user_port_1</name>
              <sub-interface>ONU-17-4_venet_vlan_1_user</sub-interface>
            </port>
            <port>
              <name>fwd_nx1_net_port_vlan_4000</name>
              <sub-interface>olt_uplink_nx1_vlan_4000</sub-interface>
            </port>
          </ports>
          <mac-learning>
            <forwarding-database>forwarder_db_nx1_vlan_4000</forwarding-database>
          </mac-learning>
        </forwarder>
      </forwarders>
      <forwarding-databases>
        <forwarding-database>
          <name>baskerville_ALLONUS_DEFAULT</name>
          <max-number-mac-addresses>5</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_1001</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_1002</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_501</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_500</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_3000</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
        <forwarding-database>
          <name>forwarder_db_nx1_vlan_4000</name>
          <max-number-mac-addresses>16384</max-number-mac-addresses>
          <aging-timer>300</aging-timer>
          <shared-forwarding-database>false</shared-forwarding-database>
        </forwarding-database>
      </forwarding-databases>
    </forwarding>
    <dhcpv6-ldra-profiles xmlns="urn:bbf:yang:bbf-ldra">
      <dhcpv6-ldra-profile>
        <name>DHCPv6_Default_DoubleTag</name>
        <options>
          <option>interface-id</option>
          <option>subscriber-id</option>
          <option>enterprise-number</option>
          <option>vendor-specific-information</option>
          <default-interface-id-syntax>baskerville eth Slot/Port:N-VID:N2VID:ONUID</default-interface-id-syntax>
          <enterprise-number>23330</enterprise-number>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </options>
      </dhcpv6-ldra-profile>
      <dhcpv6-ldra-profile>
        <name>DHCPv6_Default_SingleTag</name>
        <options>
          <option>interface-id</option>
          <option>subscriber-id</option>
          <option>enterprise-number</option>
          <option>vendor-specific-information</option>
          <default-interface-id-syntax>baskerville eth Slot/Port:N-VID:ONUID</default-interface-id-syntax>
          <enterprise-number>23330</enterprise-number>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </options>
      </dhcpv6-ldra-profile>
    </dhcpv6-ldra-profiles>
    <link-table xmlns="urn:bbf:yang:bbf-link-table">
      <link-table>
        <from-interface>ONU-1-1-ani-1-1-1</from-interface>
        <to-interface>ONU-1-1_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-1-uni-10-1-1</from-interface>
        <to-interface>ONU-1-1_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-2-ani-1-1-1</from-interface>
        <to-interface>ONU-1-2_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-2-uni-10-1-1</from-interface>
        <to-interface>ONU-1-2_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-3-ani-1-1-1</from-interface>
        <to-interface>ONU-1-3_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-3-uni-10-1-1</from-interface>
        <to-interface>ONU-1-3_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-4-ani-1-1-1</from-interface>
        <to-interface>ONU-1-4_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-1-4-uni-10-1-1</from-interface>
        <to-interface>ONU-1-4_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-1-ani-1-1-1</from-interface>
        <to-interface>ONU-17-1_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-1-uni-10-1-1</from-interface>
        <to-interface>ONU-17-1_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-2-ani-1-1-1</from-interface>
        <to-interface>ONU-17-2_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-2-uni-10-1-1</from-interface>
        <to-interface>ONU-17-2_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-3-ani-1-1-1</from-interface>
        <to-interface>ONU-17-3_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-3-uni-10-1-1</from-interface>
        <to-interface>ONU-17-3_venet_uni-port-10-1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-4-ani-1-1-1</from-interface>
        <to-interface>ONU-17-4_vAni_slot1_port1_if</to-interface>
      </link-table>
      <link-table>
        <from-interface>ONU-17-4-uni-10-1-1</from-interface>
        <to-interface>ONU-17-4_venet_uni-port-10-1_if</to-interface>
      </link-table>
    </link-table>
    <multicast xmlns="urn:bbf:yang:bbf-mgmd">
      <mgmd>
        <global-administrative-enabled>true</global-administrative-enabled>
        <multicast-vpn>
          <name>MGMD-VPN-501</name>
          <mode xmlns:bbf-mgmd="urn:bbf:yang:bbf-mgmd">bbf-mgmd:snoop-transparent</mode>
          <ip-version>ipv4</ip-version>
          <multicast-interface-to-host>
            <name>ONU-1-1_MCAST_HOST_INTF_501</name>
            <vlan-sub-interface>ONU-1-1_venet_vlan_4_user</vlan-sub-interface>
            <multicast-package>upper-range</multicast-package>
          </multicast-interface-to-host>
          <multicast-network-interface>
            <name>MCAST_NETWORK_INTF_501</name>
            <single-uplink-interface-data>
              <vlan-sub-interface>olt_uplink_nx1_vlan_501</vlan-sub-interface>
            </single-uplink-interface-data>
          </multicast-network-interface>
          <multicast-channel>
            <name>ASM_100</name>
            <network-interface>MCAST_NETWORK_INTF_501</network-interface>
            <ipv4>
              <source-ipv4-address>0.0.0.0</source-ipv4-address>
              <group-ipv4-address>239.255.255.253</group-ipv4-address>
            </ipv4>
          </multicast-channel>
          <multicast-channel>
            <name>ASM_101</name>
            <network-interface>MCAST_NETWORK_INTF_501</network-interface>
            <ipv4>
              <source-ipv4-address>0.0.0.0</source-ipv4-address>
              <group-ipv4-address>239.255.255.252</group-ipv4-address>
            </ipv4>
          </multicast-channel>
          <multicast-package>
            <name>upper-range</name>
            <multicast-channel-admission-control>
              <multicast-channel-name>ASM_100</multicast-channel-name>
            </multicast-channel-admission-control>
            <multicast-channel-admission-control>
              <multicast-channel-name>ASM_101</multicast-channel-name>
            </multicast-channel-admission-control>
          </multicast-package>
        </multicast-vpn>
        <multicast-vpn>
          <name>MGMD-VPN-500</name>
          <mode xmlns:bbf-mgmd="urn:bbf:yang:bbf-mgmd">bbf-mgmd:snoop-transparent</mode>
          <ip-version>ipv4</ip-version>
          <multicast-interface-to-host>
            <name>ONU-1-4_MCAST_HOST_INTF_500</name>
            <vlan-sub-interface>ONU-1-4_venet_vlan_4_user</vlan-sub-interface>
            <multicast-package>lower-range</multicast-package>
          </multicast-interface-to-host>
          <multicast-network-interface>
            <name>MCAST_NETWORK_INTF_500</name>
            <single-uplink-interface-data>
              <vlan-sub-interface>olt_uplink_nx1_vlan_500</vlan-sub-interface>
            </single-uplink-interface-data>
          </multicast-network-interface>
          <multicast-channel>
            <name>ASM_100</name>
            <network-interface>MCAST_NETWORK_INTF_500</network-interface>
            <ipv4>
              <source-ipv4-address>0.0.0.0</source-ipv4-address>
              <group-ipv4-address>225.0.0.1</group-ipv4-address>
            </ipv4>
          </multicast-channel>
          <multicast-channel>
            <name>ASM_101</name>
            <network-interface>MCAST_NETWORK_INTF_500</network-interface>
            <ipv4>
              <source-ipv4-address>0.0.0.0</source-ipv4-address>
              <group-ipv4-address>225.0.0.2</group-ipv4-address>
            </ipv4>
          </multicast-channel>
          <multicast-package>
            <name>lower-range</name>
            <multicast-channel-admission-control>
              <multicast-channel-name>ASM_100</multicast-channel-name>
            </multicast-channel-admission-control>
            <multicast-channel-admission-control>
              <multicast-channel-name>ASM_101</multicast-channel-name>
            </multicast-channel-admission-control>
          </multicast-package>
        </multicast-vpn>
      </mgmd>
    </multicast>
    <pppoe-profiles xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
      <pppoe-profile>
        <name>PPPoE_doubleTag_remoteID</name>
        <pppoe-vendor-specific-tag>
          <subtag>remote-id</subtag>
          <default-circuit-id-syntax/>
          <default-remote-id-syntax>baskerville eth Slot/Port/ONUID:S-VID:C-VID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
      <pppoe-profile>
        <name>PPPoE_doubleTag_circuitID</name>
        <pppoe-vendor-specific-tag>
          <subtag>circuit-id</subtag>
          <default-circuit-id-syntax>baskerville eth Slot/Port/ONUID:N-VID:N2VID</default-circuit-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
      <pppoe-profile>
        <name>PPPoE_singleTag_remoteID</name>
        <pppoe-vendor-specific-tag>
          <subtag>remote-id</subtag>
          <default-circuit-id-syntax/>
          <default-remote-id-syntax>baskerville eth Slot/Port/ONUID:S-VID</default-remote-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
      <pppoe-profile>
        <name>PPPoE_singleTag_circuitID</name>
        <pppoe-vendor-specific-tag>
          <subtag>circuit-id</subtag>
          <default-circuit-id-syntax>baskerville eth Slot/Port/ONUID:N-VID</default-circuit-id-syntax>
          <start-numbering-from-zero>false</start-numbering-from-zero>
          <use-leading-zeroes>false</use-leading-zeroes>
        </pppoe-vendor-specific-tag>
      </pppoe-profile>
    </pppoe-profiles>
    <classifiers xmlns="urn:bbf:yang:bbf-qos-classifiers">
      <classifier-entry>
        <name>onuIngressDefault_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>1</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>3</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>3</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier4</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>4</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>4</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier5</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>5</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier6</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>6</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>6</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngressDefault_classifier7</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>7</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0-1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2-4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5-6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_4class_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>onuIngress_1class_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0-7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>LGI_Test_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0-3</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>LGI_Test_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>4-5</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>4</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>LGI_Test_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>6-7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>6</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier0</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>0</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>0</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>0</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier1</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>1</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>1</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>1</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier2</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>2</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>2</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>2</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier3</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>3</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>3</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>3</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier4</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>4</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>4</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>4</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier5</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>5</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>5</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>5</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier6</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>6</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>6</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>6</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
      <classifier-entry>
        <name>QPP0_classifier7</name>
        <filter-operation xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:match-all-filter</filter-operation>
        <match-criteria>
          <tag>
            <index>0</index>
            <in-pbit-list>7</in-pbit-list>
          </tag>
          <dscp-range>any</dscp-range>
          <any-protocol/>
        </match-criteria>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:scheduling-traffic-class</action-type>
          <scheduling-traffic-class>7</scheduling-traffic-class>
        </classifier-action-entry-cfg>
        <classifier-action-entry-cfg>
          <action-type xmlns:bbf-qos-cls="urn:bbf:yang:bbf-qos-classifiers">bbf-qos-cls:pbit-marking</action-type>
          <pbit-marking-cfg>
            <pbit-marking-list>
              <index>0</index>
              <pbit-value>7</pbit-value>
            </pbit-marking-list>
          </pbit-marking-cfg>
        </classifier-action-entry-cfg>
      </classifier-entry>
    </classifiers>
    <policies xmlns="urn:bbf:yang:bbf-qos-policies">
      <policy>
        <name>onuIngressDefault_policy</name>
        <classifiers>
          <name>onuIngressDefault_classifier0</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier1</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier2</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier3</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier4</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier5</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier6</name>
        </classifiers>
        <classifiers>
          <name>onuIngressDefault_classifier7</name>
        </classifiers>
      </policy>
      <policy>
        <name>onuIngress_4class_policy</name>
        <classifiers>
          <name>onuIngress_4class_classifier0</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier1</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier2</name>
        </classifiers>
        <classifiers>
          <name>onuIngress_4class_classifier3</name>
        </classifiers>
      </policy>
      <policy>
        <name>onuIngress_1class_policy</name>
        <classifiers>
          <name>onuIngress_1class_classifier0</name>
        </classifiers>
      </policy>
      <policy>
        <name>LGI_Test_policy</name>
        <classifiers>
          <name>LGI_Test_classifier0</name>
        </classifiers>
        <classifiers>
          <name>LGI_Test_classifier1</name>
        </classifiers>
        <classifiers>
          <name>LGI_Test_classifier2</name>
        </classifiers>
      </policy>
      <policy>
        <name>QPP0_policy</name>
        <classifiers>
          <name>QPP0_classifier0</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier1</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier2</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier3</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier4</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier5</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier6</name>
        </classifiers>
        <classifiers>
          <name>QPP0_classifier7</name>
        </classifiers>
      </policy>
    </policies>
    <qos-policy-profiles xmlns="urn:bbf:yang:bbf-qos-policies">
      <policy-profile>
        <name>onuIngressDefault_profile</name>
        <policy-list>
          <name>onuIngressDefault_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>onuIngress_4class_profile</name>
        <policy-list>
          <name>onuIngress_4class_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>onuIngress_1class_profile</name>
        <policy-list>
          <name>onuIngress_1class_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>LGI_Test_profile</name>
        <policy-list>
          <name>LGI_Test_policy</name>
        </policy-list>
      </policy-profile>
      <policy-profile>
        <name>QPP0_profile</name>
        <policy-list>
          <name>QPP0_policy</name>
        </policy-list>
      </policy-profile>
    </qos-policy-profiles>
    <tm-profiles xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-100Mbps-pbs-10MB</name>
        <single-token-bucket>
          <pir>100000</pir>
          <pbs>10000000</pbs>
        </single-token-bucket>
      </shaper-profile>
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-1Gbps-pbs-100MB</name>
        <single-token-bucket>
          <pir>1000000</pir>
          <pbs>100000000</pbs>
        </single-token-bucket>
      </shaper-profile>
      <shaper-profile xmlns="urn:bbf:yang:bbf-qos-shaping">
        <name>shaper-pir-200Mbps-pbs-20MB</name>
        <single-token-bucket>
          <pir>200000</pir>
          <pbs>20000000</pbs>
        </single-token-bucket>
      </shaper-profile>
    </tm-profiles>
    <xpon xmlns="urn:bbf:yang:bbf-xpon">
      <multicast-distribution-set>
        <multicast-set>
          <name>mc_nx1_vlan_set_CT_1</name>
          <multicast-gemport-ref>mc_gem_CT_1</multicast-gemport-ref>
          <vlan-list>
            <multicast-vlan-id>1001</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>1002</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>501</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>500</multicast-vlan-id>
          </vlan-list>
        </multicast-set>
        <multicast-set>
          <name>iptv_mc_nx1_vlan_set_CT_1</name>
          <multicast-gemport-ref>iptv_mc_gem_CT_1</multicast-gemport-ref>
          <vlan-list>
            <multicast-vlan-id>1001</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>1002</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>501</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>500</multicast-vlan-id>
          </vlan-list>
        </multicast-set>
        <multicast-set>
          <name>mc_nx1_vlan_set_CT_17</name>
          <multicast-gemport-ref>mc_gem_CT_17</multicast-gemport-ref>
          <vlan-list>
            <multicast-vlan-id>3000</multicast-vlan-id>
          </vlan-list>
          <vlan-list>
            <multicast-vlan-id>4000</multicast-vlan-id>
          </vlan-list>
        </multicast-set>
      </multicast-distribution-set>
      <multicast-gemports>
        <multicast-gemport>
          <name>mc_gem_CT_1</name>
          <gemport-id>2001</gemport-id>
          <interface>cpair-1-1-1</interface>
          <traffic-class>0</traffic-class>
          <is-broadcast>true</is-broadcast>
        </multicast-gemport>
        <multicast-gemport>
          <name>iptv_mc_gem_CT_1</name>
          <gemport-id>2000</gemport-id>
          <interface>cpair-1-1-1</interface>
          <traffic-class>0</traffic-class>
          <is-broadcast>false</is-broadcast>
        </multicast-gemport>
        <multicast-gemport>
          <name>mc_gem_CT_17</name>
          <gemport-id>2001</gemport-id>
          <interface>cpair-1-9-1</interface>
          <traffic-class>0</traffic-class>
          <is-broadcast>true</is-broadcast>
        </multicast-gemport>
      </multicast-gemports>
    </xpon>
    <xpongemtcont xmlns="urn:bbf:yang:bbf-xpongemtcont">
      <traffic-descriptor-profiles>
        <traffic-descriptor-profile>
          <name>TDP_XGS_DEFAULT</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>9950000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>TDP_GPON_DEFAULT</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>1200000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>LGI_Internet_1G</name>
          <fixed-bandwidth>100000000</fixed-bandwidth>
          <assured-bandwidth>100000000</assured-bandwidth>
          <maximum-bandwidth>1000000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>LGI_Voice</name>
          <fixed-bandwidth>1000000</fixed-bandwidth>
          <assured-bandwidth>1000000</assured-bandwidth>
          <maximum-bandwidth>10000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>LGI_RG_Mgmt</name>
          <fixed-bandwidth>1000000</fixed-bandwidth>
          <assured-bandwidth>1000000</assured-bandwidth>
          <maximum-bandwidth>10000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>LGI_Internet_100M</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>100000000</maximum-bandwidth>
        </traffic-descriptor-profile>
        <traffic-descriptor-profile>
          <name>LGI_inernet_10G</name>
          <fixed-bandwidth>10000000</fixed-bandwidth>
          <assured-bandwidth>10000000</assured-bandwidth>
          <maximum-bandwidth>9950000000</maximum-bandwidth>
        </traffic-descriptor-profile>
      </traffic-descriptor-profiles>
      <tconts>
        <tcont>
          <name>ONU-1-1_tcont_0</name>
          <alloc-id>1024</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Internet_1G</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-1_tcont_1</name>
          <alloc-id>1025</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Voice</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-1_tcont_2</name>
          <alloc-id>1026</alloc-id>
          <interface-reference>ONU-1-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_RG_Mgmt</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-2_tcont_0</name>
          <alloc-id>1040</alloc-id>
          <interface-reference>ONU-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_RG_Mgmt</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-2_tcont_1</name>
          <alloc-id>1041</alloc-id>
          <interface-reference>ONU-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Voice</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-2_tcont_2</name>
          <alloc-id>1042</alloc-id>
          <interface-reference>ONU-1-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_RG_Mgmt</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-3_tcont_0</name>
          <alloc-id>1048</alloc-id>
          <interface-reference>ONU-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Internet_100M</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-3_tcont_1</name>
          <alloc-id>1049</alloc-id>
          <interface-reference>ONU-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Voice</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-3_tcont_2</name>
          <alloc-id>1050</alloc-id>
          <interface-reference>ONU-1-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_RG_Mgmt</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-4_tcont_0</name>
          <alloc-id>1056</alloc-id>
          <interface-reference>ONU-1-4_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Internet_1G</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-4_tcont_1</name>
          <alloc-id>1057</alloc-id>
          <interface-reference>ONU-1-4_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Voice</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-1-4_tcont_2</name>
          <alloc-id>1058</alloc-id>
          <interface-reference>ONU-1-4_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_RG_Mgmt</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-17-1_tcont_0</name>
          <alloc-id>1024</alloc-id>
          <interface-reference>ONU-17-1_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_inernet_10G</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-17-2_tcont_0</name>
          <alloc-id>1032</alloc-id>
          <interface-reference>ONU-17-2_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_inernet_10G</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-17-3_tcont_0</name>
          <alloc-id>1040</alloc-id>
          <interface-reference>ONU-17-3_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_inernet_10G</traffic-descriptor-profile-ref>
        </tcont>
        <tcont>
          <name>ONU-17-4_tcont_0</name>
          <alloc-id>1048</alloc-id>
          <interface-reference>ONU-17-4_vAni_slot1_port1_if</interface-reference>
          <traffic-descriptor-profile-ref>LGI_Internet_100M</traffic-descriptor-profile-ref>
        </tcont>
      </tconts>
      <gemports>
        <gemport>
          <name>ONU-1-1_gem_0</name>
          <gemport-id>1024</gemport-id>
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-1_gem_1</name>
          <gemport-id>1025</gemport-id>
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-1_gem_2</name>
          <gemport-id>1026</gemport-id>
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-1_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-2_gem_0</name>
          <gemport-id>1040</gemport-id>
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-2_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-2_gem_1</name>
          <gemport-id>1041</gemport-id>
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-2_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-2_gem_2</name>
          <gemport-id>1042</gemport-id>
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-2_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-3_gem_0</name>
          <gemport-id>1048</gemport-id>
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-3_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-3_gem_1</name>
          <gemport-id>1049</gemport-id>
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-3_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-3_gem_2</name>
          <gemport-id>1050</gemport-id>
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-3_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-4_gem_0</name>
          <gemport-id>1056</gemport-id>
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-4_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-4_gem_1</name>
          <gemport-id>1057</gemport-id>
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
          <traffic-class>1</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-4_tcont_1</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-1-4_gem_2</name>
          <gemport-id>1058</gemport-id>
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
          <traffic-class>2</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-1-4_tcont_2</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-17-1_gem_0</name>
          <gemport-id>1024</gemport-id>
          <interface>ONU-17-1_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>true</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-17-1_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-17-2_gem_0</name>
          <gemport-id>1032</gemport-id>
          <interface>ONU-17-2_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>true</upstream-aes-indicator>
          <tcont-ref>ONU-17-2_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-17-3_gem_0</name>
          <gemport-id>1040</gemport-id>
          <interface>ONU-17-3_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-17-3_tcont_0</tcont-ref>
        </gemport>
        <gemport>
          <name>ONU-17-4_gem_0</name>
          <gemport-id>1048</gemport-id>
          <interface>ONU-17-4_venet_uni-port-10-1_if</interface>
          <traffic-class>0</traffic-class>
          <downstream-aes-indicator>false</downstream-aes-indicator>
          <upstream-aes-indicator>false</upstream-aes-indicator>
          <tcont-ref>ONU-17-4_tcont_0</tcont-ref>
        </gemport>
      </gemports>
    </xpongemtcont>
    <lag-system xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
      <aggregating-system>
        <agg-system>STATIC-LAG-SYSTEM</agg-system>
        <system-id>AB-CD-12-34-56-60</system-id>
        <system-priority>1</system-priority>
      </aggregating-system>
    </lag-system>
    <hardware xmlns="urn:ietf:params:xml:ns:yang:ietf-hardware">
      <component>
        <name>chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
      </component>
      <component>
        <name>slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:slot</class>
        <parent>chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-2</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-3</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-4</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-5</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>5</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-6</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>6</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-7</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>7</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-8</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>8</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-9</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>9</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-10</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>10</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-11</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>11</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-12</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>12</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-13</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>13</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-14</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>14</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-15</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>15</parent-rel-pos>
      </component>
      <component>
        <name>olt-port-1-16</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>16</parent-rel-pos>
      </component>
      <component>
        <name>olt-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>olt-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>olt-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>olt-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>olt-transceiver-1-9</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>olt-port-1-9</parent>
        <parent-rel-pos>9</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>olt-transceiver-link-1-9-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>olt-transceiver-1-9</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-2</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>5</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-3</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>9</parent-rel-pos>
      </component>
      <component>
        <name>nni-port-1-4</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>13</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-2</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-2</parent>
        <parent-rel-pos>2</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-2-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-2</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-3</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-3</parent>
        <parent-rel-pos>3</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-3-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-3</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-1-4</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>nni-port-1-4</parent>
        <parent-rel-pos>4</parent-rel-pos>
      </component>
      <component>
        <name>nni-transceiver-link-1-4-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>nni-transceiver-1-4</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>mgt-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-1-1-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-1-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-1-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-1-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-1-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-1-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-1-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-1-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-1-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-1-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-1-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-1-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-1-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-1-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-1-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-1-2-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-2-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-2-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-2-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-2-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-2-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-2-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-2-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-2-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-2-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-2-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-2-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-2-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-2-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-2-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-1-3-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-3-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-3-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-3-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-3-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-3-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-3-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-3-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-3-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-3-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-3-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-3-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-3-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-3-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-3-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-1-4-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-4-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-4-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-4-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-4-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-1-4-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-1-4-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-4-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-4-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-1-4-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-4-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-4-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-1-4-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-4-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-4-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-1-4-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-1-4-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-17-1-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-1-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-1-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-1-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-1-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-1-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-1-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-1-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-1-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-1-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-1-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-1-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-1-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-1-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-1-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-1-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-1-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-17-2-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-2-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-2-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-2-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-2-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-2-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-2-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-2-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-2-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-2-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-2-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-2-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-2-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-2-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-2-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-2-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-17-3-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-3-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-3-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-3-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-3-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-3-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-3-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-3-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-3-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-3-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-3-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-3-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-3-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-3-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-3-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-3-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-chassis</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:chassis</class>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
        <software xmlns="urn:bbf:yang:bbf-software-management">
          <software>
            <name>ONU-FIRMWARE</name>
          </software>
        </software>
      </component>
      <component>
        <name>ONU-17-4-uni-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-4-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-4-uni-slot-10</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-4-chassis</parent>
        <parent-rel-pos>10</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-4-ani-slot-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:board</class>
        <parent>ONU-17-4-chassis</parent>
        <parent-rel-pos>1</parent-rel-pos>
        <state>
          <admin-state>unlocked</admin-state>
        </state>
      </component>
      <component>
        <name>ONU-17-4-uni-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-4-uni-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-uni-port-10-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-4-uni-slot-10</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-ani-port-1-1</name>
        <class xmlns:ianahw="urn:ietf:params:xml:ns:yang:iana-hardware">ianahw:port</class>
        <parent>ONU-17-4-ani-slot-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-uni-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-4-uni-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-uni-transceiver-10-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-4-uni-port-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-ani-transceiver-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver</class>
        <parent>ONU-17-4-ani-port-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-uni-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-4-uni-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-uni-transceiver-link-10-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-4-uni-transceiver-10-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
      <component>
        <name>ONU-17-4-ani-transceiver-link-1-1-1</name>
        <class xmlns:bbf-hwt="urn:bbf:yang:bbf-hardware-types">bbf-hwt:transceiver-link</class>
        <parent>ONU-17-4-ani-transceiver-1-1</parent>
        <parent-rel-pos>1</parent-rel-pos>
      </component>
    </hardware>
    <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
      <interface>
        <name>cgroup-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-group</type>
        <enabled>true</enabled>
        <channel-group xmlns="urn:bbf:yang:bbf-xpon">
          <polling-period>100</polling-period>
          <raman-mitigation>raman-none</raman-mitigation>
          <system-id>00000</system-id>
          <pon-pools>
            <pon-pool>
              <name>pool-1-1-1</name>
              <channel-termination-ref>cterm-1-1-1</channel-termination-ref>
              <alloc-id-values>1024-1791</alloc-id-values>
              <gemport-values>1021-8957</gemport-values>
              <onu-id-values>0-255</onu-id-values>
            </pon-pool>
          </pon-pools>
        </channel-group>
      </interface>
      <interface>
        <name>cpart-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-partition</type>
        <enabled>true</enabled>
        <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
            <scheduling-level>1</scheduling-level>
            <child-scheduler-nodes>
              <name>sched-node-ONU-1-1_venet_vlan_1_user</name>
            </child-scheduler-nodes>
            <child-scheduler-nodes>
              <name>sched-node-ONU-1-3_venet_vlan_1_user</name>
            </child-scheduler-nodes>
          </scheduler-node>
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>sched-node-ONU-1-1_venet_vlan_1_user</name>
            <scheduling-level>2</scheduling-level>
            <shaper-name>shaper-pir-1Gbps-pbs-100MB</shaper-name>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
          </scheduler-node>
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>sched-node-ONU-1-3_venet_vlan_1_user</name>
            <scheduling-level>2</scheduling-level>
            <shaper-name>shaper-pir-100Mbps-pbs-10MB</shaper-name>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
          </scheduler-node>
          <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
          </child-scheduler-nodes>
        </tm-root>
        <channel-partition xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-1-1</channel-group-ref>
          <channel-partition-index>1</channel-partition-index>
          <downstream-fec>false</downstream-fec>
          <closest-onu-distance>0</closest-onu-distance>
          <maximum-differential-xpon-distance>20</maximum-differential-xpon-distance>
          <authentication-method>serial-number</authentication-method>
          <multicast-aes-indicator>false</multicast-aes-indicator>
        </channel-partition>
      </interface>
      <interface>
        <name>cpair-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
        <enabled>true</enabled>
        <channel-pair xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-1-1</channel-group-ref>
          <channel-partition-ref>cpart-1-1-1</channel-partition-ref>
          <channel-pair-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-pair-type>
        </channel-pair>
      </interface>
      <interface>
        <name>cterm-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>olt-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <channel-pair-ref>cpair-1-1-1</channel-pair-ref>
          <channel-termination-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-termination-type>
          <xgs-pon-id>0</xgs-pon-id>
          <pon-tag>0000000000000000</pon-tag>
          <ber-calc-period>10</ber-calc-period>
          <location xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:inside-olt</location>
          <onu-phase-drift-monitoring-control>
            <monitoring-interval>1000</monitoring-interval>
            <lower-threshold>itut-recommended-value</lower-threshold>
            <upper-threshold>itut-recommended-value</upper-threshold>
          </onu-phase-drift-monitoring-control>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:los</notifiable-defect>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-in-o5</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-emergency-stopped</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-without-v-ani</notifiable-onu-presence-states>
        </channel-termination>
      </interface>
      <interface>
        <name>cgroup-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-group</type>
        <enabled>true</enabled>
        <channel-group xmlns="urn:bbf:yang:bbf-xpon">
          <polling-period>100</polling-period>
          <raman-mitigation>raman-none</raman-mitigation>
          <system-id>00000</system-id>
          <pon-pools>
            <pon-pool>
              <name>pool-1-9-1</name>
              <channel-termination-ref>cterm-1-9-1</channel-termination-ref>
              <alloc-id-values>1024-1791</alloc-id-values>
              <gemport-values>1021-8957</gemport-values>
              <onu-id-values>0-255</onu-id-values>
            </pon-pool>
          </pon-pools>
        </channel-group>
      </interface>
      <interface>
        <name>cpart-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-partition</type>
        <enabled>true</enabled>
        <tm-root xmlns="urn:bbf:yang:bbf-qos-traffic-mngt">
          <scheduler-node xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
            <scheduling-level>1</scheduling-level>
            <queue>
              <local-queue-id>0</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>1</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>2</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>3</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>4</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>5</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>6</local-queue-id>
            </queue>
            <queue>
              <local-queue-id>7</local-queue-id>
            </queue>
          </scheduler-node>
          <child-scheduler-nodes xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
            <name>NODE_DEF</name>
          </child-scheduler-nodes>
        </tm-root>
        <channel-partition xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-1</channel-group-ref>
          <channel-partition-index>1</channel-partition-index>
          <downstream-fec>false</downstream-fec>
          <closest-onu-distance>0</closest-onu-distance>
          <maximum-differential-xpon-distance>20</maximum-differential-xpon-distance>
          <authentication-method>serial-number</authentication-method>
          <multicast-aes-indicator>false</multicast-aes-indicator>
        </channel-partition>
      </interface>
      <interface>
        <name>cpair-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-pair</type>
        <enabled>true</enabled>
        <channel-pair xmlns="urn:bbf:yang:bbf-xpon">
          <channel-group-ref>cgroup-1-9-1</channel-group-ref>
          <channel-partition-ref>cpart-1-9-1</channel-partition-ref>
          <channel-pair-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-pair-type>
        </channel-pair>
      </interface>
      <interface>
        <name>cterm-1-9-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:channel-termination</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>olt-transceiver-link-1-9-1</port-layer-if>
        </hardware-component>
        <channel-termination xmlns="urn:bbf:yang:bbf-xpon">
          <channel-pair-ref>cpair-1-9-1</channel-pair-ref>
          <channel-termination-type xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:xgs</channel-termination-type>
          <xgs-pon-id>0</xgs-pon-id>
          <pon-tag>0000000000000000</pon-tag>
          <ber-calc-period>10</ber-calc-period>
          <location xmlns:bbf-xpon-types="urn:bbf:yang:bbf-xpon-types">bbf-xpon-types:inside-olt</location>
          <onu-phase-drift-monitoring-control>
            <monitoring-interval>1000</monitoring-interval>
            <lower-threshold>itut-recommended-value</lower-threshold>
            <upper-threshold>itut-recommended-value</upper-threshold>
          </onu-phase-drift-monitoring-control>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:los</notifiable-defect>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-on-intended-channel-termination</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-with-v-ani</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-no-onu-id</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-v-ani-known-and-o5-failed-undefined</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-no-v-ani-known-and-in-o5</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-present-and-emergency-stopped</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present</notifiable-onu-presence-states>
          <notifiable-onu-presence-states xmlns="urn:bbf:yang:bbf-xpon-onu-state" xmlns:bbf-xpon-onu-types="urn:bbf:yang:bbf-xpon-onu-types">bbf-xpon-onu-types:onu-not-present-without-v-ani</notifiable-onu-presence-states>
        </channel-termination>
      </interface>
      <interface>
        <name>nni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-2-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-2-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregation-port xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <aggregation-port-lacp>
            <actor-system-priority>1</actor-system-priority>
            <actor-admin-key>1</actor-admin-key>
            <actor-port-priority>4</actor-port-priority>
          </aggregation-port-lacp>
        </aggregation-port>
      </interface>
      <interface>
        <name>nni-1-3-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-3-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>nni-1-4-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>nni-transceiver-link-1-4-1</port-layer-if>
        </hardware-component>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
      </interface>
      <interface>
        <name>LAG-1</name>
        <description>NNI_LAG</description>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ieee8023adLag</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <aggregator xmlns="urn:ieee:std:802.1AX:yang:ieee802-dot1ax">
          <name>AGG-LAG-1</name>
          <agg-system-name>STATIC-LAG-SYSTEM</agg-system-name>
          <admin-state>up</admin-state>
          <link-up-down-notification>enabled</link-up-down-notification>
          <aggregator-lacp>
            <actor-admin-key>1</actor-admin-key>
          </aggregator-lacp>
        </aggregator>
      </interface>
      <interface>
        <name>ONU-1-1-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-1-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>1</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-1-1_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>1</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>GPON21500096</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-1-1-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-1-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-1_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-1-1_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-1-1_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1011</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                  <tag>
                    <index>1</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>11</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                <vlan-id>1011</vlan-id>
                <write-pbit>0</write-pbit>
                <write-dei-0/>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_1001</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_1002</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_501</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>501</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_DoubleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_DoubleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_doubleTag_circuitID</profile-ref>
        </pppoe>
        <egress-tm-objects xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
          <root-if-name>cpart-1-1-1</root-if-name>
          <scheduler-node-name>sched-node-ONU-1-1_venet_vlan_1_user</scheduler-node-name>
        </egress-tm-objects>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>11</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_venet_vlan_2_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_venet_vlan_3_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_venet_vlan_4_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>501</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>11</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_uni-port-10-1_vlan2</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>101</vlan-id>
                      <pbit>4</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1001</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>101</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_uni-port-10-1_vlan3</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>201</vlan-id>
                      <pbit>7</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1002</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>201</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1_uni-port-10-1_vlan4</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-1-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>501</vlan-id>
                      <pbit>0</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-1-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-1-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-2-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-2-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>2</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-1-2_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>2</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>GPON215000F4</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-1-2-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-2-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-2_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-1-2_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-1-2_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1012</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag_circuitID</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1012</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_venet_vlan_2_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_venet_vlan_3_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>12</vlan-id>
                      <pbit>0</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1012</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>12</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_uni-port-10-1_vlan2</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>102</vlan-id>
                      <pbit>4</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1001</vlan-id>
                    <write-pbit>4</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>102</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2_uni-port-10-1_vlan3</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-2-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>202</vlan-id>
                      <pbit>7</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1002</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>202</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-2-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-2-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-3-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-3-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>3</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-1-3_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>3</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>GPON21500087</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-1-3-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-3-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-3_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-1-3_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-1-3_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1013</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                  <tag>
                    <index>1</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>113</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                <vlan-id>1013</vlan-id>
                <write-pbit>0</write-pbit>
                <write-dei-0/>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_DoubleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_DoubleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_doubleTag_circuitID</profile-ref>
        </pppoe>
        <egress-tm-objects xmlns="urn:bbf:yang:bbf-qos-enhanced-scheduling">
          <root-if-name>cpart-1-1-1</root-if-name>
          <scheduler-node-name>sched-node-ONU-1-3_venet_vlan_1_user</scheduler-node-name>
        </egress-tm-objects>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>113</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_venet_vlan_2_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_venet_vlan_3_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>13</vlan-id>
                      <pbit>0</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>113</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>13</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_uni-port-10-1_vlan2</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>103</vlan-id>
                      <pbit>4</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1001</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>103</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3_uni-port-10-1_vlan3</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-3-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>203</vlan-id>
                      <pbit>7</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1002</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>203</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-3-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-3-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-4-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-4-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>4</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-1-4_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>4</onu-id>
          <channel-partition>cpart-1-1-1</channel-partition>
          <expected-serial-number>GPON215000D9</expected-serial-number>
          <preferred-channel-pair>cpair-1-1-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-1-4-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-4-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-1-4_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-1-4_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-1-4_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>1014</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_500</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>500</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag_circuitID</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>1014</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_venet_vlan_2_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_venet_vlan_3_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>1002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_venet_vlan_4_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>500</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                    <vlan-id>1014</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_uni-port-10-1_vlan2</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>104</vlan-id>
                      <pbit>4</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1001</vlan-id>
                    <pbit-from-tag-index>0</pbit-from-tag-index>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>104</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_uni-port-10-1_vlan3</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                      <vlan-id>204</vlan-id>
                      <pbit>7</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>1002</vlan-id>
                    <write-pbit>7</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
            <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
              <index>0</index>
              <dot1q-tag>
                <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:c-vlan</tag-type>
                <vlan-id>204</vlan-id>
                <pbit-from-tag-index>0</pbit-from-tag-index>
                <dei-from-tag-index>0</dei-from-tag-index>
              </dot1q-tag>
            </push-tag>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4_uni-port-10-1_vlan4</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-1-4-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>500</vlan-id>
                      <pbit>0</pbit>
                      <dei>0</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-1-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-1-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>1</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-17-1_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>1</onu-id>
          <channel-partition>cpart-1-9-1</channel-partition>
          <expected-serial-number>GPON21500094</expected-serial-number>
          <preferred-channel-pair>cpair-1-9-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-17-1-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-1-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-1_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-17-1_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-17-1_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-1_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag_circuitID</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-1_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3001</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-1_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-1-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>3001</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-2-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-2-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>2</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-17-2_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>2</onu-id>
          <channel-partition>cpart-1-9-1</channel-partition>
          <expected-serial-number>GPON2150010D</expected-serial-number>
          <preferred-channel-pair>cpair-1-9-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-17-2-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-2-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-2_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-17-2_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>ONU-17-2_olt_uplink_vlan1_network</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-2_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <pppoe xmlns="urn:bbf:yang:bbf-pppoe-intermediate-agent">
          <enable>true</enable>
          <profile-ref>PPPoE_singleTag_circuitID</profile-ref>
        </pppoe>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-2_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3002</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-2_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">onuIngressDefault_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-2-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>3002</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-1-4-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-1-4-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-3-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-3-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>3</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-17-3_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>3</onu-id>
          <channel-partition>cpart-1-9-1</channel-partition>
          <expected-serial-number>GPON2150016D</expected-serial-number>
          <preferred-channel-pair>cpair-1-9-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-17-3-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-3-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-3_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-17-3_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_3000</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3000</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-3_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-3_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>3000</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-3_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-3-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>3000</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-1-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-1-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-2-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-2-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-4-ani-1-1-1</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:ani</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-4-ani-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
        <ani xmlns="urn:bbf:yang:bbf-xponani">
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <onu-id>4</onu-id>
        </ani>
      </interface>
      <interface>
        <name>ONU-17-4_vAni_slot1_port1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:v-ani</type>
        <enabled>true</enabled>
        <v-ani xmlns="urn:bbf:yang:bbf-xponvani">
          <onu-id>4</onu-id>
          <channel-partition>cpart-1-9-1</channel-partition>
          <expected-serial-number>GPON21500148</expected-serial-number>
          <preferred-channel-pair>cpair-1-9-1</preferred-channel-pair>
          <upstream-fec>false</upstream-fec>
          <management-gemport-aes-indicator>true</management-gemport-aes-indicator>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lobi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:looci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:lopci</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dgi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:sdi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loki</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dfi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:loai</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:dowi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:tiwi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:memi</notifiable-defect>
          <notifiable-defect xmlns:bbf-xpon-def="urn:bbf:yang:bbf-xpon-defects">bbf-xpon-def:peei</notifiable-defect>
        </v-ani>
      </interface>
      <interface>
        <name>ONU-17-4-uni-10-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <enabled>true</enabled>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-4-uni-transceiver-link-10-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-4_venet_uni-port-10-1_if</name>
        <type xmlns:bbf-xponift="urn:bbf:yang:bbf-xpon-if-type">bbf-xponift:olt-v-enet</type>
        <enabled>true</enabled>
        <olt-v-enet xmlns="urn:bbf:yang:bbf-xponvani">
          <lower-layer-interface>ONU-17-4_vAni_slot1_port1_if</lower-layer-interface>
        </olt-v-enet>
      </interface>
      <interface>
        <name>olt_uplink_nx1_vlan_4000</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>network-port</interface-usage>
        </interface-usage>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>LAG-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>4000</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
            </rule>
          </ingress-rule>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-4_venet_vlan_1_user</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <interface-usage xmlns="urn:bbf:yang:bbf-interface-usage">
          <interface-usage>user-port</interface-usage>
        </interface-usage>
        <l2-dhcpv4-relay xmlns="urn:bbf:yang:bbf-l2-dhcpv4-relay">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCP_Default_SingleTag</profile-ref>
        </l2-dhcpv4-relay>
        <dhcpv6-ldra xmlns="urn:bbf:yang:bbf-ldra">
          <enable>true</enable>
          <trusted-port>false</trusted-port>
          <profile-ref>DHCPv6_Default_SingleTag</profile-ref>
        </dhcpv6-ldra>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <egress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">QPP0_profile</egress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-4_venet_uni-port-10-1_if</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <tag>
                    <index>0</index>
                    <dot1q-tag>
                      <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                      <vlan-id>4000</vlan-id>
                      <pbit>any</pbit>
                      <dei>any</dei>
                    </dot1q-tag>
                  </tag>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-4_uni-port-10-1_vlan1</name>
        <type xmlns:bbfift="urn:bbf:yang:bbf-if-type">bbfift:vlan-sub-interface</type>
        <enabled>true</enabled>
        <ingress-qos-policy-profile xmlns="urn:bbf:yang:bbf-qos-policies">LGI_Test_profile</ingress-qos-policy-profile>
        <subif-lower-layer xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <interface>ONU-17-4-uni-10-1-1</interface>
        </subif-lower-layer>
        <inline-frame-processing xmlns="urn:bbf:yang:bbf-sub-interfaces">
          <ingress-rule>
            <rule>
              <name>rule_1</name>
              <priority>100</priority>
              <flexible-match>
                <match-criteria xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <untagged/>
                  <ethernet-frame-type>any</ethernet-frame-type>
                </match-criteria>
              </flexible-match>
              <ingress-rewrite>
                <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">0</pop-tags>
                <push-tag xmlns="urn:bbf:yang:bbf-sub-interface-tagging">
                  <index>0</index>
                  <dot1q-tag>
                    <tag-type xmlns:bbf-dot1qt="urn:bbf:yang:bbf-dot1q-types">bbf-dot1qt:s-vlan</tag-type>
                    <vlan-id>4000</vlan-id>
                    <write-pbit>0</write-pbit>
                    <write-dei-0/>
                  </dot1q-tag>
                </push-tag>
              </ingress-rewrite>
            </rule>
          </ingress-rule>
          <egress-rewrite>
            <pop-tags xmlns="urn:bbf:yang:bbf-sub-interface-tagging">1</pop-tags>
          </egress-rewrite>
        </inline-frame-processing>
      </interface>
      <interface>
        <name>ONU-17-3-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-3-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
      <interface>
        <name>ONU-17-4-uni-1-1-1</name>
        <type xmlns:ianaift="urn:ietf:params:xml:ns:yang:iana-if-type">ianaift:ethernetCsmacd</type>
        <hardware-component xmlns="urn:bbf:yang:bbf-hardware">
          <port-layer-if>ONU-17-4-uni-transceiver-link-1-1-1</port-layer-if>
        </hardware-component>
      </interface>
    </interfaces>
    <keystore xmlns="urn:ietf:params:xml:ns:yang:ietf-keystore">
      <asymmetric-keys>
        <asymmetric-key>
          <name>genkey</name>
          <public-key-format xmlns:ct="urn:ietf:params:xml:ns:yang:ietf-crypto-types">ct:ssh-public-key-format</public-key-format>
          <public-key>MIIBigKCAYEAuQYXga9we1FbTQfc7E35E3lXHZMtgenYQbyZ9f14gK/RZPydGQUK30NCEWfqXZ6JR19C/IEKmwkfp+ZQSMMleR7AdxYGSXCn+bl0+SFoLC4uW9RF91RybJH3gLFBKIy2/4pbgS8YmiaGqw6diu26f0lMCj6TOlwROgjNFRiCMU6cRO5ZVK0iMU2vlJKAxA1NFTNme3h1TirE2jcdrwVReAxUgsT/5q/SZnVeWLvdkPrHLsYKC81UyMHPd1YxONm+CxMGmaqnSf6bMgxKG3Vq4gJ44/LmLBUKvwp6c+5ZJJX/GYkLZOz99cTmRXumXkeZpNo3xHS8i7gtecYlL9vHVthvt2+x9I26PfJcCzFAWseMAKSZFcsFcXipErVKOG0UtJyufdhAvrB1GFPhLoTIjrlDs36DKogGI983+3wIzm1NbAtD0QLcAKVTPyWUyAnagR1gnjjBzokZIA2HSE10Pm8SMLEd10bBdDI8yKZ7gVBRqvo30HcyP/4cZN+Pe4u/AgMBAAE=</public-key>
          <private-key-format xmlns:ct="urn:ietf:params:xml:ns:yang:ietf-crypto-types">ct:rsa-private-key-format</private-key-format>
          <cleartext-private-key>MIIG4wIBAAKCAYEAuQYXga9we1FbTQfc7E35E3lXHZMtgenYQbyZ9f14gK/RZPydGQUK30NCEWfqXZ6JR19C/IEKmwkfp+ZQSMMleR7AdxYGSXCn+bl0+SFoLC4uW9RF91RybJH3gLFBKIy2/4pbgS8YmiaGqw6diu26f0lMCj6TOlwROgjNFRiCMU6cRO5ZVK0iMU2vlJKAxA1NFTNme3h1TirE2jcdrwVReAxUgsT/5q/SZnVeWLvdkPrHLsYKC81UyMHPd1YxONm+CxMGmaqnSf6bMgxKG3Vq4gJ44/LmLBUKvwp6c+5ZJJX/GYkLZOz99cTmRXumXkeZpNo3xHS8i7gtecYlL9vHVthvt2+x9I26PfJcCzFAWseMAKSZFcsFcXipErVKOG0UtJyufdhAvrB1GFPhLoTIjrlDs36DKogGI983+3wIzm1NbAtD0QLcAKVTPyWUyAnagR1gnjjBzokZIA2HSE10Pm8SMLEd10bBdDI8yKZ7gVBRqvo30HcyP/4cZN+Pe4u/AgMBAAECggGAHpeXQ2YSnxEwm2f1a0zpJgMmGEnBeH2FuDjK7BVg20Y2xQ/PmddvmMKyJdactaYE5Lwng0CC1GeJyGUYWS+K/p/LCuWlXHc4Dt5PLPING2D3YU+T0fUwhisMVUb5kw7RIydpQc7broE4OwhLnDD6aRlhbUAzb67RWlsiLZ7DyAtLY0pVkt6djLFfmp0ulTvtxtec1kVwf+AqdDowukOS2NqRDp4sAaSIkVBOrTVCyTntvRoZhyIIJrmE6CJkORYnQcKm2Vv4iepH/IovzJeHSqtFqpiFz3PzjMRaEFxGvR4muhRLzc4+FevvRUtOkuEaA6iamLx09trR0LCGODAGuAaiQhAiYpCuzBTfOrUlwhiPL1PQL+9W9QiQCU9KsjxsEijqDCy4fvcMY/FnVMus3JtE+pYttGZdv7vv8LIyDDAV465KZdgjgoLTKcGxtErI3AnFAvwplMKCFiHarbasGOM2yKb49SZhb7P8NRpCmkF/ecIYueAnYJrms1rbwhchAoHBAO4a0/YvghL3Sxma5Pkh/r6LKy2l/BS5ga6+3vfddk8hctQkOsXsP8Nztg1kcUQzhdVL7HNpFFz7qMg5+CvvWLXT8vq4aecj+TDlt2y8PmVioVssbXuQAzUyUxRHtbTA3qGfvIKwMFtJOf8bcAmHO9X+PwF76/MIQo0nOiLapo3x7lmhiEFzWsxOT/RJepgMmiMDMKpwtkuQWDrWY7wrxYbwRVYfeOQncUCKybTOgVuiZCrqL5BjgdTMqWeyRnZZQwKBwQDG7fo40ZFfaGnr+gilyn6z/XxBos4er9pRB7CKkEsIv2U8uRB0DTgxIqqRPNJMQ/e58rG5BIeAe91YkWYrv9qkGqQIWpEJd1Rz330IkkqllORpfFb73Cg3Lt/rWt54u0bVijeyO1N9lyhRHzpIW1lOmql1jNZIIR5yrKjICuARv6VKcnjXOdsbDaQ2PRJkPHHRnQt1H7o4UJIyhIPaWHMZCQ+rFql7mH7V4G9dx1ollSC8mZC1bMXaLTaM39D7rdUCgcB15S7CnS9ouK2k1f8+JEkAi+QrTB6PHHNL1RKN5EgqUkOLKw025w2Dd43S/8LdpC1GObuwQX1ltO4ThjCNgIuKLJII9rrpSfSe839pBaRXiwieHldvcRVFh89/ISqlf0I9ANzUUO0ApjdjS3CkJyPHh4Ym8/cWdSaOwbeVfnItoncERmkzDy0MMFKCgMeE9eh0IaY9HmYE8EnfiDwF9h5t/BY42IiBX85ByPaq4f1HJBc48I/wjHTCqzvOLoWZIAUCgcBGDnkYmXVAzFzBJgT1niKQ8KxZ0SQV2ohgEP0zTy2dnwngIKySsjUf2L2I+Ip3IViUu8urBNVTgkupbUs2DRLKyDcMWhjJ5KRxSjuWUS7IsW7fV1Kq0BW5mWByWkYO7qU7frmuowX8LMeeCglUghcpf34+T6MHM+KtL/EgwfO3TG7BkR7NbSqklGFIWKmpc0ACOfRXAx1px7Y05EYrFwsxvecusYRuan4AhFG0DQjKQ4KL6Oj1e6ER/OpBInFOsSECgcEAy7Q1cQvyLbL0HPR7WcH5FYOCEp/6BnNZ3aTFfcdiL+hzpRinzy+s5FgMoUY+FYT2qj97RRbviAeQBBV5hz7B3DtF2zPaHnBzLAzb7iDSf2Sot16u+k9GJdviWrzswrp5jf5+Lc8PDVyQzTFK8r876+LZxx89Bb7tjvtjv27ew6XWeqezA1oh0DXBGB9xWoi79NTmzKYe1Ng2ZI3zG81Vzp6Q99YdI3W1yPi0f09o2ymOUvaL1xoUrjuDbwA4iz0a</cleartext-private-key>
        </asymmetric-key>
      </asymmetric-keys>
    </keystore>
    <nacm xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-acm">
      <enable-nacm>false</enable-nacm>
    </nacm>
    <netconf-server xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-server">
      <listen>
        <endpoint>
          <name>default-ssh</name>
          <ssh>
            <tcp-server-parameters>
              <local-address>0.0.0.0</local-address>
              <local-port>10830</local-port>
              <keepalives>
                <idle-time>1</idle-time>
                <max-probes>10</max-probes>
                <probe-interval>5</probe-interval>
              </keepalives>
            </tcp-server-parameters>
            <ssh-server-parameters>
              <server-identity>
                <host-key>
                  <name>default-key</name>
                  <public-key>
                    <keystore-reference>genkey</keystore-reference>
                  </public-key>
                </host-key>
              </server-identity>
              <client-authentication>
                <supported-authentication-methods>
                  <publickey/>
                  <password/>
                </supported-authentication-methods>
              </client-authentication>
            </ssh-server-parameters>
          </ssh>
        </endpoint>
      </listen>
    </netconf-server>
  </data>
</rpc-reply>
```

---

# 8. `XPON` YANG Data Models

The following `XPON` YANG data model structures used by XGS-PON are provided for convience only.

> **Note:** Refer to the specific YANG release accompanying the OLT for the valid YANG models, as well as any YANG models that are purely definitive (i.e. contain only YANG `TYPEDEF` statements) and not shown in the section that follow.

## 8.1. Common, `XPON` & IETF Model Relationship

```text
        ┌────────────┐ ┌──────────────┐
        │iana-if-type│ │bbf-yang-types│
        └────────┬───┘ └──────┬───────┘
                 │            │
                 │        ┌───▼────┐ ┌─────────────────┐ ┌──────────────────┐
                 └───────►│bbf-qos*│ │bbf-l2-forwarding│ │bbf-sub-interfaces│
                          └───▲────┘ └──▲──────────────┘ └────────▲─────────┘
         ┌────────────────┐   └───────┐ │ ┌───────────────────────┘
         │bbf-xpon-if-type◄─────────┐ │ │ │
         └────────────┬───┘         │ │ │ │
                      │             │ │ │ │
                 ┌────▼───────┐    ┌┴─┴─┴─┴────────┐  ┌────────────────────┐
┌─────────────┐ ┌┴───────────┐◄────┤ietf-interfaces├──►bbf-mgmd (multicast)│
│bbf-xpon-type├─►bbf-xpon-...├┘    └──────────────┬┘  └────────────────────┘
└─────────────┘ └──────────▲▲┘                    │
                           ││  ┌───────────────┐ ┌▼─────────────┐
                           │└──┤ietf-yang-types│ │bbf-link-table│
                           │   └───────────────┘ └──────────────┘
                           │   ┌───────────────┐ ┌─────────────┐
                           └───┤ietf-inet-types│ │ietf-hardware│
                               └───────────────┘ └──────┬──────┘
                                                        │
                                            ┌───────────▼───────────┐
                                            │bbf-hardware-extensions│
                                            └───────────────────────┘
 import ─►
```

## 8.2. bbf-dot1q-cfm-interfaces

```text
module: bbf-dot1q-cfm-interfaces

  augment /dot1q-cfm:cfm/dot1q-cfm:maintenance-group/dot1q-cfm:mep:
    +--rw interface?   if:interface-ref
```

- Module Dependencies: `iana-if-type` `ietf-interfaces` `bbf-if-type` `ieee802-dot1q-cfm`

## 8.3. bbf-dot1q-cfm-interfaces-state

```text
module: bbf-dot1q-cfm-interfaces-state

  augment /if:interfaces-state/if:interface:
    +--ro cfm-stack!
       +--ro cfm-stack* [md-level direction]
          +--ro md-level       cfm-types:md-level-type
          +--ro direction      cfm-types:mp-direction-type
          +--ro mac-address    yang:mac-address
          +--ro (maintenance-point)
             +--:(mep)
             |  +--ro mep
             |     +--ro maintenance-group-id    -> /dot1q-cfm:cfm/maintenance-group/maintenance-group-id
             |     +--ro mep-id                  -> /dot1q-cfm:cfm/maintenance-group[dot1q-cfm:maintenance-group-id = current()/../maintenance-group-id]/mep/mep-id
             +--:(mhf)
                +--ro mhf
                   +--ro (mip-creation)
                      +--:(auto-created)
                         +--ro md-id    -> /dot1q-cfm:cfm/maintenance-domain/md-id
                         +--ro ma-id    -> /dot1q-cfm:cfm/maintenance-domain[dot1q-cfm:md-id = current()/../md-id]/maintenance-association/ma-id
```

- Module Dependencies: `iana-if-type` `ietf-yang-types` `ietf-interfaces` `bbf-if-type` `ieee802-dot1q-cfm-types` `bbf-dot1q-cfm`

## 8.4. bbf-dot1q-cfm-l2-forwarding

```text
module: bbf-dot1q-cfm-l2-forwarding

  augment /dot1q-cfm:cfm/dot1q-cfm:maintenance-group:
    +--rw forwarder?   bbf-l2-fwd:forwarder-ref
```

- Module Dependencies: `ieee802-dot1q-cfm` `bbf-l2-forwarding`

## 8.5. bbf-dot1q-cfm

```text
module: bbf-dot1q-cfm

  augment /dot1q-cfm:cfm/dot1q-cfm:maintenance-domain/dot1q-cfm:maintenance-association:
    +--rw discard-ingressing-ltm*   bbf-if-usg:interface-usage {discard-ingressing-ltm}?
```

- Module Dependencies: `ieee802-dot1q-cfm` `bbf-interface-usage`

## 8.6. bbf-equipment-inventory

```text
module: bbf-equipment-inventory
  +--ro equipment-inventory {bbf-inv:reports-equipment-inventory}?
     +--ro device-inventory {bbf-inv:reports-device-inventory}?
     |  +--ro device* [device-id]
     |     +--ro device-id          string
     |     +--ro hardware-info
     |     |  +--ro device-type?   identityref
     |     |  +--ro serial-num?    bbf-yang:string-ascii64
     |     |  +--ro mfg-name?      bbf-yang:string-ascii64
     |     |  +--ro model-name?    bbf-yang:string-ascii64
     |     +--ro management-info
     |     |  +--ro ip-address?    inet:host
     |     |  +--ro port?          inet:port-number
     |     |  +--ro admin-state?   bbf-nodet:admin-state
     |     +--ro software-info
     |        +--ro software-instance* [software-type current-revision]
     |           +--ro software-type                    identityref
     |           +--ro current-revision                 bbf-yang:string-ascii64
     |           +--ro previous-revision?               bbf-yang:string-ascii64-or-empty
     |           +--ro current-revision-last-changed?   yang:date-and-time
     +--ro pan-info {bbf-inv:reports-baa-pan-inventory}?
     |  +--ro pan-instance* [pan-id]
     |     +--ro pan-id                          bbf-yang:string-ascii64
     |     +--ro device-ref?                     -> /equipment-inventory/device-inventory/device/device-id {bbf-inv:reports-device-inventory}?
     |     +--ro admin-state?                    bbf-node-types:admin-state
     |     +--ro admin-state-change-timestamp?   yang:date-and-time
     |     +--ro datastore
     |     |  +--ro metamodel*                             string
     |     |  +--ro metamodel-alignment-state?             bbf-node-types:alignment-state
     |     |  +--ro schema-update-timestamp?               yang:date-and-time
     |     |  +--ro an-alignment-state?                    bbf-node-types:alignment-state
     |     |  +--ro an-alignment-state-change-timestamp?   yang:date-and-time
     |     +--ro device-adapter
     |        +--ro software-type?                      identityref
     |        +--ro current-revision?                   bbf-yang:string-ascii64
     |        +--ro previous-revision?                  bbf-yang:string-ascii64-or-empty
     |        +--ro current-revision-last-changed?      yang:date-and-time
     |        +--ro alignment-state-update-timestamp?   yang:date-and-time
     +--ro eud-inventory {bbf-inv:reports-end-user-device-inventory}?
        +--ro eud-equipment* [eud-id]
           +--ro eud-id             string
           +--ro hardware-info
           |  +--ro device-type?   identityref
           |  +--ro serial-num?    bbf-yang:string-ascii64
           |  +--ro mfg-name?      bbf-yang:string-ascii64
           |  +--ro model-name?    bbf-yang:string-ascii64
           +--ro management-info
           |  +--ro ip-address?    inet:host
           |  +--ro port?          inet:port-number
           |  +--ro admin-state?   bbf-nodet:admin-state
           +--ro software-info
              +--ro software-instance* [software-type current-revision]
                 +--ro software-type                    identityref
                 +--ro current-revision                 bbf-yang:string-ascii64
                 +--ro previous-revision?               bbf-yang:string-ascii64-or-empty
                 +--ro current-revision-last-changed?   yang:date-and-time
```

- Module Dependencies: `bbf-yang-types` `bbf-device` `bbf-baa-pan`

## 8.7. bbf-ethernet-performance-management

```text
module: bbf-ethernet-performance-management

  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-15min/bbf-if-pm:current:
    +--ro ethernet
       +--ro in-pkts-errors-fcs?        bbf-yang:performance-counter64
       +--ro in-giant-pkts?             bbf-yang:performance-counter64
       +--ro in-undersize-pkts?         bbf-yang:performance-counter64 {bbf-eth-pm:in-undersize-pkts}?
       +--ro in-errors-mac-internal?    bbf-yang:performance-counter64
       +--ro out-errors-mac-internal?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-15min/bbf-if-pm:history:
    +--ro ethernet
       +--ro in-pkts-errors-fcs?        bbf-yang:performance-counter64
       +--ro in-giant-pkts?             bbf-yang:performance-counter64
       +--ro in-undersize-pkts?         bbf-yang:performance-counter64 {bbf-eth-pm:in-undersize-pkts}?
       +--ro in-errors-mac-internal?    bbf-yang:performance-counter64
       +--ro out-errors-mac-internal?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-24hr/bbf-if-pm:current:
    +--ro ethernet {bbf-if-pm:performance-24hr}?
       +--ro in-pkts-errors-fcs?        bbf-yang:performance-counter64
       +--ro in-giant-pkts?             bbf-yang:performance-counter64
       +--ro in-undersize-pkts?         bbf-yang:performance-counter64 {bbf-eth-pm:in-undersize-pkts}?
       +--ro in-errors-mac-internal?    bbf-yang:performance-counter64
       +--ro out-errors-mac-internal?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-24hr/bbf-if-pm:history:
    +--ro ethernet {bbf-if-pm:performance-24hr}?
       +--ro in-pkts-errors-fcs?        bbf-yang:performance-counter64
       +--ro in-giant-pkts?             bbf-yang:performance-counter64
       +--ro in-undersize-pkts?         bbf-yang:performance-counter64 {bbf-eth-pm:in-undersize-pkts}?
       +--ro in-errors-mac-internal?    bbf-yang:performance-counter64
       +--ro out-errors-mac-internal?   bbf-yang:performance-counter64
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-interfaces-performance-management` `iana-if-type`

## 8.8. bbf-frame-processing-profiles

```text
module: bbf-frame-processing-profiles
  +--rw frame-processing-profiles
     +--rw frame-processing-profile* [name]
        +--rw name                bbf-yang:string-ascii64
        +--rw priority            uint16
        +--rw match-criteria!
        |  +--rw any-frame?                  empty
        |  +--rw vlans!
        |  |  +--rw (vlan-tag-match-type)
        |  |     +--:(untagged)
        |  |     |  +--rw untagged?   empty
        |  |     +--:(vlan-tagged)
        |  |        +--rw tag* [index]
        |  |           +--rw index       bbf-classif:tag-index
        |  |           +--rw tag-type    union
        |  |           +--rw vlan-id     union
        |  |           +--rw pbit?       union
        |  |           +--rw dei?        union
        |  +--rw frame-destination-filter!
        |  |  +--rw destination-mac-address* [name]
        |  |  |  +--rw name                               bbf-yang:string-ascii64
        |  |  |  +--rw (mac-address)?
        |  |  |     +--:(any-multicast-mac-address)
        |  |  |     |  +--rw any-multicast-mac-address?   empty
        |  |  |     +--:(unicast-address)
        |  |  |     |  +--rw unicast-address?             empty
        |  |  |     +--:(broadcast-address)
        |  |  |     |  +--rw broadcast-address?           empty
        |  |  |     +--:(cfm-multicast-address)
        |  |  |     |  +--rw cfm-multicast-address?       empty
        |  |  |     +--:(ipv4-multicast-address)
        |  |  |     |  +--rw ipv4-multicast-address?      empty
        |  |  |     +--:(ipv6-multicast-address)
        |  |  |     |  +--rw ipv6-multicast-address?      empty
        |  |  |     +--:(mac-address-filter)
        |  |  |        +--rw mac-address-value?           yang:mac-address
        |  |  |        +--rw mac-address-mask?            yang:mac-address
        |  |  +--rw destination-ipv4-address* [name]
        |  |  |  +--rw name                                 bbf-yang:string-ascii64
        |  |  |  +--rw (ipv4-address)?
        |  |  |     +--:(any-multicast-ipv4-address)
        |  |  |     |  +--rw any-multicast-ipv4-address?    empty
        |  |  |     +--:(all-hosts-multicast-address)
        |  |  |     |  +--rw all-hosts-multicast-address?   empty
        |  |  |     +--:(rip-multicast-address)
        |  |  |     |  +--rw rip-multicast-address?         empty
        |  |  |     +--:(ntp-multicast-address)
        |  |  |     |  +--rw ntp-multicast-address?         empty
        |  |  |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
        |  |  |        +--rw ipv4-prefix?                   inet:ipv4-prefix
        |  |  +--rw destination-ipv6-address* [name]
        |  |     +--rw name                                      bbf-yang:string-ascii64
        |  |     +--rw (ipv6-address)?
        |  |        +--:(any-multicast-ipv6-address)
        |  |        |  +--rw any-multicast-ipv6-address?         empty
        |  |        +--:(all-nodes-multicast-ipv6-address)
        |  |        |  +--rw all-nodes-multicast-ipv6-address?   empty
        |  |        +--:(rip-multicast-ipv6-address)
        |  |        |  +--rw rip-multicast-ipv6-address?         empty
        |  |        +--:(ntp-multicast-ipv6-address)
        |  |        |  +--rw ntp-multicast-ipv6-address?         empty
        |  |        +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
        |  |           +--rw ipv6-prefix?                        inet:ipv6-prefix
        |  +--rw ethernet-frame-type*        bbf-dot1qt:ether-type-or-acronym
        |  +--rw protocol*                   protocols
        |  +--rw dscp-range?                 bbf-inet:dscp-range {dscp-match-criteria}?
        +--rw exclude-criteria! {bbf-subif-tag:exclude-criteria}?
        |  +--rw frame-destination-filter!
        |  |  +--rw destination-mac-address* [name]
        |  |  |  +--rw name                               bbf-yang:string-ascii64
        |  |  |  +--rw (mac-address)?
        |  |  |     +--:(any-multicast-mac-address)
        |  |  |     |  +--rw any-multicast-mac-address?   empty
        |  |  |     +--:(unicast-address)
        |  |  |     |  +--rw unicast-address?             empty
        |  |  |     +--:(broadcast-address)
        |  |  |     |  +--rw broadcast-address?           empty
        |  |  |     +--:(cfm-multicast-address)
        |  |  |     |  +--rw cfm-multicast-address?       empty
        |  |  |     +--:(ipv4-multicast-address)
        |  |  |     |  +--rw ipv4-multicast-address?      empty
        |  |  |     +--:(ipv6-multicast-address)
        |  |  |     |  +--rw ipv6-multicast-address?      empty
        |  |  |     +--:(mac-address-filter)
        |  |  |        +--rw mac-address-value?           yang:mac-address
        |  |  |        +--rw mac-address-mask?            yang:mac-address
        |  |  +--rw destination-ipv4-address* [name]
        |  |  |  +--rw name                                 bbf-yang:string-ascii64
        |  |  |  +--rw (ipv4-address)?
        |  |  |     +--:(any-multicast-ipv4-address)
        |  |  |     |  +--rw any-multicast-ipv4-address?    empty
        |  |  |     +--:(all-hosts-multicast-address)
        |  |  |     |  +--rw all-hosts-multicast-address?   empty
        |  |  |     +--:(rip-multicast-address)
        |  |  |     |  +--rw rip-multicast-address?         empty
        |  |  |     +--:(ntp-multicast-address)
        |  |  |     |  +--rw ntp-multicast-address?         empty
        |  |  |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
        |  |  |        +--rw ipv4-prefix?                   inet:ipv4-prefix
        |  |  +--rw destination-ipv6-address* [name]
        |  |     +--rw name                                      bbf-yang:string-ascii64
        |  |     +--rw (ipv6-address)?
        |  |        +--:(any-multicast-ipv6-address)
        |  |        |  +--rw any-multicast-ipv6-address?         empty
        |  |        +--:(all-nodes-multicast-ipv6-address)
        |  |        |  +--rw all-nodes-multicast-ipv6-address?   empty
        |  |        +--:(rip-multicast-ipv6-address)
        |  |        |  +--rw rip-multicast-ipv6-address?         empty
        |  |        +--:(ntp-multicast-ipv6-address)
        |  |        |  +--rw ntp-multicast-ipv6-address?         empty
        |  |        +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
        |  |           +--rw ipv6-prefix?                        inet:ipv6-prefix
        |  +--rw ethernet-frame-type*        bbf-dot1qt:ether-type-or-acronym
        |  +--rw protocol*                   protocols
        |  +--rw dscp-range?                 bbf-inet:dscp-range {dscp-match-criteria}?
        +--rw ingress-rewrite!
        |  +--rw copy-from-tags-to-marking-list* [tag-index]
        |  |  +--rw tag-index             bbf-classif:tag-index
        |  |  +--rw pbit-marking-index?   union
        |  |  +--rw dei-marking-index?    union
        |  +--rw pop-tags?                         uint8
        |  +--rw push-tag* [index] {bbf-fpprof:vlan-id-translation}?
        |     +--rw index                        bbf-classif:tag-index
        |     +--rw tag-type?                    union
        |     +--rw vlan-id                      union
        |     +--rw (pbit)
        |     |  +--:(write-pbit-0)
        |     |  |  +--rw write-pbit-0?          empty
        |     |  +--:(copy-pbit-from-input-or-0)
        |     |  |  +--rw pbit-from-tag-index?   bbf-classif:tag-index
        |     |  +--:(write-pbit-value) {bbf-subif-tag:write-pbit-value-in-vlan-tag}?
        |     |  |  +--rw write-pbit?            bbf-dot1qt:pbit
        |     |  +--:(generate-pbit-from-marking-list-or-0)
        |     |     +--rw pbit-marking-index?    bbf-qos-cls:qos-pbit-marking-index
        |     +--rw (dei)
        |        +--:(write-dei-0)
        |        |  +--rw write-dei-0?           empty
        |        +--:(copy-dei-from-input-or-0)
        |        |  +--rw dei-from-tag-index?    bbf-classif:tag-index
        |        +--:(write-dei-1)
        |        |  +--rw write-dei-1?           empty
        |        +--:(generate-dei-from-marking-list-or-0)
        |           +--rw dei-marking-index?     bbf-qos-cls:qos-dei-marking-index
        +--rw egress-rewrite!
           +--rw pop-tags?   uint8 {bbf-fpprof:vlan-id-translation}?
           +--rw push-tag* [index]
              +--rw index                        bbf-classif:tag-index
              +--rw tag-type?                    union
              +--rw vlan-id?                     union
              +--rw (pbit)
              |  +--:(write-pbit-0)
              |  |  +--rw write-pbit-0?          empty
              |  +--:(copy-pbit-from-input-or-0)
              |  |  +--rw pbit-from-tag-index?   bbf-classif:tag-index
              |  +--:(write-pbit-value) {bbf-subif-tag:write-pbit-value-in-vlan-tag}?
              |  |  +--rw write-pbit?            bbf-dot1qt:pbit
              |  +--:(generate-pbit-from-marking-list-or-0)
              |     +--rw pbit-marking-index?    bbf-qos-cls:qos-pbit-marking-index
              +--rw (dei)
                 +--:(write-dei-0)
                 |  +--rw write-dei-0?           empty
                 +--:(copy-dei-from-input-or-0)
                 |  +--rw dei-from-tag-index?    bbf-classif:tag-index
                 +--:(write-dei-1)
                 |  +--rw write-dei-1?           empty
                 +--:(generate-dei-from-marking-list-or-0)
                    +--rw dei-marking-index?     bbf-qos-cls:qos-dei-marking-index

  augment /if:interfaces/if:interface/bbf-subif:frame-processing:
    +--:(frame-processing-profile)
       +--rw frame-processing-profile?   -> /frame-processing-profiles/frame-processing-profile/name
       +--rw tag-0!
       |  +--rw vlan-id    bbf-dot1qt:vlan-id-or-0
       +--rw tag-1!
       |  +--rw vlan-id    bbf-dot1qt:vlan-id-or-0
       +--rw ingress-rewrite-tag-0! {bbf-fpprof:vlan-id-translation}?
       |  +--rw vlan-id    bbf-dot1qt:vlan-id-or-0
       +--rw ingress-rewrite-tag-1! {bbf-fpprof:vlan-id-translation}?
          +--rw vlan-id    bbf-dot1qt:vlan-id-or-0
```

- Module Dependencies: `bbf-yang-types` `bbf-dot1q-types` `ietf-interfaces` `bbf-sub-interfaces` `bbf-frame-classification` `bbf-sub-interface-tagging` `bbf-qos-classifiers`

## 8.9. bbf-hardware-cpu

```text
module: bbf-hardware-cpu

  augment /hw:hardware/hw:component:
    +--ro cpu-processor-data
       +--ro number-of-active-sessions?   yang:gauge32
       +--ro system-load
       |  +--ro average-system-load-1-min?    decimal64
       |  +--ro average-system-load-5-min?    decimal64
       |  +--ro average-system-load-15-min?   decimal64
       +--ro tasks
       |  +--ro total-tasks?      yang:gauge32
       |  +--ro running-tasks?    yang:gauge32
       |  +--ro sleeping-tasks?   yang:gauge32
       |  +--ro stopped-tasks?    yang:gauge32
       |  +--ro zombie-tasks?     yang:gauge32
       +--ro threads
       |  +--ro total-threads?     yang:gauge32
       |  +--ro run-threads?       yang:gauge32
       |  +--ro sleep-threads?     yang:gauge32
       |  +--ro stopped-threads?   yang:gauge32
       |  +--ro zombie-threads?    yang:gauge32
       +--ro cpu-usage
       |  +--ro cpu-core-processes?   bbf-yang:percent
       |  +--ro cpu-user?             bbf-yang:percent
       |  +--ro cpu-idle?             bbf-yang:percent
       |  +--ro cpu-hwio?             bbf-yang:percent
       |  +--ro cpu-io?               bbf-yang:percent
       |  +--ro cpu-nice?             bbf-yang:percent
       |  +--ro cpu-swint?            bbf-yang:percent
       +--ro memory-usage
          +--ro used-mem?            uint64
          +--ro total-memory?        uint64
          +--ro free-memory?         uint64
          +--ro buffer-memory?       uint64
          +--ro total-swap-memory?   uint64
          +--ro used-swap?           uint64
          +--ro free-swap?           uint64
          +--ro available-mem?       uint64
```

- Module Dependencies: `iana-hardware` `ietf-hardware` `ietf-yang-types` `bbf-yang-types`

## 8.10. bbf-hardware

```text
module: bbf-hardware

  augment /hw:hardware/hw:component:
    +--rw expected-model-name?   string {model-name-configuration}?
  augment /hw:hardware/hw:component:
    +---x reset {hardware-component-reset}?
    |  +---w input
    |     +---w reset-type?   identityref
    +---n component-reset
       +--rw reset-type?   identityref
       +--rw reason?       bbf-yang:string-ascii64
  augment /if:interfaces/if:interface:
    +--rw hardware-component! {interface-hardware-management}?
       +--rw port-layer-if*   -> /hw:hardware/component/name {interface-hardware-reference}?
```

- Module Dependencies: `bbf-yang-types` `iana-hardware` `ietf-hardware` `ietf-interfaces`

## 8.11. bbf-hardware-storage-drives

```text
module: bbf-hardware-storage-drives

  augment /hw:hardware/hw:component:
    +--ro storage-drive
       +--ro device-transfer-rate?     decimal64
       +--ro device-data-read-rate?    uint64
       +--ro device-data-write-rate?   uint64
       +--ro device-data-read?         uint64
       +--ro device-data-write?        uint64
```

- Module Dependencies: `iana-hardware` `ietf-hardware`

## 8.12. bbf-hardware-transceivers

```text
module: bbf-hardware-transceivers

  augment /hw:hardware/hw:component:
    +--rw transceiver
       +--rw identifiers-and-codes
       |  +--ro physical-device?                       identityref
       |  +--ro physical-device-extended-identifier?   enumeration
       |  +--ro connector?                             identityref
       |  +--rw compliance-codes
       |  |  +--ro ten-gigabit-ethernet?               enumeration
       |  |  +--ro infiniband?                         enumeration
       |  |  +--ro escon?                              enumeration
       |  |  +--ro sonet?                              enumeration
       |  |  +--ro ethernet?                           enumeration
       |  |  +--ro fibre-channel-link-length?          enumeration
       |  |  +--ro fibre-channel-technology?           enumeration
       |  |  +--ro sfp-plus-cable-technology?          enumeration
       |  |  +--ro fibre-channel-transmission-media?   enumeration
       |  |  +--ro fibre-channel-speed?                enumeration
       |  +--ro encoding?                              enumeration
       |  +--ro nominal-bit-rate?                      uint8
       |  +--ro maximum-bit-rate?                      uint8
       |  +--ro minimum-bit-rate?                      uint8
       +--rw link-length
       |  +--ro length-single-mode-km?           uint8
       |  +--ro length-single-mode-100m?         uint8
       |  +--ro length-50um-om2?                 uint8
       |  +--ro length-62.5um-om2?               uint8
       |  +--ro length-50um-om4?                 uint8
       |  +--ro length-active-cable-or-copper?   uint8
       |  +--ro length-50um-om3?                 uint8
       +--rw vendor-fields
       |  +--ro oui?           string
       |  +--ro part-number?   string
       |  +--ro eeprom {bbf-hw-xcvr:vendor-eeprom}?
       |     +--ro string?   string {bbf-hw-xcvr:vendor-eeprom-string}?
       |     +--ro binary?   binary {bbf-hw-xcvr:vendor-eeprom-binary}?
       +--ro diagnostics-monitoring-type?   bits
       +--ro enhanced-options?              bits
       +--rw diagnostics!
       |  +--ro temperature       int32
       |  +--ro supply-voltage    uint16
       |  +--ro alarm-flag*       identityref
       |  +--ro warning-flag*     identityref
       +--rw thresholds
       |  +--ro temperature-high-alarm?           int32
       |  +--ro temperature-low-alarm?            int32
       |  +--ro temperature-high-warning?         int32
       |  +--ro temperature-low-warning?          int32
       |  +--ro supply-voltage-high-alarm?        uint16
       |  +--ro supply-voltage-low-alarm?         uint16
       |  +--ro supply-voltage-high-warning?      uint16
       |  +--ro supply-voltage-low-warning?       uint16
       |  +--ro tx-bias-high-alarm?               uint32
       |  +--ro tx-bias-low-alarm?                uint32
       |  +--ro tx-bias-high-warning?             uint32
       |  +--ro tx-bias-low-warning?              uint32
       |  +--ro tx-power-high-alarm?              uint16
       |  +--ro tx-power-low-alarm?               uint16
       |  +--ro tx-power-high-warning?            uint16
       |  +--ro tx-power-low-warning?             uint16
       |  +--ro rx-power-high-alarm?              uint16
       |  +--ro rx-power-low-alarm?               uint16
       |  +--ro rx-power-high-warning?            uint16
       |  +--ro rx-power-low-warning?             uint16
       |  +--ro laser-temperature-high-alarm?     int32
       |  +--ro laser-temperature-low-alarm?      int32
       |  +--ro laser-temperature-high-warning?   int32
       |  +--ro laser-temperature-low-warning?    int32
       |  +--ro tec-current-high-alarm?           int16
       |  +--ro tec-current-low-alarm?            int16
       |  +--ro tec-current-high-warning?         int16
       |  +--ro tec-current-low-warning?          int16
       +--ro extended-information {bbf-hw-xcvr:extended-information}?
          +--ro user-eeprom {bbf-hw-xcvr:user-eeprom}?
             +--ro string?   string {bbf-hw-xcvr:user-eeprom-string}?
             +--ro binary?   binary {bbf-hw-xcvr:user-eeprom-binary}?
  augment /hw:hardware/hw:component:
    +--rw transceiver-link
       +--ro (wavelength-or-specification-compliance)?
       |  +--:(unspecified-wavelength-or-cable-specification-compliance)
       |  |  +--ro unspecified?                              empty
       |  +--:(wavelength)
       |  |  +--ro wavelength?                               uint32
       |  +--:(passive-cable-specification-compliance)
       |  |  +--ro passive-cable-specification-compliance?   enumeration
       |  +--:(active-cable-specification-compliance)
       |     +--ro active-cable-specification-compliance?    enumeration
       +--rw diagnostics!
          +--ro tx-bias              uint32
          +--ro (power)
          |  +--:(power-in-mwatt)
          |     +--ro tx-power       uint16
          |     +--ro rx-power       uint16
          +--ro laser-temperature?   int32
          +--ro tec-current?         int16
          +--ro alarm-flag*          identityref
          +--ro warning-flag*        identityref
```

- Module Dependencies: `ietf-hardware` `bbf-hardware-types`

## 8.13. bbf-hardware-transceivers-xpon

```text
module: bbf-hardware-transceivers-xpon

  augment /hw:hardware/hw:component/bbf-hw-xcvr:transceiver-link/bbf-hw-xcvr:diagnostics:
    +--ro rssi-onu* [detected-serial-number]
       +--ro detected-serial-number    bbf-xpon-types:onu-serial-number
       +--ro rssi                      bbf-xpon-types:int16-or-unknown
       +--ro v-ani-ref?                if:interface-ref
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-types` `ietf-hardware` `bbf-hardware-transceivers`

## 8.14. bbf-interfaces-performance-management

```text
module: bbf-interfaces-performance-management

  augment /if:interfaces/if:interface:
    +--rw performance
       +--rw enable?   boolean
  augment /if:interfaces-state/if:interface:
    +--ro performance
       +--ro intervals-15min
       |  +--ro current
       |  |  +--ro time-stamp?           yang:date-and-time {bbf-if-pm:current-interval-time-stamp}?
       |  |  +--ro in-octets?            bbf-yang:performance-counter64
       |  |  +--ro in-pkts?              bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
       |  |  +--ro in-unicast-pkts?      bbf-yang:performance-counter64
       |  |  +--ro in-broadcast-pkts?    bbf-yang:performance-counter64
       |  |  +--ro in-multicast-pkts?    bbf-yang:performance-counter64
       |  |  +--ro in-discards?          bbf-yang:performance-counter32
       |  |  +--ro in-errors?            bbf-yang:performance-counter32
       |  |  +--ro in-unknown-protos?    bbf-yang:performance-counter32
       |  |  +--ro out-octets?           bbf-yang:performance-counter64
       |  |  +--ro out-pkts?             bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
       |  |  +--ro out-unicast-pkts?     bbf-yang:performance-counter64
       |  |  +--ro out-broadcast-pkts?   bbf-yang:performance-counter64
       |  |  +--ro out-multicast-pkts?   bbf-yang:performance-counter64
       |  |  +--ro out-discards?         bbf-yang:performance-counter32
       |  |  +--ro out-errors?           bbf-yang:performance-counter32
       |  +--ro number-of-intervals?   performance-15min-interval
       |  +--ro non-valid-intervals?   performance-15min-interval
       |  +--ro history* [interval-number]
       |     +--ro interval-number       performance-15min-history-interval
       |     +--ro measured-time?        uint32
       |     +--ro invalid-data-flag?    boolean
       |     +--ro time-stamp?           yang:date-and-time
       |     +--ro in-octets?            bbf-yang:performance-counter64
       |     +--ro in-pkts?              bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
       |     +--ro in-unicast-pkts?      bbf-yang:performance-counter64
       |     +--ro in-broadcast-pkts?    bbf-yang:performance-counter64
       |     +--ro in-multicast-pkts?    bbf-yang:performance-counter64
       |     +--ro in-discards?          bbf-yang:performance-counter32
       |     +--ro in-errors?            bbf-yang:performance-counter32
       |     +--ro in-unknown-protos?    bbf-yang:performance-counter32
       |     +--ro out-octets?           bbf-yang:performance-counter64
       |     +--ro out-pkts?             bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
       |     +--ro out-unicast-pkts?     bbf-yang:performance-counter64
       |     +--ro out-broadcast-pkts?   bbf-yang:performance-counter64
       |     +--ro out-multicast-pkts?   bbf-yang:performance-counter64
       |     +--ro out-discards?         bbf-yang:performance-counter32
       |     +--ro out-errors?           bbf-yang:performance-counter32
       +--ro intervals-24hr {performance-24hr}?
          +--ro current
          |  +--ro time-stamp?           yang:date-and-time {bbf-if-pm:current-interval-time-stamp}?
          |  +--ro in-octets?            bbf-yang:performance-counter64
          |  +--ro in-pkts?              bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
          |  +--ro in-unicast-pkts?      bbf-yang:performance-counter64
          |  +--ro in-broadcast-pkts?    bbf-yang:performance-counter64
          |  +--ro in-multicast-pkts?    bbf-yang:performance-counter64
          |  +--ro in-discards?          bbf-yang:performance-counter32
          |  +--ro in-errors?            bbf-yang:performance-counter32
          |  +--ro in-unknown-protos?    bbf-yang:performance-counter32
          |  +--ro out-octets?           bbf-yang:performance-counter64
          |  +--ro out-pkts?             bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
          |  +--ro out-unicast-pkts?     bbf-yang:performance-counter64
          |  +--ro out-broadcast-pkts?   bbf-yang:performance-counter64
          |  +--ro out-multicast-pkts?   bbf-yang:performance-counter64
          |  +--ro out-discards?         bbf-yang:performance-counter32
          |  +--ro out-errors?           bbf-yang:performance-counter32
          +--ro number-of-intervals?   performance-24hr-interval
          +--ro non-valid-intervals?   performance-24hr-interval
          +--ro history* [interval-number]
             +--ro interval-number       performance-24hr-history-interval
             +--ro measured-time?        uint32
             +--ro invalid-data-flag?    boolean
             +--ro time-stamp?           yang:date-and-time
             +--ro in-octets?            bbf-yang:performance-counter64
             +--ro in-pkts?              bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
             +--ro in-unicast-pkts?      bbf-yang:performance-counter64
             +--ro in-broadcast-pkts?    bbf-yang:performance-counter64
             +--ro in-multicast-pkts?    bbf-yang:performance-counter64
             +--ro in-discards?          bbf-yang:performance-counter32
             +--ro in-errors?            bbf-yang:performance-counter32
             +--ro in-unknown-protos?    bbf-yang:performance-counter32
             +--ro out-octets?           bbf-yang:performance-counter64
             +--ro out-pkts?             bbf-yang:performance-counter64 {bbf-if-pm:total-pkts}?
             +--ro out-unicast-pkts?     bbf-yang:performance-counter64
             +--ro out-broadcast-pkts?   bbf-yang:performance-counter64
             +--ro out-multicast-pkts?   bbf-yang:performance-counter64
             +--ro out-discards?         bbf-yang:performance-counter32
             +--ro out-errors?           bbf-yang:performance-counter32
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-yang-types`

## 8.15. bbf-interfaces-remote-hardware-state

```text
module: bbf-interfaces-remote-hardware-state

  augment /if:interfaces-state/if:interface:
    +--ro remote-hardware!
       +--ro component*   -> /hw:hardware/component/name
```

- Module Dependencies: `ietf-interfaces` `ietf-hardware` `bbf-hardware-types`

## 8.16. bbf-interfaces-statistics-management

```text
module: bbf-interfaces-statistics-management

  augment /if:interfaces/if:interface:
    +--rw statistics-accumulation?   enumeration {interface-statistics-accumulation,interface-statistics-configuration}?
  augment /if:interfaces-state/if:interface:
    +---x reset-statistics
  augment /if:interfaces-state/if:interface/if:statistics:
    +--ro in-pkts?        yang:counter64 {bbf-if-ctrs:total-pkts}?
    +--ro out-pkts?       yang:counter64 {bbf-if-ctrs:total-pkts}?
    +--ro accumulating?   boolean {interface-statistics-accumulation}?
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types`

## 8.17. bbf-interface-usage

```text
module: bbf-interface-usage

  augment /if:interfaces/if:interface:
    +--rw interface-usage
       +--rw interface-usage?   interface-usage
```

- Module Dependencies: `ietf-interfaces` `iana-if-type` `bbf-if-type`

## 8.18. bbf-l2-dhcpv4-relay-forwarding

```text
module: bbf-l2-dhcpv4-relay-forwarding

  augment /bbf-l2-fwd:forwarding/bbf-l2-fwd:forwarders/bbf-l2-fwd:forwarder:
    +--rw l2-dhcpv4-relay!
       +--rw downstream-broadcast-flooding?   union
```

- Module Dependencies: `bbf-l2-forwarding`

## 8.19. bbf-l2-dhcpv4-relay

```text
module: bbf-l2-dhcpv4-relay
  +--rw l2-dhcpv4-relay-profiles
     +--rw l2-dhcpv4-relay-profile* [name]
        +--rw name               bbf-yang:string-ascii64
        +--rw max-packet-size?   uint16
        +--rw option82-format
           +--rw suboptions*                  enumeration
           +--rw default-circuit-id-syntax?   bbf-yang:string-ascii63-or-empty
           +--rw default-remote-id-syntax?    bbf-yang:string-ascii63-or-empty
           +--rw access-loop-suboptions?      bbf-subtype:broadband-line-characteristics
           +--rw start-numbering-from-zero?   boolean
           +--rw use-leading-zeroes?          boolean

  augment /if:interfaces/if:interface:
    +--rw l2-dhcpv4-relay!
       +--rw enable?         boolean
       +--rw trusted-port?   boolean
       +--rw profile-ref     -> /l2-dhcpv4-relay-profiles/l2-dhcpv4-relay-profile/name
  augment /if:interfaces-state/if:interface/if:statistics:
    +--ro dhcpv4!
       +--ro in-bad-packets-from-client?             yang:counter32
       +--ro in-bad-packets-from-server?             yang:counter32
       +--ro in-packets-from-client?                 yang:counter32
       +--ro in-packets-from-server?                 yang:counter32
       +--ro out-packets-to-server?                  yang:counter32
       +--ro out-packets-to-client?                  yang:counter32
       +--ro option-82-inserted-packets-to-server?   yang:counter32
       +--ro option-82-removed-packets-to-client?    yang:counter32
       +--ro option-82-not-inserted-to-server?       yang:counter32
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-yang-types` `bbf-if-type` `bbf-subscriber-types`

## 8.20. bbf-l2-forwarding-base

```text
submodule: bbf-l2-forwarding-base (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  +--ro forwarding-state
```

- Module Dependencies: `N/A`

## 8.21. bbf-l2-forwarding-flooding-policies

```text
submodule: bbf-l2-forwarding-flooding-policies (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name                 bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |     |  +--rw port-group* [name]
  |  |     |     +--rw name    bbf-yang:string-ascii64
  |  |     |     +--rw port*   -> ../../../ports/port/name
  |  |     +--rw flooding-policies {flooding-policies}?
  |  |        +--rw (flooding-policy-type)?
  |  |           +--:(profile-based) {flooding-policies-profiles}?
  |  |           |  +--rw flooding-policies-profile?   -> /forwarding/flooding-policies-profiles/flooding-policies-profile/name
  |  |           +--:(forwarder-specific)
  |  |              +--rw flooding-policy* [name]
  |  |                 +--rw name                   bbf-yang:string-ascii64
  |  |                 +--rw in-ports
  |  |                 |  +--rw (list-type)?
  |  |                 |     +--:(forwarder-port)
  |  |                 |     |  +--rw forwarder-port?         -> ../../../../ports/port/name
  |  |                 |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |                 |        +--rw forwarder-port-group?   -> ../../../../port-groups/port-group/name
  |  |                 +--rw destination-address
  |  |                 |  +--rw (frame-filter)?
  |  |                 |     +--:(any-frame)
  |  |                 |     |  +--rw any-frame?                                empty
  |  |                 |     +--:(destination-mac-address)
  |  |                 |     |  +--rw (mac-address)?
  |  |                 |     |     +--:(any-multicast-mac-address)
  |  |                 |     |     |  +--rw any-multicast-mac-address?          empty
  |  |                 |     |     +--:(unicast-address)
  |  |                 |     |     |  +--rw unicast-address?                    empty
  |  |                 |     |     +--:(broadcast-address)
  |  |                 |     |     |  +--rw broadcast-address?                  empty
  |  |                 |     |     +--:(cfm-multicast-address)
  |  |                 |     |     |  +--rw cfm-multicast-address?              empty
  |  |                 |     |     +--:(ipv4-multicast-address)
  |  |                 |     |     |  +--rw ipv4-multicast-address?             empty
  |  |                 |     |     +--:(ipv6-multicast-address)
  |  |                 |     |     |  +--rw ipv6-multicast-address?             empty
  |  |                 |     |     +--:(mac-address-filter)
  |  |                 |     |        +--rw mac-address-value?                  yang:mac-address
  |  |                 |     |        +--rw mac-address-mask?                   yang:mac-address
  |  |                 |     +--:(destination-ipv4-address)
  |  |                 |     |  +--rw (ipv4-address)?
  |  |                 |     |     +--:(any-multicast-ipv4-address)
  |  |                 |     |     |  +--rw any-multicast-ipv4-address?         empty
  |  |                 |     |     +--:(all-hosts-multicast-address)
  |  |                 |     |     |  +--rw all-hosts-multicast-address?        empty
  |  |                 |     |     +--:(rip-multicast-address)
  |  |                 |     |     |  +--rw rip-multicast-address?              empty
  |  |                 |     |     +--:(ntp-multicast-address)
  |  |                 |     |     |  +--rw ntp-multicast-address?              empty
  |  |                 |     |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |                 |     |        +--rw ipv4-prefix?                        inet:ipv4-prefix
  |  |                 |     +--:(destination-ipv6-address)
  |  |                 |        +--rw (ipv6-address)?
  |  |                 |           +--:(any-multicast-ipv6-address)
  |  |                 |           |  +--rw any-multicast-ipv6-address?         empty
  |  |                 |           +--:(all-nodes-multicast-ipv6-address)
  |  |                 |           |  +--rw all-nodes-multicast-ipv6-address?   empty
  |  |                 |           +--:(rip-multicast-ipv6-address)
  |  |                 |           |  +--rw rip-multicast-ipv6-address?         empty
  |  |                 |           +--:(ntp-multicast-ipv6-address)
  |  |                 |           |  +--rw ntp-multicast-ipv6-address?         empty
  |  |                 |           +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |                 |              +--rw ipv6-prefix?                        inet:ipv6-prefix
  |  |                 +--rw (frame-forwarding)?
  |  |                    +--:(discard)
  |  |                    |  +--rw discard?         empty
  |  |                    +--:(forward)
  |  |                       +--rw out-ports
  |  |                          +--rw (list-type)?
  |  |                             +--:(forwarder-port)
  |  |                             |  +--rw forwarder-port?         -> ../../../../ports/port/name
  |  |                             +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |                                +--rw forwarder-port-group?   -> ../../../../port-groups/port-group/name
  |  +--rw flooding-policies-profiles {flooding-policies-profiles}?
  |     +--rw flooding-policies-profile* [name]
  |        +--rw name               bbf-yang:string-ascii64
  |        +--rw flooding-policy* [name]
  |           +--rw name                          bbf-yang:string-ascii64
  |           +--rw in-interface-usages
  |           |  +--rw interface-usages*   bbf-if-usg:interface-usage
  |           +--rw destination-address
  |           |  +--rw (frame-filter)?
  |           |     +--:(any-frame)
  |           |     |  +--rw any-frame?                                empty
  |           |     +--:(destination-mac-address)
  |           |     |  +--rw (mac-address)?
  |           |     |     +--:(any-multicast-mac-address)
  |           |     |     |  +--rw any-multicast-mac-address?          empty
  |           |     |     +--:(unicast-address)
  |           |     |     |  +--rw unicast-address?                    empty
  |           |     |     +--:(broadcast-address)
  |           |     |     |  +--rw broadcast-address?                  empty
  |           |     |     +--:(cfm-multicast-address)
  |           |     |     |  +--rw cfm-multicast-address?              empty
  |           |     |     +--:(ipv4-multicast-address)
  |           |     |     |  +--rw ipv4-multicast-address?             empty
  |           |     |     +--:(ipv6-multicast-address)
  |           |     |     |  +--rw ipv6-multicast-address?             empty
  |           |     |     +--:(mac-address-filter)
  |           |     |        +--rw mac-address-value?                  yang:mac-address
  |           |     |        +--rw mac-address-mask?                   yang:mac-address
  |           |     +--:(destination-ipv4-address)
  |           |     |  +--rw (ipv4-address)?
  |           |     |     +--:(any-multicast-ipv4-address)
  |           |     |     |  +--rw any-multicast-ipv4-address?         empty
  |           |     |     +--:(all-hosts-multicast-address)
  |           |     |     |  +--rw all-hosts-multicast-address?        empty
  |           |     |     +--:(rip-multicast-address)
  |           |     |     |  +--rw rip-multicast-address?              empty
  |           |     |     +--:(ntp-multicast-address)
  |           |     |     |  +--rw ntp-multicast-address?              empty
  |           |     |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
  |           |     |        +--rw ipv4-prefix?                        inet:ipv4-prefix
  |           |     +--:(destination-ipv6-address)
  |           |        +--rw (ipv6-address)?
  |           |           +--:(any-multicast-ipv6-address)
  |           |           |  +--rw any-multicast-ipv6-address?         empty
  |           |           +--:(all-nodes-multicast-ipv6-address)
  |           |           |  +--rw all-nodes-multicast-ipv6-address?   empty
  |           |           +--:(rip-multicast-ipv6-address)
  |           |           |  +--rw rip-multicast-ipv6-address?         empty
  |           |           +--:(ntp-multicast-ipv6-address)
  |           |           |  +--rw ntp-multicast-ipv6-address?         empty
  |           |           +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
  |           |              +--rw ipv6-prefix?                        inet:ipv6-prefix
  |           +--rw (frame-forwarding)?
  |              +--:(discard)
  |              |  +--rw discard?                empty
  |              +--:(forward)
  |                 +--rw out-interface-usages
  |                    +--rw interface-usages*   bbf-if-usg:interface-usage
  +--ro forwarding-state
     +--ro forwarders
        +--ro forwarder* [name]
           +--ro name     bbf-yang:string-ascii64
           +--ro ports
              +--ro port* [name]
                 +--ro name             bbf-yang:string-ascii64
                 +--ro sub-interface?   if:interface-state-ref
```

- Module Dependencies: `bbf-yang-types` `bbf-interface-usage` `bbf-frame-classification` `bbf-l2-forwarding-base` `bbf-l2-forwarding-forwarders`

## 8.22. bbf-l2-forwarding-forwarders

```text
submodule: bbf-l2-forwarding-forwarders (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |     +--rw forwarder* [name]
  |        +--rw name           bbf-yang:string-ascii64
  |        +--rw ports
  |        |  +--rw port* [name]
  |        |     +--rw name             bbf-yang:string-ascii64
  |        |     +--rw sub-interface?   if:interface-ref
  |        +--rw port-groups {forwarder-port-groups}?
  |           +--rw port-group* [name]
  |              +--rw name    bbf-yang:string-ascii64
  |              +--rw port*   -> ../../../ports/port/name
  +--ro forwarding-state
     +--ro forwarders
        +--ro forwarder* [name]
           +--ro name     bbf-yang:string-ascii64
           +--ro ports
              +--ro port* [name]
                 +--ro name             bbf-yang:string-ascii64
                 +--ro sub-interface?   if:interface-state-ref
```

- Module Dependencies: `bbf-yang-types` `bbf-if-type` `ietf-interfaces` `bbf-l2-forwarding-base`

## 8.23. bbf-l2-forwarding-forwarding-databases

```text
submodule: bbf-l2-forwarding-forwarding-databases (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name           bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |        +--rw port-group* [name]
  |  |           +--rw name    bbf-yang:string-ascii64
  |  |           +--rw port*   -> ../../../ports/port/name
  |  +--rw forwarding-databases {forwarding-databases}?
  |     +--rw forwarding-database* [name]
  |        +--rw name                          bbf-yang:string-ascii64
  |        +--rw max-number-mac-addresses?     uint32
  |        +--rw aging-timer?                  uint32 {mac-learning}?
  |        +--rw shared-forwarding-database?   boolean {shared-forwarding-databases}?
  |        +--rw static-mac-address* [mac-address]
  |           +--rw mac-address                              yang:mac-address
  |           +--rw (learning-constraint)?
  |              +--:(do-not-learn-and-discard-frame)
  |              |  +--rw discard-frame?                     empty
  |              +--:(allowed-to-learn-on) {mac-learning}?
  |              |  +--rw (allow-to-learn-on)?
  |              |     +--:(forwarder-port)
  |              |     |  +--rw forwarder-port-ref
  |              |     |     +--rw forwarder?   forwarder-ref
  |              |     |     +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |              |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |              |        +--rw forwarder-port-group-ref
  |              |           +--rw forwarder?   forwarder-ref
  |              |           +--rw group?       -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/port-groups/port-group/name
  |              +--:(install-in-fdb-on)
  |                 +--rw (install-on)?
  |                    +--:(static-port)
  |                       +--rw static-forwarder-port-ref
  |                          +--rw forwarder?   forwarder-ref
  |                          +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  +--ro forwarding-state
     +--ro forwarders
     |  +--ro forwarder* [name]
     |     +--ro name                    bbf-yang:string-ascii64
     |     +--ro ports
     |     |  +--ro port* [name]
     |     |     +--ro name             bbf-yang:string-ascii64
     |     |     +--ro sub-interface?   if:interface-state-ref
     |     +--ro forwarding-databases {forwarding-databases}?
     |        +--ro forwarding-database?   -> /forwarding-state/forwarding-databases/forwarding-database/name
     +--ro forwarding-databases {forwarding-databases}?
        +--ro max-number-mac-addresses?   uint32 {read-system-fdb-capacity}?
        +--ro forwarding-database* [name]
           +--ro name             bbf-yang:string-ascii64
           +--ro mac-addresses
              +--ro mac-address* [mac-address]
                 +--ro mac-address        yang:mac-address
                 +--ro (learned-on)?
                    +--:(forwarder-port)
                       +--ro forwarder?   forwarder-state-ref
                       +--ro port?        -> /forwarding-state/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
```

- Module Dependencies: `bbf-yang-types` `ietf-yang-types` `bbf-l2-forwarding-base` `bbf-l2-forwarding-forwarders`

## 8.24. bbf-l2-forwarding-mac-learning-control

```text
submodule: bbf-l2-forwarding-mac-learning-control (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name           bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |        +--rw port-group* [name]
  |  |           +--rw name    bbf-yang:string-ascii64
  |  |           +--rw port*   -> ../../../ports/port/name
  |  +--rw forwarding-databases {forwarding-databases}?
  |  |  +--rw forwarding-database* [name]
  |  |     +--rw name                          bbf-yang:string-ascii64
  |  |     +--rw max-number-mac-addresses?     uint32
  |  |     +--rw aging-timer?                  uint32 {mac-learning}?
  |  |     +--rw shared-forwarding-database?   boolean {shared-forwarding-databases}?
  |  |     +--rw static-mac-address* [mac-address]
  |  |     |  +--rw mac-address                              yang:mac-address
  |  |     |  +--rw (learning-constraint)?
  |  |     |     +--:(do-not-learn-and-discard-frame)
  |  |     |     |  +--rw discard-frame?                     empty
  |  |     |     +--:(allowed-to-learn-on) {mac-learning}?
  |  |     |     |  +--rw (allow-to-learn-on)?
  |  |     |     |     +--:(forwarder-port)
  |  |     |     |     |  +--rw forwarder-port-ref
  |  |     |     |     |     +--rw forwarder?   forwarder-ref
  |  |     |     |     |     +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |  |     |     |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |     |     |        +--rw forwarder-port-group-ref
  |  |     |     |           +--rw forwarder?   forwarder-ref
  |  |     |     |           +--rw group?       -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/port-groups/port-group/name
  |  |     |     +--:(install-in-fdb-on)
  |  |     |        +--rw (install-on)?
  |  |     |           +--:(static-port)
  |  |     |              +--rw static-forwarder-port-ref
  |  |     |                 +--rw forwarder?   forwarder-ref
  |  |     |                 +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |  |     +--rw mac-learning-control {mac-learning and mac-learning-control-profiles,forwarding-databases}?
  |  |        +--rw mac-learning-control-profile?   -> /forwarding/mac-learning-control-profiles/mac-learning-control-profile/name
  |  |        +--rw generate-mac-learning-alarm?    boolean
  |  +--rw mac-learning-control-profiles {forwarding-databases and mac-learning and
mac-learning-control-profiles}?
  |     +--rw mac-learning-control-profile* [name]
  |        +--rw name                 bbf-yang:string-ascii64
  |        +--rw mac-learning-rule* [receiving-interface-usage]
  |           +--rw receiving-interface-usage    bbf-if-usg:interface-usage
  |           +--rw (mac-learning-action)?
  |              +--:(learn-and-translate)
  |              +--:(learn-but-do-not-move)
  |                 +--rw mac-can-not-move-to*   bbf-if-usg:interface-usage
  +--ro forwarding-state
     +--ro forwarders
     |  +--ro forwarder* [name]
     |     +--ro name                    bbf-yang:string-ascii64
     |     +--ro ports
     |     |  +--ro port* [name]
     |     |     +--ro name             bbf-yang:string-ascii64
     |     |     +--ro sub-interface?   if:interface-state-ref
     |     +--ro forwarding-databases {forwarding-databases}?
     |        +--ro forwarding-database?   -> /forwarding-state/forwarding-databases/forwarding-database/name
     +--ro forwarding-databases {forwarding-databases}?
        +--ro max-number-mac-addresses?   uint32 {read-system-fdb-capacity}?
        +--ro forwarding-database* [name]
           +--ro name             bbf-yang:string-ascii64
           +--ro mac-addresses
              +--ro mac-address* [mac-address]
                 +--ro mac-address        yang:mac-address
                 +--ro (learned-on)?
                    +--:(forwarder-port)
                       +--ro forwarder?   forwarder-state-ref
                       +--ro port?        -> /forwarding-state/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
```

- Module Dependencies: `bbf-yang-types` `bbf-interface-usage` `bbf-l2-forwarding-base` `bbf-l2-forwarding-forwarders` `bbf-l2-forwarding-forwarding-databases`

## 8.25. bbf-l2-forwarding-mac-learning

```text
submodule: bbf-l2-forwarding-mac-learning (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name            bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |     |  +--rw port-group* [name]
  |  |     |     +--rw name    bbf-yang:string-ascii64
  |  |     |     +--rw port*   -> ../../../ports/port/name
  |  |     +--rw mac-learning {forwarding-databases}?
  |  |        +--rw forwarding-database?   -> /forwarding/forwarding-databases/forwarding-database/name
  |  +--rw forwarding-databases {forwarding-databases}?
  |     +--rw forwarding-database* [name]
  |        +--rw name                          bbf-yang:string-ascii64
  |        +--rw max-number-mac-addresses?     uint32
  |        +--rw aging-timer?                  uint32 {mac-learning}?
  |        +--rw shared-forwarding-database?   boolean {shared-forwarding-databases}?
  |        +--rw static-mac-address* [mac-address]
  |           +--rw mac-address                              yang:mac-address
  |           +--rw (learning-constraint)?
  |              +--:(do-not-learn-and-discard-frame)
  |              |  +--rw discard-frame?                     empty
  |              +--:(allowed-to-learn-on) {mac-learning}?
  |              |  +--rw (allow-to-learn-on)?
  |              |     +--:(forwarder-port)
  |              |     |  +--rw forwarder-port-ref
  |              |     |     +--rw forwarder?   forwarder-ref
  |              |     |     +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |              |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |              |        +--rw forwarder-port-group-ref
  |              |           +--rw forwarder?   forwarder-ref
  |              |           +--rw group?       -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/port-groups/port-group/name
  |              +--:(install-in-fdb-on)
  |                 +--rw (install-on)?
  |                    +--:(static-port)
  |                       +--rw static-forwarder-port-ref
  |                          +--rw forwarder?   forwarder-ref
  |                          +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  +--ro forwarding-state
     +--ro forwarders
     |  +--ro forwarder* [name]
     |     +--ro name                    bbf-yang:string-ascii64
     |     +--ro ports
     |     |  +--ro port* [name]
     |     |     +--ro name             bbf-yang:string-ascii64
     |     |     +--ro sub-interface?   if:interface-state-ref
     |     +--ro forwarding-databases {forwarding-databases}?
     |        +--ro forwarding-database?   -> /forwarding-state/forwarding-databases/forwarding-database/name
     +--ro forwarding-databases {forwarding-databases}?
        +--ro max-number-mac-addresses?   uint32 {read-system-fdb-capacity}?
        +--ro forwarding-database* [name]
           +--ro name             bbf-yang:string-ascii64
           +--ro mac-addresses
              +--ro mac-address* [mac-address]
                 +--ro mac-address        yang:mac-address
                 +--ro (learned-on)?
                    +--:(forwarder-port)
                       +--ro forwarder?   forwarder-state-ref
                       +--ro port?        -> /forwarding-state/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name

  augment /if:interfaces/if:interface:
    +--rw mac-learning {forwarding-databases and mac-learning}?
       +--rw max-number-mac-addresses?         uint32
       +--rw number-committed-mac-addresses?   uint32
       +--rw mac-learning-enable?              boolean
       +--rw mac-learning-failure-action?      enumeration
```

- Module Dependencies: `iana-if-type` `bbf-if-type` `ietf-interfaces` `bbf-l2-forwarding-base` `bbf-l2-forwarding-forwarders` `bbf-l2-forwarding-forwarding-databases`

## 8.26. bbf-l2-forwarding

```text
module: bbf-l2-forwarding
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name                      bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |     |  +--rw port-group* [name]
  |  |     |     +--rw name    bbf-yang:string-ascii64
  |  |     |     +--rw port*   -> ../../../ports/port/name
  |  |     +--rw flooding-policies {flooding-policies}?
  |  |     |  +--rw (flooding-policy-type)?
  |  |     |     +--:(profile-based) {flooding-policies-profiles}?
  |  |     |     |  +--rw flooding-policies-profile?   -> /forwarding/flooding-policies-profiles/flooding-policies-profile/name
  |  |     |     +--:(forwarder-specific)
  |  |     |        +--rw flooding-policy* [name]
  |  |     |           +--rw name                   bbf-yang:string-ascii64
  |  |     |           +--rw in-ports
  |  |     |           |  +--rw (list-type)?
  |  |     |           |     +--:(forwarder-port)
  |  |     |           |     |  +--rw forwarder-port?         -> ../../../../ports/port/name
  |  |     |           |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |     |           |        +--rw forwarder-port-group?   -> ../../../../port-groups/port-group/name
  |  |     |           +--rw destination-address
  |  |     |           |  +--rw (frame-filter)?
  |  |     |           |     +--:(any-frame)
  |  |     |           |     |  +--rw any-frame?                                empty
  |  |     |           |     +--:(destination-mac-address)
  |  |     |           |     |  +--rw (mac-address)?
  |  |     |           |     |     +--:(any-multicast-mac-address)
  |  |     |           |     |     |  +--rw any-multicast-mac-address?          empty
  |  |     |           |     |     +--:(unicast-address)
  |  |     |           |     |     |  +--rw unicast-address?                    empty
  |  |     |           |     |     +--:(broadcast-address)
  |  |     |           |     |     |  +--rw broadcast-address?                  empty
  |  |     |           |     |     +--:(cfm-multicast-address)
  |  |     |           |     |     |  +--rw cfm-multicast-address?              empty
  |  |     |           |     |     +--:(ipv4-multicast-address)
  |  |     |           |     |     |  +--rw ipv4-multicast-address?             empty
  |  |     |           |     |     +--:(ipv6-multicast-address)
  |  |     |           |     |     |  +--rw ipv6-multicast-address?             empty
  |  |     |           |     |     +--:(mac-address-filter)
  |  |     |           |     |        +--rw mac-address-value?                  yang:mac-address
  |  |     |           |     |        +--rw mac-address-mask?                   yang:mac-address
  |  |     |           |     +--:(destination-ipv4-address)
  |  |     |           |     |  +--rw (ipv4-address)?
  |  |     |           |     |     +--:(any-multicast-ipv4-address)
  |  |     |           |     |     |  +--rw any-multicast-ipv4-address?         empty
  |  |     |           |     |     +--:(all-hosts-multicast-address)
  |  |     |           |     |     |  +--rw all-hosts-multicast-address?        empty
  |  |     |           |     |     +--:(rip-multicast-address)
  |  |     |           |     |     |  +--rw rip-multicast-address?              empty
  |  |     |           |     |     +--:(ntp-multicast-address)
  |  |     |           |     |     |  +--rw ntp-multicast-address?              empty
  |  |     |           |     |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |     |           |     |        +--rw ipv4-prefix?                        inet:ipv4-prefix
  |  |     |           |     +--:(destination-ipv6-address)
  |  |     |           |        +--rw (ipv6-address)?
  |  |     |           |           +--:(any-multicast-ipv6-address)
  |  |     |           |           |  +--rw any-multicast-ipv6-address?         empty
  |  |     |           |           +--:(all-nodes-multicast-ipv6-address)
  |  |     |           |           |  +--rw all-nodes-multicast-ipv6-address?   empty
  |  |     |           |           +--:(rip-multicast-ipv6-address)
  |  |     |           |           |  +--rw rip-multicast-ipv6-address?         empty
  |  |     |           |           +--:(ntp-multicast-ipv6-address)
  |  |     |           |           |  +--rw ntp-multicast-ipv6-address?         empty
  |  |     |           |           +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |     |           |              +--rw ipv6-prefix?                        inet:ipv6-prefix
  |  |     |           +--rw (frame-forwarding)?
  |  |     |              +--:(discard)
  |  |     |              |  +--rw discard?         empty
  |  |     |              +--:(forward)
  |  |     |                 +--rw out-ports
  |  |     |                    +--rw (list-type)?
  |  |     |                       +--:(forwarder-port)
  |  |     |                       |  +--rw forwarder-port?         -> ../../../../ports/port/name
  |  |     |                       +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |     |                          +--rw forwarder-port-group?   -> ../../../../port-groups/port-group/name
  |  |     +--rw mac-learning {forwarding-databases}?
  |  |     |  +--rw forwarding-database?   -> /forwarding/forwarding-databases/forwarding-database/name
  |  |     +--rw split-horizon-profiles {split-horizon-profiles}?
  |  |        +--rw split-horizon-profile?   -> /forwarding/split-horizon-profiles/split-horizon-profile/name
  |  +--rw flooding-policies-profiles {flooding-policies-profiles}?
  |  |  +--rw flooding-policies-profile* [name]
  |  |     +--rw name               bbf-yang:string-ascii64
  |  |     +--rw flooding-policy* [name]
  |  |        +--rw name                          bbf-yang:string-ascii64
  |  |        +--rw in-interface-usages
  |  |        |  +--rw interface-usages*   bbf-if-usg:interface-usage
  |  |        +--rw destination-address
  |  |        |  +--rw (frame-filter)?
  |  |        |     +--:(any-frame)
  |  |        |     |  +--rw any-frame?                                empty
  |  |        |     +--:(destination-mac-address)
  |  |        |     |  +--rw (mac-address)?
  |  |        |     |     +--:(any-multicast-mac-address)
  |  |        |     |     |  +--rw any-multicast-mac-address?          empty
  |  |        |     |     +--:(unicast-address)
  |  |        |     |     |  +--rw unicast-address?                    empty
  |  |        |     |     +--:(broadcast-address)
  |  |        |     |     |  +--rw broadcast-address?                  empty
  |  |        |     |     +--:(cfm-multicast-address)
  |  |        |     |     |  +--rw cfm-multicast-address?              empty
  |  |        |     |     +--:(ipv4-multicast-address)
  |  |        |     |     |  +--rw ipv4-multicast-address?             empty
  |  |        |     |     +--:(ipv6-multicast-address)
  |  |        |     |     |  +--rw ipv6-multicast-address?             empty
  |  |        |     |     +--:(mac-address-filter)
  |  |        |     |        +--rw mac-address-value?                  yang:mac-address
  |  |        |     |        +--rw mac-address-mask?                   yang:mac-address
  |  |        |     +--:(destination-ipv4-address)
  |  |        |     |  +--rw (ipv4-address)?
  |  |        |     |     +--:(any-multicast-ipv4-address)
  |  |        |     |     |  +--rw any-multicast-ipv4-address?         empty
  |  |        |     |     +--:(all-hosts-multicast-address)
  |  |        |     |     |  +--rw all-hosts-multicast-address?        empty
  |  |        |     |     +--:(rip-multicast-address)
  |  |        |     |     |  +--rw rip-multicast-address?              empty
  |  |        |     |     +--:(ntp-multicast-address)
  |  |        |     |     |  +--rw ntp-multicast-address?              empty
  |  |        |     |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |        |     |        +--rw ipv4-prefix?                        inet:ipv4-prefix
  |  |        |     +--:(destination-ipv6-address)
  |  |        |        +--rw (ipv6-address)?
  |  |        |           +--:(any-multicast-ipv6-address)
  |  |        |           |  +--rw any-multicast-ipv6-address?         empty
  |  |        |           +--:(all-nodes-multicast-ipv6-address)
  |  |        |           |  +--rw all-nodes-multicast-ipv6-address?   empty
  |  |        |           +--:(rip-multicast-ipv6-address)
  |  |        |           |  +--rw rip-multicast-ipv6-address?         empty
  |  |        |           +--:(ntp-multicast-ipv6-address)
  |  |        |           |  +--rw ntp-multicast-ipv6-address?         empty
  |  |        |           +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
  |  |        |              +--rw ipv6-prefix?                        inet:ipv6-prefix
  |  |        +--rw (frame-forwarding)?
  |  |           +--:(discard)
  |  |           |  +--rw discard?                empty
  |  |           +--:(forward)
  |  |              +--rw out-interface-usages
  |  |                 +--rw interface-usages*   bbf-if-usg:interface-usage
  |  +--rw forwarding-databases {forwarding-databases}?
  |  |  +--rw forwarding-database* [name]
  |  |     +--rw name                          bbf-yang:string-ascii64
  |  |     +--rw max-number-mac-addresses?     uint32
  |  |     +--rw aging-timer?                  uint32 {mac-learning}?
  |  |     +--rw shared-forwarding-database?   boolean {shared-forwarding-databases}?
  |  |     +--rw static-mac-address* [mac-address]
  |  |     |  +--rw mac-address                              yang:mac-address
  |  |     |  +--rw (learning-constraint)?
  |  |     |     +--:(do-not-learn-and-discard-frame)
  |  |     |     |  +--rw discard-frame?                     empty
  |  |     |     +--:(allowed-to-learn-on) {mac-learning}?
  |  |     |     |  +--rw (allow-to-learn-on)?
  |  |     |     |     +--:(forwarder-port)
  |  |     |     |     |  +--rw forwarder-port-ref
  |  |     |     |     |     +--rw forwarder?   forwarder-ref
  |  |     |     |     |     +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |  |     |     |     +--:(forwarder-port-group) {forwarder-port-groups}?
  |  |     |     |        +--rw forwarder-port-group-ref
  |  |     |     |           +--rw forwarder?   forwarder-ref
  |  |     |     |           +--rw group?       -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/port-groups/port-group/name
  |  |     |     +--:(install-in-fdb-on)
  |  |     |        +--rw (install-on)?
  |  |     |           +--:(static-port)
  |  |     |              +--rw static-forwarder-port-ref
  |  |     |                 +--rw forwarder?   forwarder-ref
  |  |     |                 +--rw port?        -> /forwarding/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
  |  |     +--rw mac-learning-control {mac-learning and mac-learning-control-profiles,forwarding-databases}?
  |  |        +--rw mac-learning-control-profile?   -> /forwarding/mac-learning-control-profiles/mac-learning-control-profile/name
  |  |        +--rw generate-mac-learning-alarm?    boolean
  |  +--rw split-horizon-profiles {split-horizon-profiles}?
  |  |  +--rw split-horizon-profile* [name]
  |  |     +--rw name             bbf-yang:string-ascii64
  |  |     +--rw split-horizon* [in-interface-usage]
  |  |        +--rw in-interface-usage     bbf-if-usg:interface-usage
  |  |        +--rw out-interface-usage*   bbf-if-usg:interface-usage
  |  +--rw mac-learning-control-profiles {forwarding-databases and mac-learning and
mac-learning-control-profiles}?
  |     +--rw mac-learning-control-profile* [name]
  |        +--rw name                 bbf-yang:string-ascii64
  |        +--rw mac-learning-rule* [receiving-interface-usage]
  |           +--rw receiving-interface-usage    bbf-if-usg:interface-usage
  |           +--rw (mac-learning-action)?
  |              +--:(learn-and-translate)
  |              +--:(learn-but-do-not-move)
  |                 +--rw mac-can-not-move-to*   bbf-if-usg:interface-usage
  +--ro forwarding-state
     +--ro forwarders
     |  +--ro forwarder* [name]
     |     +--ro name                    bbf-yang:string-ascii64
     |     +--ro ports
     |     |  +--ro port* [name]
     |     |     +--ro name             bbf-yang:string-ascii64
     |     |     +--ro sub-interface?   if:interface-state-ref
     |     +--ro forwarding-databases {forwarding-databases}?
     |        +--ro forwarding-database?   -> /forwarding-state/forwarding-databases/forwarding-database/name
     +--ro forwarding-databases {forwarding-databases}?
        +--ro max-number-mac-addresses?   uint32 {read-system-fdb-capacity}?
        +--ro forwarding-database* [name]
           +--ro name             bbf-yang:string-ascii64
           +--ro mac-addresses
              +--ro mac-address* [mac-address]
                 +--ro mac-address        yang:mac-address
                 +--ro (learned-on)?
                    +--:(forwarder-port)
                       +--ro forwarder?   forwarder-state-ref
                       +--ro port?        -> /forwarding-state/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name

  augment /if:interfaces/if:interface:
    +--rw mac-learning {forwarding-databases and mac-learning}?
       +--rw max-number-mac-addresses?         uint32
       +--rw number-committed-mac-addresses?   uint32
       +--rw mac-learning-enable?              boolean
       +--rw mac-learning-failure-action?      enumeration
```

- Module Dependencies: `bbf-l2-forwarding-base` `bbf-l2-forwarding-flooding-policies` `bbf-l2-forwarding-forwarders` `bbf-l2-forwarding-forwarding-databases` `bbf-l2-forwarding-mac-learning` `bbf-l2-forwarding-split-horizon-profiles` `bbf-l2-forwarding-mac-learning-control`

## 8.27. bbf-l2-forwarding-shared-fdb

```text
module: bbf-l2-forwarding-shared-fdb

  augment /bbf-l2-fwd:forwarding/bbf-l2-fwd:forwarding-databases/bbf-l2-fwd:forwarding-database/bbf-l2-fwd:static-mac-address/bbf-l2-fwd:learning-constraint/bbf-l2-fwd:allowed-to-learn-on/bbf-l2-fwd:allow-to-learn-on:
    +--:(ethernet-interfaces) {bbf-l2-fwd:shared-forwarding-databases,bbf-l2-fwd:forwarding-databases and
bbf-l2-fwd:mac-learning}?
       +--rw interface-references
          +--rw interface*   if:interface-ref
  augment /bbf-l2-fwd:forwarding/bbf-l2-fwd:forwarding-databases/bbf-l2-fwd:forwarding-database/bbf-l2-fwd:static-mac-address/bbf-l2-fwd:learning-constraint/bbf-l2-fwd:install-in-fdb-on/bbf-l2-fwd:install-on:
    +--:(static-ethernet-interface) {bbf-l2-fwd:shared-forwarding-databases,bbf-l2-fwd:forwarding-databases}?
       +--rw static-interface-reference
          +--rw interface?   if:interface-ref
  augment /bbf-l2-fwd:forwarding-state/bbf-l2-fwd:forwarding-databases/bbf-l2-fwd:forwarding-database/bbf-l2-fwd:mac-addresses/bbf-l2-fwd:mac-address/bbf-l2-fwd:learned-on:
    +--:(interface) {bbf-l2-fwd:shared-forwarding-databases,bbf-l2-fwd:forwarding-databases and
bbf-l2-fwd:mac-learning}?
       +--ro interface?         if:interface-state-ref
       +--ro forwarder-ports* [index]
          +--ro index        uint32
          +--ro forwarder?   forwarder-state-ref
          +--ro port?        -> /bbf-l2-fwd:forwarding-state/forwarders/forwarder[bbf-l2-fwd:name = current()/../forwarder]/ports/port/name
```

- Module Dependencies: `ietf-interfaces` `bbf-l2-forwarding`

## 8.28. bbf-l2-forwarding-split-horizon-profiles

```text
submodule: bbf-l2-forwarding-split-horizon-profiles (belongs-to bbf-l2-forwarding)
  +--rw forwarding
  |  +--rw forwarders
  |  |  +--rw forwarder* [name]
  |  |     +--rw name                      bbf-yang:string-ascii64
  |  |     +--rw ports
  |  |     |  +--rw port* [name]
  |  |     |     +--rw name             bbf-yang:string-ascii64
  |  |     |     +--rw sub-interface?   if:interface-ref
  |  |     +--rw port-groups {forwarder-port-groups}?
  |  |     |  +--rw port-group* [name]
  |  |     |     +--rw name    bbf-yang:string-ascii64
  |  |     |     +--rw port*   -> ../../../ports/port/name
  |  |     +--rw split-horizon-profiles {split-horizon-profiles}?
  |  |        +--rw split-horizon-profile?   -> /forwarding/split-horizon-profiles/split-horizon-profile/name
  |  +--rw split-horizon-profiles {split-horizon-profiles}?
  |     +--rw split-horizon-profile* [name]
  |        +--rw name             bbf-yang:string-ascii64
  |        +--rw split-horizon* [in-interface-usage]
  |           +--rw in-interface-usage     bbf-if-usg:interface-usage
  |           +--rw out-interface-usage*   bbf-if-usg:interface-usage
  +--ro forwarding-state
     +--ro forwarders
        +--ro forwarder* [name]
           +--ro name     bbf-yang:string-ascii64
           +--ro ports
              +--ro port* [name]
                 +--ro name             bbf-yang:string-ascii64
                 +--ro sub-interface?   if:interface-state-ref
```

- Module Dependencies: `bbf-yang-types` `bbf-interface-usage` `bbf-l2-forwarding-base` `bbf-l2-forwarding-forwarders`

## 8.29. bbf-l2-terminations

```text
module: bbf-l2-terminations

  augment /if:interfaces/if:interface:
    +--rw l2-termination
       +--rw ingress-rewrite
       |  +--rw pop-tags?   uint8
       +--rw egress-rewrite
          +--rw push-tag* [index]
             +--rw index       bbf-classif:tag-index
             +--rw tag-type    union
             +--rw vlan-id     union
             +--rw pbit?       bbf-dot1qt:pbit
```

- Module Dependencies: `ietf-interfaces` `bbf-if-type` `bbf-dot1q-types` `bbf-frame-classification`

## 8.30. bbf-ldra

```text
module: bbf-ldra
  +--rw dhcpv6-ldra-profiles
     +--rw dhcpv6-ldra-profile* [name]
        +--rw name       bbf-yang:string-ascii64
        +--rw options
           +--rw option*                        enumeration
           +--rw default-interface-id-syntax?   bbf-yang:string-ascii63-or-empty
           +--rw default-remote-id-syntax?      bbf-yang:string-ascii63-or-empty
           +--rw enterprise-number?             uint32
           +--rw access-loop-characteristics?   bbf-subtype:broadband-line-characteristics
           +--rw start-numbering-from-zero?     boolean
           +--rw use-leading-zeroes?            boolean

  augment /if:interfaces/if:interface:
    +--rw dhcpv6-ldra!
       +--rw enable?         boolean
       +--rw trusted-port?   boolean
       +--rw profile-ref     -> /dhcpv6-ldra-profiles/dhcpv6-ldra-profile/name
  augment /if:interfaces-state/if:interface/if:statistics:
    +--ro dhcpv6-ldra!
       +--ro in-bad-packets-from-client?                  yang:counter32
       +--ro in-bad-packets-from-server?                  yang:counter32
       +--ro option-17-inserted-packets-to-server?        yang:counter32
       +--ro option-17-removed-packets-to-client?         yang:counter32
       +--ro option-18-inserted-packets-to-server?        yang:counter32
       +--ro option-18-removed-packets-to-client?         yang:counter32
       +--ro option-37-inserted-packets-to-server?        yang:counter32
       +--ro option-37-removed-packets-to-client?         yang:counter32
       +--ro outgoing-mtu-exceeded-packets-from-client?   yang:counter32
```

- Module Dependencies: `ietf-interfaces` `bbf-yang-types` `ietf-yang-types` `bbf-if-type` `bbf-subscriber-types`

## 8.31. bbf-link-table

```text
module: bbf-link-table
  +--rw link-table
     +--rw link-table* [from-interface]
        +--rw from-interface    if:interface-ref
        +--rw to-interface*     if:interface-ref
```

- Module Dependencies: `ietf-interfaces`

## 8.32. bbf-mgmd

```text
module: bbf-mgmd
  +--rw multicast
  |  +--rw mgmd
  |     +--rw global-administrative-enabled?                  boolean
  |     +--rw preview-parameters-profile* [name] {multicast-preview}?
  |     |  +--rw name                                bbf-yang:string-ascii64
  |     |  +--rw preview-control-style?              enumeration
  |     |  +--rw preview-pattern
  |     |  |  +--rw preview-repeat-interval?   uint32
  |     |  |  +--rw preview-repeat-count?      usage-limit
  |     |  |  +--rw preview-clip-length?       uint32
  |     |  +--rw preview-time?                       uint32
  |     |  +--rw preview-permission-restore-cycle?   uint32
  |     +--rw multicast-snoop-transparent-profile* [name] {mgmd-snoop-transparent}?
  |     |  +--rw name                        bbf-yang:string-ascii64
  |     |  +--rw protocol-version            bbf-mgmd-types:mgmd-protocol-and-version
  |     |  +--rw interfaces-to-hosts-data
  |     |     +--rw immediate-leave?              enumeration
  |     |     +--rw group-membership-interval?    uint32
  |     |     +--rw last-member-query-interval?   uint32 {bbf-mgmd:mgmd-snoop-last-leave}?
  |     |     +--rw last-member-query-count?      uint32 {bbf-mgmd:mgmd-snoop-last-leave}?
  |     +--rw multicast-snoop-with-proxy-reporting-profile* [name] {mgmd-snoop-with-proxy-reporting}?
  |     |  +--rw name                        bbf-yang:string-ascii64
  |     |  +--rw protocol-version            bbf-mgmd-types:mgmd-protocol-and-version
  |     |  +--rw interfaces-to-hosts-data
  |     |  |  +--rw immediate-leave?              enumeration
  |     |  |  +--rw group-membership-interval?    uint32
  |     |  |  +--rw last-member-query-interval?   uint32 {bbf-mgmd:mgmd-snoop-last-leave}?
  |     |  |  +--rw last-member-query-count?      uint32 {bbf-mgmd:mgmd-snoop-last-leave}?
  |     |  +--rw interface-to-router-data
  |     |     +--rw unsolicited-report-interval?   uint32
  |     |     +--rw robustness?                    uint32
  |     +--rw multicast-proxy-profile* [name] {mgmd-proxy}?
  |     |  +--rw name                        bbf-yang:string-ascii64
  |     |  +--rw protocol-version            bbf-mgmd-types:mgmd-protocol-and-version
  |     |  +--rw interfaces-to-hosts-data
  |     |  |  +--rw query-interval?               uint32
  |     |  |  +--rw query-max-response-time?      uint32
  |     |  |  +--rw last-member-query-interval?   uint32
  |     |  |  +--rw last-member-query-count?      uint32
  |     |  |  +--rw startup-query-interval?       uint32
  |     |  |  +--rw startup-query-count?          uint32
  |     |  |  +--rw immediate-leave?              enumeration
  |     |  |  +--rw robustness?                   uint32
  |     |  +--rw interface-to-router-data
  |     |     +--rw unsolicited-report-interval?   uint32
  |     |     +--rw robustness?                    uint32
  |     +--rw multicast-vpn* [name]
  |     |  +--rw name                                                 bbf-yang:string-ascii64
  |     |  +--rw vpn-administrative-enabled?                          boolean
  |     |  +--rw mode                                                 identityref
  |     |  +--rw multicast-proxy-profile-name?                        -> /multicast/mgmd/multicast-proxy-profile/name {mgmd-proxy}?
  |     |  +--rw multicast-snoop-transparent-profile-name?            -> /multicast/mgmd/multicast-snoop-transparent-profile/name {mgmd-snoop-transparent}?
  |     |  +--rw multicast-snoop-with-proxy-reporting-profile-name?   -> /multicast/mgmd/multicast-snoop-with-proxy-reporting-profile/name {mgmd-snoop-with-proxy-reporting}?
  |     |  +--rw ip-version?                                          inet:ip-version
  |     |  +--rw (ip-to-host)?
  |     |  |  +--:(ip-address)
  |     |  |     +--rw ipv4-address?                                  inet:ipv4-address
  |     |  |     x--rw ipv6-address?                                  inet:ipv6-address
  |     |  +--rw unmatched-join-processing?                           identityref
  |     |  +--rw network-interface-for-unmatched-reports              -> ../multicast-network-interface/name
  |     |  +--rw multicast-interface-to-host* [name]
  |     |  |  +--rw name                                        bbf-yang:string-ascii64
  |     |  |  +--rw vlan-sub-interface                          if:interface-ref
  |     |  |  +--rw data-path-vlan-sub-interface?               if:interface-ref
  |     |  |  +--rw interface-to-host-administrative-enabled?   boolean
  |     |  |  +--rw max-group-number?                           usage-limit
  |     |  |  +--rw maximum-concurrent-devices-per-channel?     usage-limit
  |     |  |  +--rw multicast-rate-limit?                       uint32 {multicast-cac}?
  |     |  |  +--rw multicast-rate-limit-exceed-action?         rate-limit-action-enum {multicast-cac}?
  |     |  |  +--rw multicast-package*                          -> ../../multicast-package/name {multicast-package}?
  |     |  |  +---x clear-host-interface-statistics
  |     |  +--rw multicast-network-interface* [name]
  |     |  |  +--rw name                                  bbf-yang:string-ascii64
  |     |  |  +--rw (multicast-transport)
  |     |  |  |  +--:(single-uplink)
  |     |  |  |     +--rw single-uplink-interface-data
  |     |  |  |        +--rw vlan-sub-interface?   if:interface-ref
  |     |  |  +--rw (ip-layer)?
  |     |  |  |  +--:(ip-address)
  |     |  |  |     +--rw ipv4-address?                   inet:ipv4-address
  |     |  |  |     x--rw ipv6-address?                   inet:ipv6-address
  |     |  |  +---x clear-network-interface-statistics
  |     |  +--rw multicast-channel* [name]
  |     |  |  +--rw name                 bbf-yang:string-ascii64
  |     |  |  +--rw network-interface    -> ../../multicast-network-interface/name
  |     |  |  +--rw ipv4
  |     |  |  |  +--rw source-ipv4-address?      inet:ipv4-address
  |     |  |  |  +--rw group-ipv4-address        inet:ipv4-address
  |     |  |  |  +--rw group-ipv4-address-end?   inet:ipv4-address {mgmd-group-ip-range}?
  |     |  |  +--rw ipv6
  |     |  |  |  +--rw source-ipv6-address?      inet:ipv6-address
  |     |  |  |  +--rw group-ipv6-address        inet:ipv6-address
  |     |  |  |  +--rw group-ipv6-address-end?   inet:ipv6-address {mgmd-group-ip-range}?
  |     |  |  +--rw channel-rate?        uint32 {multicast-cac}?
  |     |  |  +--rw interface-to-host*   -> ../../multicast-interface-to-host/name {static-multicast-host-associations}?
  |     |  +--rw multicast-package* [name] {multicast-package}?
  |     |  |  +--rw name                                   bbf-yang:string-ascii64
  |     |  |  +--rw multicast-channel-admission-control* [multicast-channel-name]
  |     |  |     +--rw multicast-channel-name    -> ../../../multicast-channel/name
  |     |  |     +--rw access-right?             enumeration
  |     |  |     +--rw access-parameters
  |     |  |        +--rw preview-parameter?   -> /multicast/mgmd/preview-parameters-profile/name {bbf-mgmd:multicast-preview}?
  |     |  +---x clear-vpn-statistics
  |     +---x clear-global-statistics
  +--ro multicast-state
     +--ro mgmd
        +--ro multicast-vpn* [name]
           +--ro name                           bbf-yang:string-ascii64
           +--ro mode                           identityref
           +--ro multicast-interface-to-host* [name]
           |  +--ro name                                   bbf-yang:string-ascii64
           |  +--ro interface-to-host-rx-state-data
           |  |  +--ro in-successful-join-requests?     yang:counter32
           |  |  +--ro in-unsuccessful-join-requests?   yang:counter32
           |  |  +--ro in-leaves?                       yang:counter32
           |  |  +--ro in-valid-messages?               yang:counter32
           |  |  +--ro in-invalid-messages?             yang:counter32
           |  +--ro interface-to-hosts-tx-state-data
           |  |  +--ro out-specific-queries?   yang:counter32
           |  |  +--ro out-general-queries?    yang:counter32
           |  +--ro multicast-rate-limit-exceeded-count?   yang:counter32 {multicast-cac}?
           |  +--ro current-multicast-bw-delivered?        uint32 {multicast-cac}?
           |  +--ro number-active-multicast-channels?      uint32
           +--ro multicast-network-interface* [name]
           |  +--ro name                                                  bbf-yang:string-ascii64
           |  +--ro (multicast-transport)
           |     +--:(single-uplink)
           |        +--ro single-uplink-interface-to-router-state-data
           |           +--ro vlan-sub-interface?                     if:interface-state-ref
           |           +--ro out-joins?                              yang:counter32
           |           +--ro out-leaves?                             yang:counter32
           |           +--ro in-general-queries?                     yang:counter32
           |           +--ro in-specific-queries?                    yang:counter32
           |           +--ro in-valid-messages?                      yang:counter32
           |           +--ro in-invalid-message?                     yang:counter32
           |           +--ro in-interface-wrong-version-queries?     yang:counter32
           |           +--ro host-interface-version2-querier-time?   yang:timeticks
           +--ro multicast-proxy-or-snooper
           |  +--ro number-active-groups?   uint32
           +--ro active-channel* [source-address group-address]
              +--ro source-address                       inet:ip-address
              +--ro group-address                        inet:ip-address
              +--ro name?                                bbf-yang:string-ascii64
              +--ro multicast-network-interface?         -> ../../multicast-network-interface/name
              +--ro number-active-interfaces-to-hosts?   uint32
              +--ro multicast-interface-to-host* [name]
              |  +--ro name                     -> ../../../multicast-interface-to-host/name
              |  +--ro host-reporter-address*   inet:ip-address
              +--ro uptime?                              yang:timeticks
```

- Module Dependencies: `ietf-yang-types` `bbf-yang-types` `bbf-mgmd-types` `ietf-interfaces` `ietf-inet-types` `bbf-if-type` `bbf-mgmd-configuration-interface-to-router` `bbf-mgmd-configuration-interface-to-host` `bbf-mgmd-configuration-multicast-snoop` `bbf-mgmd-operational-interface-to-host` `bbf-mgmd-operational-interface-to-router`

## 8.33. bbf-mgmd-mrd

```text
module: bbf-mgmd-mrd

  augment /bbf-mgmd:multicast/bbf-mgmd:mgmd/bbf-mgmd:multicast-vpn:
    +--rw mgmd-mrd
       +--rw neighbor-dead-interval?   uint32
       +--rw (ip-layer)?
          +--:(ip-address)
             +--rw ipv4-address?       inet:ipv4-address
  augment /bbf-mgmd:multicast-state/bbf-mgmd:mgmd/bbf-mgmd:multicast-vpn:
    +--ro mgmd-mrd-statistics
       +--ro out-solicit?        yang:counter32
       +--ro in-termination?     yang:counter32
       +--ro in-advertisement?   yang:counter32
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types` `bbf-mgmd`

## 8.34. bbf-network-map

```text
module: bbf-network-map

  augment /nw:networks/nw:network:
    +--rw tenant-id?   string {bbf-nm:reports-network-map}?
  augment /nw:networks/nw:network/nw:node:
    +--rw node-type?                   identityref {bbf-nm:reports-network-map}?
    +--rw network-device-properties {bbf-nm:reports-network-map}?
    |  +--rw alias?        bbf-yang:string-ascii64
    |  +--rw device-ref?   -> /bbf-inv:equipment-inventory/device-inventory/device/device-id {bbf-inv:reports-equipment-inventory,bbf-inv:reports-device-inventory}?
    +--rw enduser-device-properties {bbf-nm:reports-network-map}?
    |  +--rw end-users
    |  |  +--rw end-user* [end-user-id]
    |  |     +--rw end-user-id    bbf-yang:string-ascii64
    |  |     +--rw given-name?    bbf-yang:string-ascii64
    |  |     +--rw family-name?   bbf-yang:string-ascii64
    |  |     +--rw contact* [type]
    |  |        +--rw type     bbf-yang:string-ascii64
    |  |        +--rw value?   bbf-yang:string-ascii64
    |  +--rw device-ref?   -> /bbf-inv:equipment-inventory/eud-inventory/eud-equipment/eud-id {bbf-inv:reports-equipment-inventory,bbf-inv:reports-end-user-device-inventory}?
    +--rw geographic-location {bbf-nm:reports-network-map}?
    |  +--rw civic-address
    |  |  +--rw civic-address* [label value]
    |  |     +--rw label    enumeration
    |  |     +--rw value    string
    |  +--rw wgs84-location {bbf-loc:wgs84-location-support}?
    |     +--rw latitude?    string
    |     +--rw longitude?   string
    +--rw responsible-party-contact {bbf-nm:reports-network-map}?
    |  +--rw given-name?    bbf-yang:string-ascii64
    |  +--rw family-name?   bbf-yang:string-ascii64
    |  +--rw contact* [type]
    |     +--rw type     bbf-yang:string-ascii64
    |     +--rw value?   bbf-yang:string-ascii64
    +--rw site {bbf-nm:reports-network-map}?
    |  +--rw site-type?        identityref
    |  +--rw plant-type?       identityref
    |  +--rw building?         bbf-yang:string-ascii64
    |  +--rw floor?            bbf-yang:string-ascii64
    |  +--rw room?             bbf-yang:string-ascii64
    |  +--rw line?             uint16
    |  +--rw rack-position?    uint16
    |  +--rw frame-position?   uint16
    +--rw deployment-topology {bbf-nm:reports-network-map}?
       +--rw deployment-type?   identityref
       +--rw connected-node* [network-ref node-ref]
          +--rw network-ref    -> /nw:networks/network/supporting-network/network-ref
          +--rw node-ref       -> /nw:networks/network/node/node-id
  augment /nw:networks/nw:network/nw:node/nt:termination-point:
    +--rw distribution-frame-type?   identityref {bbf-nm:reports-network-map}?
    +--rw geographic-location {bbf-nm:reports-network-map}?
       +--rw civic-address
       |  +--rw civic-address* [label value]
       |     +--rw label    enumeration
       |     +--rw value    string
       +--rw wgs84-location {bbf-loc:wgs84-location-support}?
          +--rw latitude?    string
          +--rw longitude?   string
```

- Module Dependencies: `ietf-network` `ietf-network-topology` `bbf-location-types` `bbf-network-types` `bbf-node-types` `bbf-location` `bbf-device` `bbf-contact` `bbf-end-user` `bbf-equipment-inventory`

## 8.35. bbf-olt-vomci-grpc-client

```text
module: bbf-olt-vomci-grpc-client

  augment /bbf-olt-vomci:remote-nf/bbf-olt-vomci:nf-client/bbf-olt-vomci:initiate/bbf-olt-vomci:remote-server/bbf-olt-vomci:transport:
    +--:(grpc)
       +--rw grpc-client {bbf-grpcc:grpc-client}?
          +--rw channel
          |  +--rw ping-interval?   uint32
          +--rw connection-backoff {bbf-grpcc:connection-backoff}?
          |  +--rw enable?                boolean
          |  +--rw initial-backoff?       uint16
          |  +--rw min-connect-timeout?   uint16
          |  +--rw multiplier?            decimal64
          |  +--rw jitter?                decimal64
          |  +--rw max-backoff?           uint16
          +--rw access-point* [name]
             +--rw name                             bbf-yang:string-ascii64
             +--rw grpc-transport-parameters
             |  +--rw (tcp-client-options)?
             |     +--:(remote-port)
             |        +--rw remote-port
             |           +--rw remote-port?   inet:port-number
             +---n remote-endpoint-status-change
                +-- connected       boolean
                +-- last-changed    yang:date-and-time
```

- Module Dependencies: `bbf-grpc-client` `bbf-olt-vomci`

## 8.36. bbf-olt-vomci-grpc-server

```text
module: bbf-olt-vomci-grpc-server

  augment /bbf-olt-vomci:remote-nf/bbf-olt-vomci:nf-server/bbf-olt-vomci:listen/bbf-olt-vomci:listen-endpoint/bbf-olt-vomci:transport:
    +--:(grpc)
       +--rw grpc-server {bbf-grpcs:grpc-server}?
          +--rw (tcp-client-options)?
             +--:(remote-port)
                +--rw remote-port?   inet:port-number
```

- Module Dependencies: `bbf-grpc-server` `bbf-olt-vomci`

## 8.37. bbf-olt-vomci

```text
module: bbf-olt-vomci
  +--rw remote-nf!
     +--rw nf-client {bbf-olt-vomci:nf-client}?
     |  +--rw enabled?    boolean
     |  +--rw initiate
     |     +--rw remote-server* [name]
     |        +--rw name                      bbf-yang:string-ascii64
     |        +--rw nf-type?                  identityref
     |        +--rw on-demand?                boolean
     |        +--rw local-service-endpoint?   bbf-yang:string-ascii64
     |        +--rw (transport)
     +--rw nf-server {bbf-olt-vomci:nf-server}?
     |  +--rw enabled?   boolean
     |  +--rw listen!
     |     +--rw idle-timeout?      uint16
     |     +--rw listen-endpoint* [name]
     |        +--rw name                      bbf-yang:string-ascii64
     |        +--rw local-service-endpoint?   bbf-yang:string-ascii64
     |        +--rw (transport)
     |        +--ro remote-clients
     |           +--ro remote-client* [local-service-endpoint]
     |           |  +--ro local-service-endpoint    bbf-yang:string-ascii64
     |           +---n remote-client-status-change
     |              +-- remote-client                        -> ../../../remote-clients/remote-client/local-service-endpoint
     |              +-- connected                            boolean
     |              +-- remote-endpoint-state-last-change    yang:date-and-time
     +--rw nf-endpoint-filter
        +--rw rule* [name]
           +--rw name              bbf-yang:string-ascii64
           +--rw priority          uint16
           +--rw match-criteria
           |  +--rw criteria-type?   identityref
           |  +--rw onu-vendor       bbf-vomcit:onu-vendor-id
           +--rw endpoint?         bbf-yang:string-ascii64

  augment /if:interfaces/if:interface/bbf-xponvani:v-ani:
    +--rw vomci-onu
       +--rw vomci-function
          +--rw remote-endpoint?   bbf-yang:string-ascii64
```

- Module Dependencies: `bbf-network-function-client` `bbf-network-function-server` `bbf-vomci-types` `bbf-xponvani` `bbf-yang-types` `ietf-interfaces`

## 8.38. bbf-olt-vomci-state

```text
module: bbf-olt-vomci-state

  augment /if:interfaces-state/if:interface/bbf-xponvani:v-ani:
    +--ro vomci-onu
       +--ro vomci-function
       |  +--ro remote-endpoint?        bbf-yang:string-ascii64
       |  +--ro communication-status?   identityref
       +--ro statistics
          +--ro out-messages?       yang:counter64
          +--ro in-messages?        yang:counter64
          +--ro errored-messages?   yang:counter64
```

- Module Dependencies: `bbf-vomci-common` `bbf-vomci-types` `bbf-xponvani` `bbf-yang-types` `ietf-interfaces`

## 8.39. bbf-pppoe-intermediate-agent

```text
module: bbf-pppoe-intermediate-agent
  +--rw pppoe-profiles
     +--rw pppoe-profile* [name]
        +--rw name                         bbf-yang:string-ascii64
        +--rw pppoe-vendor-specific-tag
           +--rw subtag*                      enumeration
           +--rw default-circuit-id-syntax?   bbf-yang:string-ascii63-or-empty
           +--rw default-remote-id-syntax?    bbf-yang:string-ascii63-or-empty
           +--rw access-loop-subtags?         bbf-subtype:broadband-line-characteristics
           +--rw start-numbering-from-zero?   boolean
           +--rw use-leading-zeroes?          boolean

  augment /if:interfaces/if:interface:
    +--rw pppoe!
       +--rw enable?        boolean
       +--rw profile-ref    -> /pppoe-profiles/pppoe-profile/name
  augment /if:interfaces-state/if:interface/if:statistics:
    +--ro pppoe!
       +--ro in-error-packets-from-client?                     yang:counter32
       +--ro in-error-packets-from-server?                     yang:counter32
       +--ro in-packets-from-client?                           yang:counter32
       +--ro in-packets-from-server?                           yang:counter32
       +--ro out-packets-to-server?                            yang:counter32
       +--ro out-packets-to-client?                            yang:counter32
       +--ro vendor-specific-tag-inserted-packets-to-server?   yang:counter32
       +--ro vendor-specific-tag-removed-packets-to-client?    yang:counter32
       +--ro outgoing-mtu-exceeded-packets-from-client?        yang:counter32
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-yang-types` `bbf-if-type` `bbf-subscriber-types`

## 8.40. bbf-qos-classifiers

```text
module: bbf-qos-classifiers
  +--rw classifiers
     +--ro system-default-actions
     |  +--ro scheduling-traffic-class?   union
     |  +--ro flow-color?                 union
     |  +--ro bac-color?                  union
     +--rw configurable-default-actions {qos-configurable-defaults}?
     |  +--rw scheduling-traffic-class?   union
     |  +--rw flow-color?                 union
     |  +--rw bac-color?                  union
     +--rw classifier-entry* [name]
        +--rw name                           bbf-yang:string-ascii64
        +--rw description?                   bbf-yang:string-ascii64-or-empty
        x--rw filter-operation?              identityref
        +--rw (filter-method)?
        |  x--:(inline)
        |  |  x--rw match-criteria
        |  |     x--rw (vlan-tag-match-type)?
        |  |     |  x--:(match-all)
        |  |     |  |  x--rw match-all?      empty
        |  |     |  x--:(untagged)
        |  |     |  |  x--rw untagged?       empty
        |  |     |  x--:(vlan-tagged)
        |  |     |     x--rw tag* [index]
        |  |     |        x--rw index           bbf-classif:tag-index
        |  |     |        x--rw in-pbit-list?   bbf-dot1qt:pbit-list
        |  |     |        x--rw in-dei?         uint8
        |  |     x--rw dscp-range?           bbf-classif:dscp-range-or-any
        |  |     x--rw (protocols)?
        |  |        x--:(any-protocol)
        |  |        |  x--rw any-protocol?   empty
        |  |        x--:(protocol)
        |  |           x--rw protocol*       bbf-classif:protocols
        |  +--:(all-packets)
        |     +--rw all-packets?             empty
        +--rw classifier-action-entry-cfg* [action-type]
           +--rw action-type                       identityref
           +--rw (action-cfg-params)?
              +--:(pbit-marking)
              |  +--rw pbit-marking-cfg
              |     +--rw pbit-marking-list* [index]
              |        +--rw index         qos-pbit-marking-index
              |        +--rw pbit-value?   bbf-dot1qt:pbit
              +--:(dei-marking)
              |  +--rw dei-marking-cfg
              |     +--rw dei-marking-list* [index]
              |        +--rw index        qos-dei-marking-index
              |        +--rw dei-value?   bbf-dot1qt:dei
              +--:(dscp-marking)
              |  +--rw dscp-marking-cfg
              |     +--rw dscp?   inet:dscp
              +--:(scheduling-traffic-class)
              |  +--rw scheduling-traffic-class?   bbf-qos-t:traffic-class-id
              +--:(permit) {bbf-qos-cls:qos-action-permit}?
                 +--rw permit?                     empty
```

- Module Dependencies: `ietf-inet-types` `bbf-yang-types` `bbf-dot1q-types` `bbf-frame-classification` `bbf-qos-types`

## 8.41. bbf-qos-composite-filters

```text
module: bbf-qos-composite-filters
  +--rw composite-filters
     +--rw composite-filter* [name]
        +--rw name                bbf-yang:string-ascii64
        +--rw description?        bbf-yang:string-ascii64-or-empty
        +--rw filter-operation?   identityref
        +--rw filter* [name]
           +--rw name                             bbf-yang:string-ascii64
           +--rw description?                     bbf-yang:string-ascii64-or-empty
           +--rw (filter-method)?
              +--:(by-reference)
              |  +--rw composite-filter-name?     -> /composite-filters/composite-filter/name
              +--:(inline)
                 +--rw source-mac-address!
                 |  +--rw filter-match?                      boolean
                 |  +--rw (mac-address)
                 |     +--:(any-multicast-mac-address)
                 |     |  +--rw any-multicast-mac-address?   empty
                 |     +--:(unicast-address)
                 |     |  +--rw unicast-address?             empty
                 |     +--:(broadcast-address)
                 |     |  +--rw broadcast-address?           empty
                 |     +--:(cfm-multicast-address)
                 |     |  +--rw cfm-multicast-address?       empty
                 |     +--:(ipv4-multicast-address)
                 |     |  +--rw ipv4-multicast-address?      empty
                 |     +--:(ipv6-multicast-address)
                 |     |  +--rw ipv6-multicast-address?      empty
                 |     +--:(mac-address-filter)
                 |        +--rw mac-address-value?           yang:mac-address
                 |        +--rw mac-address-mask?            yang:mac-address
                 +--rw destination-mac-address!
                 |  +--rw filter-match?                      boolean
                 |  +--rw (mac-address)
                 |     +--:(any-multicast-mac-address)
                 |     |  +--rw any-multicast-mac-address?   empty
                 |     +--:(unicast-address)
                 |     |  +--rw unicast-address?             empty
                 |     +--:(broadcast-address)
                 |     |  +--rw broadcast-address?           empty
                 |     +--:(cfm-multicast-address)
                 |     |  +--rw cfm-multicast-address?       empty
                 |     +--:(ipv4-multicast-address)
                 |     |  +--rw ipv4-multicast-address?      empty
                 |     +--:(ipv6-multicast-address)
                 |     |  +--rw ipv6-multicast-address?      empty
                 |     +--:(mac-address-filter)
                 |        +--rw mac-address-value?           yang:mac-address
                 |        +--rw mac-address-mask?            yang:mac-address
                 +--rw vlans!
                 |  +--rw (vlan-match)
                 |     +--:(match-untagged)
                 |     |  +--rw untagged?   empty
                 |     +--:(match-vlan-tagged)
                 |        +--rw tag* [index]
                 |           +--rw index           bbf-classif:tag-index
                 |           +--rw in-pbit-list?   bbf-dot1qt:pbit-list
                 |           +--rw in-dei?         uint8
                 +--rw ethernet-frame-type*       bbf-dot1qt:ether-type-or-acronym
                 +--rw ipv4! {bbf-qos-cpsfilt:match-on-specific-ipv4-header-fields}?
                 |  +--rw ihl?                              uint8
                 |  +--rw flags?                            bits
                 |  +--rw offset?                           uint16
                 |  +--rw identification?                   uint16
                 |  +--rw (destination-network)?
                 |  |  +--:(destination-ipv4-network)
                 |  |     +--rw destination-ipv4-network?   inet:ipv4-prefix
                 |  +--rw (source-network)?
                 |     +--:(source-ipv4-network)
                 |        +--rw source-ipv4-network?        inet:ipv4-prefix
                 +--rw ipv6! {bbf-qos-cpsfilt:match-on-specific-ipv6-header-fields}?
                 |  +--rw (destination-network)?
                 |  |  +--:(destination-ipv6-network)
                 |  |     +--rw destination-ipv6-network?   inet:ipv6-prefix
                 |  +--rw (source-network)?
                 |  |  +--:(source-ipv6-network)
                 |  |     +--rw source-ipv6-network?        inet:ipv6-prefix
                 |  +--rw flow-label?                       inet:ipv6-flow-label
                 +--rw ip-common! {bbf-qos-cpsfilt:match-on-common-ip-header-fields}?
                 |  +--rw dscp-range?   bbf-classif:dscp-range-or-any
                 |  +--rw dscp?         inet:dscp
                 |  +--rw ecn?          uint8
                 |  +--rw length?       uint16
                 |  +--rw ttl?          uint8
                 |  +--rw protocol?     uint8
                 +--rw transport! {bbf-qos-cpsfilt:match-on-common-ip-header-fields}?
                 |  +--rw source-port-range!
                 |  |  +--rw (port-range-or-operator)
                 |  |     +--:(range)
                 |  |     |  +--rw lower-port    inet:port-number
                 |  |     |  +--rw upper-port    inet:port-number
                 |  |     +--:(operator)
                 |  |        +--rw operator?     operator
                 |  |        +--rw port          inet:port-number
                 |  +--rw destination-port-range!
                 |     +--rw (port-range-or-operator)
                 |        +--:(range)
                 |        |  +--rw lower-port    inet:port-number
                 |        |  +--rw upper-port    inet:port-number
                 |        +--:(operator)
                 |           +--rw operator?     operator
                 |           +--rw port          inet:port-number
                 +--rw protocol*                  bbf-classif:protocols
                 +--rw pbit-marking-list* [index]
                 |  +--rw index         bbf-qos-cls:qos-pbit-marking-index
                 |  +--rw pbit-value*   bbf-dot1qt:pbit
                 +--rw dei-marking-list* [index]
                 |  +--rw index        bbf-qos-cls:qos-dei-marking-index
                 |  +--rw dei-value    bbf-dot1qt:dei
                 +--rw flow-color*                bbf-qos-cls:qos-color

  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:filter-method:
    +--:(composite-filter)
    |  +--rw composite-filter-name?     -> /composite-filters/composite-filter/name
    +--:(enhanced-classifier)
       +--rw source-mac-address!
       |  +--rw filter-match?                      boolean
       |  +--rw (mac-address)
       |     +--:(any-multicast-mac-address)
       |     |  +--rw any-multicast-mac-address?   empty
       |     +--:(unicast-address)
       |     |  +--rw unicast-address?             empty
       |     +--:(broadcast-address)
       |     |  +--rw broadcast-address?           empty
       |     +--:(cfm-multicast-address)
       |     |  +--rw cfm-multicast-address?       empty
       |     +--:(ipv4-multicast-address)
       |     |  +--rw ipv4-multicast-address?      empty
       |     +--:(ipv6-multicast-address)
       |     |  +--rw ipv6-multicast-address?      empty
       |     +--:(mac-address-filter)
       |        +--rw mac-address-value?           yang:mac-address
       |        +--rw mac-address-mask?            yang:mac-address
       +--rw destination-mac-address!
       |  +--rw filter-match?                      boolean
       |  +--rw (mac-address)
       |     +--:(any-multicast-mac-address)
       |     |  +--rw any-multicast-mac-address?   empty
       |     +--:(unicast-address)
       |     |  +--rw unicast-address?             empty
       |     +--:(broadcast-address)
       |     |  +--rw broadcast-address?           empty
       |     +--:(cfm-multicast-address)
       |     |  +--rw cfm-multicast-address?       empty
       |     +--:(ipv4-multicast-address)
       |     |  +--rw ipv4-multicast-address?      empty
       |     +--:(ipv6-multicast-address)
       |     |  +--rw ipv6-multicast-address?      empty
       |     +--:(mac-address-filter)
       |        +--rw mac-address-value?           yang:mac-address
       |        +--rw mac-address-mask?            yang:mac-address
       +--rw vlans!
       |  +--rw (vlan-match)
       |     +--:(match-untagged)
       |     |  +--rw untagged?   empty
       |     +--:(match-vlan-tagged)
       |        +--rw tag* [index]
       |           +--rw index           bbf-classif:tag-index
       |           +--rw in-pbit-list?   bbf-dot1qt:pbit-list
       |           +--rw in-dei?         uint8
       +--rw ethernet-frame-type*       bbf-dot1qt:ether-type-or-acronym
       +--rw ipv4! {bbf-qos-cpsfilt:match-on-specific-ipv4-header-fields}?
       |  +--rw ihl?                              uint8
       |  +--rw flags?                            bits
       |  +--rw offset?                           uint16
       |  +--rw identification?                   uint16
       |  +--rw (destination-network)?
       |  |  +--:(destination-ipv4-network)
       |  |     +--rw destination-ipv4-network?   inet:ipv4-prefix
       |  +--rw (source-network)?
       |     +--:(source-ipv4-network)
       |        +--rw source-ipv4-network?        inet:ipv4-prefix
       +--rw ipv6! {bbf-qos-cpsfilt:match-on-specific-ipv6-header-fields}?
       |  +--rw (destination-network)?
       |  |  +--:(destination-ipv6-network)
       |  |     +--rw destination-ipv6-network?   inet:ipv6-prefix
       |  +--rw (source-network)?
       |  |  +--:(source-ipv6-network)
       |  |     +--rw source-ipv6-network?        inet:ipv6-prefix
       |  +--rw flow-label?                       inet:ipv6-flow-label
       +--rw ip-common! {bbf-qos-cpsfilt:match-on-common-ip-header-fields}?
       |  +--rw dscp-range?   bbf-classif:dscp-range-or-any
       |  +--rw dscp?         inet:dscp
       |  +--rw ecn?          uint8
       |  +--rw length?       uint16
       |  +--rw ttl?          uint8
       |  +--rw protocol?     uint8
       +--rw transport! {bbf-qos-cpsfilt:match-on-common-ip-header-fields}?
       |  +--rw source-port-range!
       |  |  +--rw (port-range-or-operator)
       |  |     +--:(range)
       |  |     |  +--rw lower-port    inet:port-number
       |  |     |  +--rw upper-port    inet:port-number
       |  |     +--:(operator)
       |  |        +--rw operator?     operator
       |  |        +--rw port          inet:port-number
       |  +--rw destination-port-range!
       |     +--rw (port-range-or-operator)
       |        +--:(range)
       |        |  +--rw lower-port    inet:port-number
       |        |  +--rw upper-port    inet:port-number
       |        +--:(operator)
       |           +--rw operator?     operator
       |           +--rw port          inet:port-number
       +--rw protocol*                  bbf-classif:protocols
       +--rw pbit-marking-list* [index]
       |  +--rw index         bbf-qos-cls:qos-pbit-marking-index
       |  +--rw pbit-value*   bbf-dot1qt:pbit
       +--rw dei-marking-list* [index]
       |  +--rw index        bbf-qos-cls:qos-dei-marking-index
       |  +--rw dei-value    bbf-dot1qt:dei
       +--rw flow-color*                bbf-qos-cls:qos-color
```

- Module Dependencies: `bbf-yang-types` `bbf-dot1q-types` `bbf-qos-classifiers` `bbf-frame-classification` `ietf-packet-fields`

## 8.42. bbf-qos-enhanced-scheduling

```text
module: bbf-qos-enhanced-scheduling

  augment /if:interfaces/if:interface/bbf-qos-tm:tm-root/bbf-qos-tm:children-type:
    +--:(scheduler-node)
       +--rw scheduler-node* [name]
       |  +--rw name                             bbf-yang:string-ascii64
       |  +--rw description?                     bbf-yang:string-ascii64
       |  +--rw scheduling-level                 uint8
       |  +--rw shaper-name?                     -> /bbf-qos-tm:tm-profiles/bbf-qos-shap:shaper-profile/name
       |  +--rw (children-type)?
       |     +--:(scheduler-node)
       |     |  +--rw child-scheduler-nodes* [name]
       |     |     +--rw name                     -> ../../../scheduler-node/name
       |     |     +--rw (scheduling-type)?
       |     |        +--:(inline)
       |     |        |  +--rw priority?          scheduling-priority
       |     |        |  +--rw weight?            scheduling-weight
       |     |        |  +--rw extended-weight?   extended-scheduling-weight {bbf-qos-tm:extended-scheduling-weight}?
       |     |        |  +--rw traffic-type?      identityref {child-traffic-type}?
       |     |        +--:(dual-rate-scheduling) {dual-rate-scheduling-from-scheduler}?
       |     |           +--rw cir-traffic
       |     |           |  +--rw priority?          extended-scheduling-priority
       |     |           |  +--rw extended-weight?   extended-scheduling-weight
       |     |           +--rw eir-traffic
       |     |              +--rw priority?          extended-scheduling-priority
       |     |              +--rw extended-weight?   extended-scheduling-weight
       |     +--:(queue)
       |     |  +--rw contains-queues?           boolean
       |     |  +--rw queue* [local-queue-id]
       |     |  |  +--rw local-queue-id                queue-id
       |     |  |  +--rw bac-name?                     -> /bbf-qos-tm:tm-profiles/bac-entry/name
       |     |  |  +--rw (queue-scheduling-cfg-type)?
       |     |  |  |  +--:(inline)
       |     |  |  |  |  +--rw priority?               scheduling-priority
       |     |  |  |  |  +--rw weight?                 scheduling-weight
       |     |  |  |  |  +--rw extended-weight?        extended-scheduling-weight {bbf-qos-tm:extended-scheduling-weight}?
       |     |  |  |  +--:(dual-rate-scheduling) {bbf-qos-tm:dual-rate-scheduling-from-queue}?
       |     |  |  |     +--rw dual-rate-scheduling
       |     |  |  |        +--rw cir-traffic
       |     |  |  |        |  +--rw priority?          extended-scheduling-priority
       |     |  |  |        |  +--rw extended-weight?   extended-scheduling-weight
       |     |  |  |        +--rw eir-traffic
       |     |  |  |           +--rw priority?          extended-scheduling-priority
       |     |  |  |           +--rw extended-weight?   extended-scheduling-weight
       |     |  |  +--rw pre-emption?                  boolean {bbf-qos-tm:pre-emption}?
       |     |  |  +--rw shaper-name?                  -> /bbf-qos-tm:tm-profiles/bbf-qos-shap:shaper-profile/name
       |     |  +--rw queue-statistics-enable?   boolean {bbf-qos-tm:queue-statistics}?
       |     +--:(child-scheduler-queues) {child-scheduler-queue-references}?
       |        +--rw child-scheduler-queues
       |           +--rw child-scheduler* [name]
       |              +--rw name      -> ../../../../scheduler-node/name
       |              +--rw queues
       |                 +--rw queue* [queue-id]
       |                    +--rw queue-id                 -> ../../../../../../scheduler-node[name=current()/../../../name]/queue/local-queue-id
       |                    +--rw (scheduling-type)?
       |                       +--:(inline)
       |                          +--rw priority?          bbf-qos-tm:scheduling-priority
       |                          +--rw extended-weight?   bbf-qos-tm:extended-scheduling-weight
       |                          +--rw traffic-type?      identityref {child-traffic-type}?
       +--rw child-scheduler-nodes* [name]
          +--rw name                     -> ../../scheduler-node/name
          +--rw (scheduling-type)?
             +--:(inline)
             |  +--rw priority?          scheduling-priority
             |  +--rw weight?            scheduling-weight
             |  +--rw extended-weight?   extended-scheduling-weight {bbf-qos-tm:extended-scheduling-weight}?
             +--:(dual-rate-scheduling) {bbf-qos-sched:dual-rate-scheduling-from-scheduler}?
                +--rw cir-traffic
                |  +--rw priority?          extended-scheduling-priority
                |  +--rw extended-weight?   extended-scheduling-weight
                +--rw eir-traffic
                   +--rw priority?          extended-scheduling-priority
                   +--rw extended-weight?   extended-scheduling-weight
  augment /if:interfaces/if:interface:
    +--rw egress-tm-objects!
       +--rw (select-tm-objects-method)?
          +--:(scheduler)
             +--rw root-if-name?          -> /if:interfaces/interface/name
             +--rw scheduler-node-name?   -> /if:interfaces/interface[if:name = current()/../root-if-name]/bbf-qos-tm:tm-root/bbf-qos-sched:scheduler-node/name
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-qos-traffic-mngt` `bbf-qos-shaping`

## 8.43. bbf-qos-enhanced-scheduling-state

```text
module: bbf-qos-enhanced-scheduling-state

  augment /if:interfaces-state/if:interface/bbf-qos-tm-s:tm-root/bbf-qos-tm-s:children-type:
    +--:(scheduler-node)
       +--ro scheduler-node* [name]
          +--ro name            bbf-yang:string-ascii64
          +--ro (children-type)?
             +--:(queue)
                +--ro queues
                   +---x reset-statistics
                   +--ro queue* [local-queue-id]
                      +--ro local-queue-id    bbf-qos-tm:queue-id
                      +--ro statistics {bbf-qos-tm:queue-statistics}?
                      |  +--ro enqueued-frames?   yang:gauge64
                      |  +--ro enqueued-octets?   yang:gauge64
                      |  +--ro total
                      |  |  +--ro out-frames?                  yang:counter64
                      |  |  +--ro out-octets?                  yang:counter64
                      |  |  +--ro congestion-discard-frames?   yang:counter64
                      |  |  +--ro congestion-discard-octets?   yang:counter64
                      |  +--ro green
                      |  |  +--ro out-frames?                  yang:counter64
                      |  |  +--ro out-octets?                  yang:counter64
                      |  |  +--ro congestion-discard-frames?   yang:counter64
                      |  |  +--ro congestion-discard-octets?   yang:counter64
                      |  +--ro yellow
                      |  |  +--ro out-frames?                  yang:counter64
                      |  |  +--ro out-octets?                  yang:counter64
                      |  |  +--ro congestion-discard-frames?   yang:counter64
                      |  |  +--ro congestion-discard-octets?   yang:counter64
                      |  +--ro red
                      |     +--ro out-frames?                  yang:counter64
                      |     +--ro out-octets?                  yang:counter64
                      |     +--ro congestion-discard-frames?   yang:counter64
                      |     +--ro congestion-discard-octets?   yang:counter64
                      +---x reset-counters
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-qos-traffic-mngt` `bbf-qos-traffic-mngt-state`

## 8.44. bbf-qos-filters

```text
module: bbf-qos-filters
  +--rw filters
     +--rw filter* [name]
        +--rw name                             bbf-yang:string-ascii64
        +--rw description?                     bbf-yang:string-ascii64-or-empty
        +--rw filter-match?                    boolean
        +--rw (filter-field)?
           +--:(source-mac-address)
           |  +--rw source-mac-address
           |     +--rw (mac-address)?
           |        +--:(any-multicast-mac-address)
           |        |  +--rw any-multicast-mac-address?   empty
           |        +--:(unicast-address)
           |        |  +--rw unicast-address?             empty
           |        +--:(broadcast-address)
           |        |  +--rw broadcast-address?           empty
           |        +--:(cfm-multicast-address)
           |        |  +--rw cfm-multicast-address?       empty
           |        +--:(ipv4-multicast-address)
           |        |  +--rw ipv4-multicast-address?      empty
           |        +--:(ipv6-multicast-address)
           |        |  +--rw ipv6-multicast-address?      empty
           |        +--:(mac-address-filter)
           |           +--rw mac-address-value?           yang:mac-address
           |           +--rw mac-address-mask?            yang:mac-address
           +--:(destination-mac-address)
              +--rw destination-mac-address
                 +--rw (mac-address)?
                    +--:(any-multicast-mac-address)
                    |  +--rw any-multicast-mac-address?   empty
                    +--:(unicast-address)
                    |  +--rw unicast-address?             empty
                    +--:(broadcast-address)
                    |  +--rw broadcast-address?           empty
                    +--:(cfm-multicast-address)
                    |  +--rw cfm-multicast-address?       empty
                    +--:(ipv4-multicast-address)
                    |  +--rw ipv4-multicast-address?      empty
                    +--:(ipv6-multicast-address)
                    |  +--rw ipv6-multicast-address?      empty
                    +--:(mac-address-filter)
                       +--rw mac-address-value?           yang:mac-address
                       +--rw mac-address-mask?            yang:mac-address

  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:filter-method:
    +--:(by-reference)
       +--rw filter* [name]
          +--rw name           bbf-yang:string-ascii64
          +--rw filter-name?   -> /filters/filter/name
```

- Module Dependencies: `bbf-yang-types` `bbf-qos-classifiers` `bbf-frame-classification`

## 8.45. bbf-qos-policer-envelope-profiles

```text
module: bbf-qos-policer-envelope-profiles
  +--rw envelope-profiles
     +--rw envelope-profile* [name]
        +--rw name             bbf-yang:string-ascii64
        +--rw scope?           identityref
        +--rw coupling-flag?   bbf-qos-plc-tp:coupling-flag
        +--rw flows
           +--rw flow* [rank]
              +--rw rank             flow-rank
              +--rw cir?             bbf-qos-plc-tp:information-rate
              +--rw cir-max?         bbf-qos-plc-tp:information-rate
              +--rw cbs?             bbf-qos-plc-tp:burst-size
              +--rw eir?             bbf-qos-plc-tp:information-rate
              +--rw eir-max?         bbf-qos-plc-tp:information-rate
              +--rw ebs?             bbf-qos-plc-tp:burst-size
              +--rw coupling-flag?   bbf-qos-plc-tp:coupling-flag
              +--rw color-mode?      bbf-qos-plc-tp:color-mode

  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:classifier-action-entry-cfg/bbf-qos-cls:action-cfg-params:
    +--:(envelope-policing)
       +--rw policer-envelope-flow
          +--rw envelope?   -> /envelope-profiles/envelope-profile/name
          +--rw flow?       -> /envelope-profiles/envelope-profile[bbf-qos-plc-ep:name=current()/../envelope]/flows/flow/rank
```

- Module Dependencies: `bbf-qos-classifiers` `bbf-yang-types` `bbf-qos-policing-types`

## 8.46. bbf-qos-policies

```text
module: bbf-qos-policies
  +--rw policies
  |  +--rw policy* [name]
  |     +--rw name           bbf-yang:string-ascii64
  |     +--rw description?   bbf-yang:string-ascii64-or-empty
  |     +--rw classifiers* [name]
  |        +--rw name    -> /bbf-qos-cls:classifiers/classifier-entry/name
  +--rw qos-policy-profiles
     +--rw policy-profile* [name]
        +--rw name           bbf-yang:string-ascii64
        +--rw policy-list* [name]
           +--rw name    -> /policies/policy/name

  augment /if:interfaces/if:interface:
    +--rw ingress-qos-policy-profile?   qos-policy-profile-ref
    +--rw egress-qos-policy-profile?    qos-policy-profile-ref
  augment /if:interfaces/if:interface:
    +--rw qos-policies! {interface-policy-management}?
```

- Module Dependencies: `ietf-interfaces` `iana-if-type` `bbf-if-type` `bbf-yang-types` `bbf-qos-classifiers`

## 8.47. bbf-qos-policies-state

```text
module: bbf-qos-policies-state

  augment /if:interfaces-state/if:interface:
    +--ro qos-policies!
       +--ro ingress!
       |  +--ro policy* [name]
       |     +--ro name          bbf-yang:string-ascii64
       |     +--ro classifier* [name]
       |        +--ro name    bbf-yang:string-ascii64
       +--ro egress!
          +--ro policy* [name]
             +--ro name          bbf-yang:string-ascii64
             +--ro classifier* [name]
                +--ro name    bbf-yang:string-ascii64
```

- Module Dependencies: `ietf-interfaces` `bbf-yang-types`

## 8.48. bbf-qos-policies-sub-interface-rewrite

```text
module: bbf-qos-policies-sub-interface-rewrite

  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:ingress-rule/bbf-subif:rule/bbf-subif:ingress-rewrite:
    +--rw copy-from-tags-to-marking-list* [tag-index]
       +--rw tag-index             bbf-classif:tag-index
       +--rw pbit-marking-index?   union
       +--rw dei-marking-index?    union
```

- Module Dependencies: `ietf-interfaces` `bbf-qos-classifiers` `bbf-frame-classification` `bbf-sub-interfaces`

## 8.49. bbf-qos-policies-sub-interfaces

```text
module: bbf-qos-policies-sub-interfaces

  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:ingress-rule/bbf-subif:rule/bbf-subif:ingress-rewrite/bbf-subif-tag:push-tag/bbf-subif-tag:dot1q-tag/bbf-subif-tag:pbit:
    +--:(generate-pbit-via-profile-or-0)
       +--rw pbit-marking-index?   bbf-qos-cls:qos-pbit-marking-index
  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:ingress-rule/bbf-subif:rule/bbf-subif:ingress-rewrite/bbf-subif-tag:push-tag/bbf-subif-tag:dot1q-tag/bbf-subif-tag:dei:
    +--:(generate-dei-via-profile-or-0)
       +--rw dei-marking-index?   bbf-qos-cls:qos-dei-marking-index
  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:egress-rewrite/bbf-subif-tag:push-tag/bbf-subif-tag:dot1q-tag/bbf-subif-tag:pbit:
    +--:(generate-pbit-via-profile-or-0)
       +--rw pbit-marking-index?   bbf-qos-cls:qos-pbit-marking-index
  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:egress-rewrite/bbf-subif-tag:push-tag/bbf-subif-tag:dot1q-tag/bbf-subif-tag:dei:
    +--:(generate-dei-via-profile-or-0)
       +--rw dei-marking-index?   bbf-qos-cls:qos-dei-marking-index
```

- Module Dependencies: `ietf-interfaces` `bbf-qos-classifiers` `bbf-sub-interfaces` `bbf-sub-interface-tagging`

## 8.50. bbf-qos-policing

```text
module: bbf-qos-policing
  +--rw policing-profiles
     +--rw policing-profile* [name]
        +--rw name                                                 bbf-yang:string-ascii64
        +--rw scope?                                               bbf-qos-cls:action-scope
        +--rw (policing-type)
        |  +--:(single-rate-two-color-marker-type) {single-rate-two-color-marker}?
        |  |  +--rw single-rate-two-color-marker
        |  |     +--rw cir?   bbf-qos-plc-tp:information-rate
        |  |     +--rw cbs?   bbf-qos-plc-tp:burst-size
        |  +--:(single-rate-three-color-marker-type) {single-rate-three-color-marker}?
        |  |  +--rw single-rate-three-color-marker
        |  |     +--rw cir?   bbf-qos-plc-tp:information-rate
        |  |     +--rw cbs?   bbf-qos-plc-tp:burst-size
        |  |     +--rw ebs?   bbf-qos-plc-tp:burst-size
        |  +--:(two-rate-three-color-marker-type) {two-rate-three-color-marker}?
        |  |  +--rw two-rate-three-color-marker
        |  |     +--rw cir?   bbf-qos-plc-tp:information-rate
        |  |     +--rw cbs?   bbf-qos-plc-tp:burst-size
        |  |     +--rw pir?   bbf-qos-plc-tp:information-rate
        |  |     +--rw pbs?   bbf-qos-plc-tp:burst-size
        |  +--:(two-rate-three-color-marker-mef-type) {two-rate-three-color-marker-mef}?
        |  |  +--rw two-rate-three-color-marker-mef
        |  |     +--rw cir?           bbf-qos-plc-tp:information-rate
        |  |     +--rw cbs?           bbf-qos-plc-tp:burst-size
        |  |     +--rw eir?           bbf-qos-plc-tp:information-rate
        |  |     +--rw ebs?           bbf-qos-plc-tp:burst-size
        |  |     +--rw couple-flag?   bbf-qos-plc-tp:coupling-flag
        |  |     +--rw color-mode?    bbf-qos-plc-tp:color-mode
        |  +--:(two-rate-three-color-marker-color-promotion-type) {two-rate-three-color-marker-color-promotion}?
        |     +--rw two-rate-three-color-marker-color-promotion
        |        +--rw cir?   bbf-qos-plc-tp:information-rate
        |        +--rw cbs?   bbf-qos-plc-tp:burst-size
        |        +--rw eir?   bbf-qos-plc-tp:information-rate
        |        +--rw ebs?   bbf-qos-plc-tp:burst-size
        +--rw hierarchical-policing
           +--rw policing-profile?   -> /policing-profiles/policing-profile/name

  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:filter-method/bbf-qos-cls:inline/bbf-qos-cls:match-criteria:
    x--rw pbit-marking-list* [index]
    |  x--rw index         bbf-qos-cls:qos-pbit-marking-index
    |  x--rw pbit-value?   bbf-dot1qt:pbit
    x--rw dei-marking-list* [index]
    |  x--rw index        bbf-qos-cls:qos-dei-marking-index
    |  x--rw dei-value?   bbf-dot1qt:dei
    x--rw flow-color*          qos-color
  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:classifier-action-entry-cfg/bbf-qos-cls:action-cfg-params:
    +--:(flow-color)
    |  +--rw flow-color?   qos-color
    +--:(bac-color)
    |  +--rw bac-color?    qos-color
    +--:(discard)
    |  +--rw discard?      empty
    +--:(policing)
       +--rw policing
          +--rw policing-profile?   -> /policing-profiles/policing-profile/name
  augment /if:interfaces/if:interface/bbf-qos-pol:qos-policies:
    +--rw policing! {interface-policing-management}?
       +--rw statistics {policing-statistics}?
          +--rw enabled?   boolean
          +---x reset
```

- Module Dependencies: `bbf-yang-types` `bbf-qos-classifiers` `bbf-qos-policies` `bbf-qos-policing-types` `bbf-dot1q-types` `ietf-interfaces` `ietf-yang-types`

## 8.51. bbf-qos-policing-state

```text
module: bbf-qos-policing-state

  augment /if:interfaces-state/if:interface/bbf-qos-pol-s:qos-policies/bbf-qos-pol-s:ingress/bbf-qos-pol-s:policy/bbf-qos-pol-s:classifier:
    +--ro policing!
       +--ro statistics {bbf-qos-plc:policing-statistics}?
          +--ro out-green
          |  +--ro total-octets?       yang:counter64
          |  +--ro total-frames?       yang:counter64
          |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          +--ro out-yellow
          |  +--ro total-octets?       yang:counter64
          |  +--ro total-frames?       yang:counter64
          |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          +--ro out-red
             +--ro total-octets?       yang:counter64
             +--ro total-frames?       yang:counter64
             +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-red-octets?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-red-frames?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
  augment /if:interfaces-state/if:interface/bbf-qos-pol-s:qos-policies/bbf-qos-pol-s:egress/bbf-qos-pol-s:policy/bbf-qos-pol-s:classifier:
    +--ro policing!
       +--ro statistics {bbf-qos-plc:policing-statistics}?
          +--ro out-green
          |  +--ro total-octets?       yang:counter64
          |  +--ro total-frames?       yang:counter64
          |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          +--ro out-yellow
          |  +--ro total-octets?       yang:counter64
          |  +--ro total-frames?       yang:counter64
          |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
          +--ro out-red
             +--ro total-octets?       yang:counter64
             +--ro total-frames?       yang:counter64
             +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-red-octets?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro in-red-frames?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
  augment /if:interfaces-state/if:interface/bbf-qos-pol-s:qos-policies/bbf-qos-pol-s:ingress:
    +--ro policing!
       +--ro policer* [name]
          +--ro name          bbf-yang:string-ascii64
          +--ro statistics {bbf-qos-plc:policing-statistics}?
             +--ro out-green
             |  +--ro total-octets?       yang:counter64
             |  +--ro total-frames?       yang:counter64
             |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro out-yellow
             |  +--ro total-octets?       yang:counter64
             |  +--ro total-frames?       yang:counter64
             |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro out-red
                +--ro total-octets?       yang:counter64
                +--ro total-frames?       yang:counter64
                +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-red-octets?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-red-frames?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
  augment /if:interfaces-state/if:interface/bbf-qos-pol-s:qos-policies/bbf-qos-pol-s:egress:
    +--ro policing!
       +--ro policer* [name]
          +--ro name          bbf-yang:string-ascii64
          +--ro statistics {bbf-qos-plc:policing-statistics}?
             +--ro out-green
             |  +--ro total-octets?       yang:counter64
             |  +--ro total-frames?       yang:counter64
             |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro out-yellow
             |  +--ro total-octets?       yang:counter64
             |  +--ro total-frames?       yang:counter64
             |  +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             |  +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
             +--ro out-red
                +--ro total-octets?       yang:counter64
                +--ro total-frames?       yang:counter64
                +--ro in-green-octets?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-green-frames?    yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-yellow-octets?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-yellow-frames?   yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-red-octets?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
                +--ro in-red-frames?      yang:counter64 {bbf-qos-plc:counts-per-ingress-and-egress-color}?
```

- Module Dependencies: `bbf-qos-policies-state` `bbf-qos-policing` `bbf-yang-types` `ietf-interfaces`

## 8.52. bbf-qos-rate-control

```text
module: bbf-qos-rate-control

  augment /bbf-qos-cls:classifiers/bbf-qos-cls:classifier-entry/bbf-qos-cls:classifier-action-entry-cfg/bbf-qos-cls:action-cfg-params:
    +--:(rate-limit-frames)
       +--rw rate-limit-frames
          +--rw rate?         uint32
          +--rw burst-size?   uint32
```

- Module Dependencies: `bbf-qos-classifiers`

## 8.53. bbf-qos-shaping

```text
module: bbf-qos-shaping

  augment /bbf-qos-tm:tm-profiles:
    +--rw shaper-profile* [name]
       +--rw name                         bbf-yang:string-ascii64
       +--rw (shaper-type)
          +--:(single-token-bucket)
          |  +--rw single-token-bucket
          |     +--rw pir?   uint32
          |     +--rw pbs?   uint32
          +--:(two-token-bucket) {two-token-bucket-shaper}?
             +--rw two-token-bucket
                +--rw pir    uint32
                +--rw cir    uint32
  augment /if:interfaces/if:interface/bbf-qos-tm:tm-root:
    +--rw shaper-name?   -> /bbf-qos-tm:tm-profiles/bbf-qos-shap:shaper-profile/name
  augment /if:interfaces/if:interface/bbf-qos-tm:tm-root/bbf-qos-tm:children-type/bbf-qos-tm:queues/bbf-qos-tm:queue:
    +--rw shaper-name?   -> /bbf-qos-tm:tm-profiles/bbf-qos-shap:shaper-profile/name
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-qos-traffic-mngt`

## 8.54. bbf-qos-traffic-mngt

```text
module: bbf-qos-traffic-mngt
  +--rw tm-profiles
     +--rw tc-id-2-queue-id-mapping-profile* [name] {tc-id-2-queue-id-mapping-config}?
     |  +--rw name             bbf-yang:string-ascii64
     |  +--rw mapping-entry* [traffic-class-id]
     |     +--rw traffic-class-id    bbf-qos-t:traffic-class-id
     |     +--rw local-queue-id      queue-id
     +--rw bac-entry* [name]
        +--rw name               bbf-yang:string-ascii64
        +--rw max-queue-size?    uint32
        +--rw (bac-type)
           +--:(taildrop)
           |  +--rw taildrop
           |     +--rw max-threshold?   bbf-yang:percent
           +--:(red)
           |  +--rw red
           |     +--rw min-threshold?     bbf-yang:percent
           |     +--rw max-threshold?     bbf-yang:percent
           |     +--rw max-probability?   bbf-yang:percent
           +--:(wtaildrop)
           |  +--rw wtaildrop
           |     +--rw color
           |        +--rw green
           |        |  +--rw max-threshold?   bbf-yang:percent
           |        +--rw yellow
           |        |  +--rw max-threshold?   bbf-yang:percent
           |        +--rw red
           |           +--rw max-threshold?   bbf-yang:percent
           +--:(wred)
              +--rw wred
                 +--rw color
                    +--rw green
                    |  +--rw min-threshold?     bbf-yang:percent
                    |  +--rw max-threshold?     bbf-yang:percent
                    |  +--rw max-probability?   bbf-yang:percent
                    +--rw yellow
                    |  +--rw min-threshold?     bbf-yang:percent
                    |  +--rw max-threshold?     bbf-yang:percent
                    |  +--rw max-probability?   bbf-yang:percent
                    +--rw red
                       +--rw min-threshold?     bbf-yang:percent
                       +--rw max-threshold?     bbf-yang:percent
                       +--rw max-probability?   bbf-yang:percent

  augment /if:interfaces/if:interface:
    +--rw tm-root!
       +--rw (children-type)?
       |  +--:(queues)
       |     +--rw queue* [local-queue-id]
       |     |  +--rw local-queue-id                queue-id
       |     |  +--rw bac-name?                     -> /tm-profiles/bac-entry/name
       |     |  +--rw (queue-scheduling-cfg-type)?
       |     |  |  +--:(inline)
       |     |  |  |  +--rw priority?               scheduling-priority
       |     |  |  |  +--rw weight?                 scheduling-weight
       |     |  |  |  +--rw extended-weight?        extended-scheduling-weight {bbf-qos-tm:extended-scheduling-weight}?
       |     |  |  +--:(dual-rate-scheduling) {bbf-qos-tm:dual-rate-scheduling-from-queue}?
       |     |  |     +--rw dual-rate-scheduling
       |     |  |        +--rw cir-traffic
       |     |  |        |  +--rw priority?          extended-scheduling-priority
       |     |  |        |  +--rw extended-weight?   extended-scheduling-weight
       |     |  |        +--rw eir-traffic
       |     |  |           +--rw priority?          extended-scheduling-priority
       |     |  |           +--rw extended-weight?   extended-scheduling-weight
       |     |  +--rw pre-emption?                  boolean {bbf-qos-tm:pre-emption}?
       |     +--rw queue-statistics-enable?           boolean {bbf-qos-tm:queue-statistics}?
       +--rw tc-id-2-queue-id-mapping-profile-name?   -> /tm-profiles/tc-id-2-queue-id-mapping-profile/name {bbf-qos-tm:tc-id-2-queue-id-mapping-config}?
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-yang-types` `bbf-qos-types`

## 8.55. bbf-qos-traffic-mngt-state

```text
module: bbf-qos-traffic-mngt-state

  augment /if:interfaces-state/if:interface:
    +--ro tm-root!
       +--ro (children-type)?
          +--:(queues)
             +--ro queues
                +---x reset-statistics
                +--ro queue* [local-queue-id]
                   +--ro local-queue-id    bbf-qos-tm:queue-id
                   +--ro statistics {bbf-qos-tm:queue-statistics}?
                   |  +--ro enqueued-frames?   yang:gauge64
                   |  +--ro enqueued-octets?   yang:gauge64
                   |  +--ro total
                   |  |  +--ro out-frames?                  yang:counter64
                   |  |  +--ro out-octets?                  yang:counter64
                   |  |  +--ro congestion-discard-frames?   yang:counter64
                   |  |  +--ro congestion-discard-octets?   yang:counter64
                   |  +--ro green
                   |  |  +--ro out-frames?                  yang:counter64
                   |  |  +--ro out-octets?                  yang:counter64
                   |  |  +--ro congestion-discard-frames?   yang:counter64
                   |  |  +--ro congestion-discard-octets?   yang:counter64
                   |  +--ro yellow
                   |  |  +--ro out-frames?                  yang:counter64
                   |  |  +--ro out-octets?                  yang:counter64
                   |  |  +--ro congestion-discard-frames?   yang:counter64
                   |  |  +--ro congestion-discard-octets?   yang:counter64
                   |  +--ro red
                   |     +--ro out-frames?                  yang:counter64
                   |     +--ro out-octets?                  yang:counter64
                   |     +--ro congestion-discard-frames?   yang:counter64
                   |     +--ro congestion-discard-octets?   yang:counter64
                   +---x reset-counters
```

- Module Dependencies: `ietf-interfaces` `bbf-qos-traffic-mngt`

## 8.56. bbf-software-management

```text
module: bbf-software-management

  augment /hw:hardware/hw:component:
    +--ro software!
       +--ro software* [name]
          +--ro name          bbf-yang:string-ascii64
          +--ro capability*   identityref
          +--ro revisions
             +--ro maximum-number-of-revisions?   uint16
             +--ro download
             |  +---x download {software-management-actions}?
             |     +---w input
             |     |  +---w source
             |     |  |  +---w (source)
             |     |  |     +--:(url)
             |     |  |        +---w url?   inet:uri
             |     |  +---w target
             |     |     +---w (target)
             |     |     |  +--:(selected-by-user) {download-target-selection-by-user}?
             |     |     |  |  +---w selected-by-user
             |     |     |  |     +---w operation    enumeration
             |     |     |  |     +---w replace
             |     |     |  |        +---w (selection-criteria)
             |     |     |  |           +--:(id)
             |     |     |  |           |  +---w id       -> ../../../../../../../revisions/revision/id
             |     |     |  |           +--:(alias)
             |     |     |  |              +---w alias    -> ../../../../../../../revisions/revision/alias
             |     |     |  +--:(selected-by-system) {download-target-selection-by-system}?
             |     |     |     +---w selected-by-system
             |     |     |        +---w operation    enumeration
             |     |     |        +---w replace
             |     |     |           +---w not-available-only?   boolean
             |     |     |           +---w oldest-only?          boolean
             |     |     +---w alias?                      bbf-yang:string-ascii64
             |     +--ro output
             |        +--ro target!
             |           +--ro id          uint8
             |           +--ro replaces!
             |              +--ro alias?          bbf-yang:string-ascii64
             |              +--ro version?        string
             |              +--ro product-code?   string
             |              +--ro hash?           string
             |              +--ro source
             |                 +--ro (source)?
             |                    +--:(url)
             |                       +--ro url?   inet:uri
             +---n revision-deleted
             |  +-- id              uint8
             |  +-- alias?          bbf-yang:string-ascii64
             |  +-- version?        string
             |  +-- product-code?   string
             |  +-- hash?           string
             |  +-- source
             |     +-- (source)?
             |        +--:(url)
             |           +-- url?   inet:uri
             +--ro revision* [id]
                +--ro id                          uint8
                +--ro alias?                      bbf-yang:string-ascii64
                +--ro state                       enumeration
                +--ro last-changed                yang:date-and-time
                +--ro download-completed?         yang:date-and-time
                +--ro version?                    string
                +--ro product-code?               string
                +--ro hash?                       string
                +--ro source
                |  +--ro (source)?
                |     +--:(url)
                |        +--ro url?   inet:uri
                +--ro is-valid                    boolean
                +--ro is-active                   boolean
                +--ro is-committed                boolean
                +--ro abort-download
                |  +---x abort-download {software-management-actions}?
                |  +---n download-revision-aborted
                |  +---n abort-failed
                |     +-- error
                |        +--ro tag        identityref
                |        +--ro message?   string
                +--ro activate
                |  +--ro last-activated?       yang:date-and-time
                |  +---x activate {software-management-actions}?
                |  +---n revision-activated
                |  +---n activate-failed
                |     +-- error
                |        +--ro tag        identityref
                |        +--ro message?   string
                +--ro commit
                |  +--ro last-committed?       yang:date-and-time
                |  +---x commit {software-management-actions}?
                |  +---n revision-committed
                |  +---n commit-failed
                |     +-- error
                |        +--ro tag        identityref
                |        +--ro message?   string
                +--ro delete
                |  +---x delete {software-management-actions}?
                |  +---n delete-failed
                |     +-- error
                |        +--ro tag        identityref
                |        +--ro message?   string
                +---n revision-downloaded
                +---n download-revision-failed
                   +-- error
                      +--ro tag        identityref
                      +--ro message?   string
```

- Module Dependencies: `ietf-inet-types` `ietf-hardware` `ietf-yang-types` `bbf-yang-types`

## 8.57. bbf-software-management-voice

```text
module: bbf-software-management-voice

  augment /hw:hardware/hw:component/bbf-swm:software/bbf-swm:software/bbf-swm:revisions/bbf-swm:revision/bbf-swm:activate/bbf-swm:activate/bbf-swm:input:
    +---w activate-conditions?   activate-conditions
  augment /hw:hardware/hw:component/bbf-swm:software/bbf-swm:software/bbf-swm:revisions/bbf-swm:revision/bbf-swm:activate/bbf-swm:revision-activated:
    +--ro activate-conditions    activate-conditions
  augment /hw:hardware/hw:component/bbf-swm:software/bbf-swm:software/bbf-swm:revisions/bbf-swm:revision/bbf-swm:activate/bbf-swm:activate-failed:
    +--ro activate-conditions    activate-conditions
```

- Module Dependencies: `bbf-software-management` `ietf-hardware`

## 8.58. bbf-sub-interfaces

```text
module: bbf-sub-interfaces

  augment /if:interfaces/if:interface:
    +--rw subif-lower-layer
    |  +--rw interface    if:interface-ref
    +--rw (frame-processing)?
       +--:(inline-frame-processing)
          +--rw inline-frame-processing
             +--rw ingress-rule
             |  +--rw rule* [name]
             |     +--rw name               bbf-yang:string-ascii64
             |     +--rw priority           uint16
             |     +--rw flexible-match
             |     +--rw ingress-rewrite {tag-rewrites}?
             +--rw egress-rewrite {tag-rewrites}?
```

- Module Dependencies: `ietf-interfaces` `bbf-yang-types` `bbf-if-type`

## 8.59. bbf-sub-interface-tagging

```text
module: bbf-sub-interface-tagging

  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:ingress-rule/bbf-subif:rule/bbf-subif:flexible-match:
    +--rw match-criteria
    |  +--rw (frame-filter)?
    |  |  +--:(any-frame)
    |  |  |  +--rw any-frame?                                empty
    |  |  +--:(destination-mac-address)
    |  |  |  +--rw (mac-address)?
    |  |  |     +--:(any-multicast-mac-address)
    |  |  |     |  +--rw any-multicast-mac-address?          empty
    |  |  |     +--:(unicast-address)
    |  |  |     |  +--rw unicast-address?                    empty
    |  |  |     +--:(broadcast-address)
    |  |  |     |  +--rw broadcast-address?                  empty
    |  |  |     +--:(cfm-multicast-address)
    |  |  |     |  +--rw cfm-multicast-address?              empty
    |  |  |     +--:(ipv4-multicast-address)
    |  |  |     |  +--rw ipv4-multicast-address?             empty
    |  |  |     +--:(ipv6-multicast-address)
    |  |  |     |  +--rw ipv6-multicast-address?             empty
    |  |  |     +--:(mac-address-filter)
    |  |  |        +--rw mac-address-value?                  yang:mac-address
    |  |  |        +--rw mac-address-mask?                   yang:mac-address
    |  |  +--:(destination-ipv4-address)
    |  |  |  +--rw (ipv4-address)?
    |  |  |     +--:(any-multicast-ipv4-address)
    |  |  |     |  +--rw any-multicast-ipv4-address?         empty
    |  |  |     +--:(all-hosts-multicast-address)
    |  |  |     |  +--rw all-hosts-multicast-address?        empty
    |  |  |     +--:(rip-multicast-address)
    |  |  |     |  +--rw rip-multicast-address?              empty
    |  |  |     +--:(ntp-multicast-address)
    |  |  |     |  +--rw ntp-multicast-address?              empty
    |  |  |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
    |  |  |        +--rw ipv4-prefix?                        inet:ipv4-prefix
    |  |  +--:(destination-ipv6-address)
    |  |     +--rw (ipv6-address)?
    |  |        +--:(any-multicast-ipv6-address)
    |  |        |  +--rw any-multicast-ipv6-address?         empty
    |  |        +--:(all-nodes-multicast-ipv6-address)
    |  |        |  +--rw all-nodes-multicast-ipv6-address?   empty
    |  |        +--:(rip-multicast-ipv6-address)
    |  |        |  +--rw rip-multicast-ipv6-address?         empty
    |  |        +--:(ntp-multicast-ipv6-address)
    |  |        |  +--rw ntp-multicast-ipv6-address?         empty
    |  |        +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
    |  |           +--rw ipv6-prefix?                        inet:ipv6-prefix
    |  +--rw (vlan-tag-match-type)?
    |  |  x--:(match-all)
    |  |  |  x--rw match-all?                                empty
    |  |  +--:(untagged)
    |  |  |  +--rw untagged?                                 empty
    |  |  +--:(vlan-tagged)
    |  |     +--rw tag* [index]
    |  |        +--rw index        tag-index
    |  |        +--rw dot1q-tag
    |  |           +--rw tag-type    union
    |  |           +--rw vlan-id     union
    |  |           +--rw pbit?       union
    |  |           +--rw dei?        union
    |  +--rw ethernet-frame-type?                            bbf-dot1qt:ether-type-or-acronym
    |  +--rw (protocols)?
    |  |  +--:(any-protocol)
    |  |  |  +--rw any-protocol?                             empty
    |  |  +--:(protocol)
    |  |     +--rw protocol*                                 protocols
    |  +--rw dscp-range?                                     dscp-range-or-any {dscp-match-criteria}?
    +--rw exclude-criteria {exclude-criteria}?
       +--rw frame-destination-filter!
       |  +--rw destination-mac-address* [name]
       |  |  +--rw name                               bbf-yang:string-ascii64
       |  |  +--rw (mac-address)?
       |  |     +--:(any-multicast-mac-address)
       |  |     |  +--rw any-multicast-mac-address?   empty
       |  |     +--:(unicast-address)
       |  |     |  +--rw unicast-address?             empty
       |  |     +--:(broadcast-address)
       |  |     |  +--rw broadcast-address?           empty
       |  |     +--:(cfm-multicast-address)
       |  |     |  +--rw cfm-multicast-address?       empty
       |  |     +--:(ipv4-multicast-address)
       |  |     |  +--rw ipv4-multicast-address?      empty
       |  |     +--:(ipv6-multicast-address)
       |  |     |  +--rw ipv6-multicast-address?      empty
       |  |     +--:(mac-address-filter)
       |  |        +--rw mac-address-value?           yang:mac-address
       |  |        +--rw mac-address-mask?            yang:mac-address
       |  +--rw destination-ipv4-address* [name]
       |  |  +--rw name                                 bbf-yang:string-ascii64
       |  |  +--rw (ipv4-address)?
       |  |     +--:(any-multicast-ipv4-address)
       |  |     |  +--rw any-multicast-ipv4-address?    empty
       |  |     +--:(all-hosts-multicast-address)
       |  |     |  +--rw all-hosts-multicast-address?   empty
       |  |     +--:(rip-multicast-address)
       |  |     |  +--rw rip-multicast-address?         empty
       |  |     +--:(ntp-multicast-address)
       |  |     |  +--rw ntp-multicast-address?         empty
       |  |     +--:(ipv4-prefix) {bbf-classif:filter-on-ip-prefix}?
       |  |        +--rw ipv4-prefix?                   inet:ipv4-prefix
       |  +--rw destination-ipv6-address* [name]
       |     +--rw name                                      bbf-yang:string-ascii64
       |     +--rw (ipv6-address)?
       |        +--:(any-multicast-ipv6-address)
       |        |  +--rw any-multicast-ipv6-address?         empty
       |        +--:(all-nodes-multicast-ipv6-address)
       |        |  +--rw all-nodes-multicast-ipv6-address?   empty
       |        +--:(rip-multicast-ipv6-address)
       |        |  +--rw rip-multicast-ipv6-address?         empty
       |        +--:(ntp-multicast-ipv6-address)
       |        |  +--rw ntp-multicast-ipv6-address?         empty
       |        +--:(ipv6-prefix) {bbf-classif:filter-on-ip-prefix}?
       |           +--rw ipv6-prefix?                        inet:ipv6-prefix
       +--rw ethernet-frame-type*        bbf-dot1qt:ether-type-or-acronym
       +--rw protocol*                   protocols
       +--rw dscp-range?                 bbf-inet:dscp-range {dscp-match-criteria}?
  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:ingress-rule/bbf-subif:rule/bbf-subif:ingress-rewrite:
    +--rw pop-tags?   uint8
    +--rw push-tag* [index]
       +--rw index        bbf-classif:tag-index
       +--rw dot1q-tag
          +--rw tag-type                             union
          +--rw vlan-id                              union
          +--rw vlan-id-from-tag-index-or-discard    bbf-classif:tag-index {bbf-subif-tag:copy-vlan-id-from-tag-index}?
          +--rw (pbit)
          |  +--:(write-pbit-0)
          |  |  +--rw write-pbit-0?                  empty
          |  +--:(copy-pbit-from-input-or-0)
          |  |  +--rw pbit-from-tag-index?           bbf-classif:tag-index
          |  +--:(write-pbit-value) {bbf-subif-tag:write-pbit-value-in-vlan-tag}?
          |     +--rw write-pbit?                    bbf-dot1qt:pbit
          +--rw (dei)
             +--:(write-dei-0)
             |  +--rw write-dei-0?                   empty
             +--:(copy-dei-from-input-or-0)
             |  +--rw dei-from-tag-index?            bbf-classif:tag-index
             +--:(write-dei-1)
                +--rw write-dei-1?                   empty
  augment /if:interfaces/if:interface/bbf-subif:frame-processing/bbf-subif:inline-frame-processing/bbf-subif:inline-frame-processing/bbf-subif:egress-rewrite:
    +--rw pop-tags?   uint8
    +--rw push-tag* [index]
       +--rw index        bbf-classif:tag-index
       +--rw dot1q-tag
          +--rw tag-type                             union
          +--rw vlan-id                              union
          +--rw vlan-id-from-tag-index-or-discard    bbf-classif:tag-index {bbf-subif-tag:copy-vlan-id-from-tag-index}?
          +--rw (pbit)
          |  +--:(write-pbit-0)
          |  |  +--rw write-pbit-0?                  empty
          |  +--:(copy-pbit-from-input-or-0)
          |  |  +--rw pbit-from-tag-index?           bbf-classif:tag-index
          |  +--:(write-pbit-value) {bbf-subif-tag:write-pbit-value-in-vlan-tag}?
          |     +--rw write-pbit?                    bbf-dot1qt:pbit
          +--rw (dei)
             +--:(write-dei-0)
             |  +--rw write-dei-0?                   empty
             +--:(copy-dei-from-input-or-0)
             |  +--rw dei-from-tag-index?            bbf-classif:tag-index
             +--:(write-dei-1)
                +--rw write-dei-1?                   empty
```

- Module Dependencies: `ietf-interfaces` `bbf-dot1q-types` `bbf-if-type` `bbf-sub-interfaces` `bbf-frame-classification`

## 8.60. bbf-subscriber-profiles

```text
module: bbf-subscriber-profiles
  +--rw subscriber-profiles
     +--rw subscriber-profile* [name]
        +--rw name             bbf-yang:string-ascii64
        +--rw circuit-id?      bbf-yang:string-ascii63-or-empty
        +--rw remote-id?       bbf-yang:string-ascii63-or-empty
        +--rw subscriber-id?   bbf-yang:string-ascii63-or-empty

  augment /if:interfaces/if:interface:
    +--rw subscriber-profile
       +--rw profile?   subscriber-profile-ref
  augment /sys:system:
    +--rw subscriber-management
       +--rw access-node-id?   bbf-yang:string-ascii63-or-empty
```

- Module Dependencies: `ietf-interfaces` `ietf-system` `bbf-yang-types`

## 8.61. bbf-xponani-ani-body

```text
submodule: bbf-xponani-ani-body (belongs-to bbf-xponani)

  augment /if:interfaces/if:interface:
    +--rw ani
       +--rw upstream-fec?                       boolean
       +--rw management-gemport-aes-indicator?   boolean
       +--rw onu-id?                             bbf-xpon-types:onu-id {bbf-xponani:configurable-ani-onu-id}?
       +--rw management-gemport-id?              uint32 {bbf-xponani:configurable-ani-management-gem-port-id}?
       +--rw notifiable-defect*                  identityref {bbf-xponani:ani-defect-notifications}?
  augment /if:interfaces/if:interface:
    +--rw onu-v-enet
  augment /if:interfaces/if:interface:
    +--rw onu-v-vrefpoint
  augment /if:interfaces-state/if:interface:
    +--ro ani
       +--ro onu-id?                      bbf-xpon-types:onu-id
       +--ro onu-serial-number            bbf-xpon-types:onu-serial-number {bbf-xponani:ani-onu-serial-number-reporting}?
       +--ro channel-partition-id?        uint8
       +--ro management-tcont-alloc-id?   uint32
       +--ro management-gemport-id?       uint32
       +--ro active-defects*              identityref {bbf-xponani:ani-defects}?
       +---n defect-state-change {bbf-xponani:ani-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-xpon-types` `bbf-xpon-defects` `bbf-xponani-base`

## 8.62. bbf-xponani-base

```text
submodule: bbf-xponani-base (belongs-to bbf-xponani)

  augment /if:interfaces/if:interface:
    +--rw ani
  augment /if:interfaces/if:interface:
    +--rw onu-v-enet
  augment /if:interfaces/if:interface:
    +--rw onu-v-vrefpoint
  augment /if:interfaces-state/if:interface:
    +--ro ani
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type`

## 8.63. bbf-xponani

```text
module: bbf-xponani

  augment /if:interfaces/if:interface:
    +--rw ani
       +--rw upstream-fec?                       boolean
       +--rw management-gemport-aes-indicator?   boolean
       +--rw onu-id?                             bbf-xpon-types:onu-id {bbf-xponani:configurable-ani-onu-id}?
       +--rw management-gemport-id?              uint32 {bbf-xponani:configurable-ani-management-gem-port-id}?
       +--rw notifiable-defect*                  identityref {bbf-xponani:ani-defect-notifications}?
  augment /if:interfaces/if:interface:
    +--rw onu-v-enet
       +--rw ani    if:interface-ref
  augment /if:interfaces/if:interface:
    +--rw onu-v-vrefpoint
       +--rw related-onu    if:interface-ref
  augment /if:interfaces-state/if:interface:
    +--ro ani
       +--ro onu-id?                      bbf-xpon-types:onu-id
       +--ro onu-serial-number            bbf-xpon-types:onu-serial-number {bbf-xponani:ani-onu-serial-number-reporting}?
       +--ro channel-partition-id?        uint8
       +--ro management-tcont-alloc-id?   uint32
       +--ro management-gemport-id?       uint32
       +--ro active-defects*              identityref {bbf-xponani:ani-defects}?
       +---n defect-state-change {bbf-xponani:ani-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `bbf-xponani-base` `bbf-xponani-ani-body` `bbf-xponani-v-enet-body`

## 8.64. bbf-xponani-power-management

```text
module: bbf-xponani-power-management
  +--rw xponani-power-management-profiles {bbf-xpon-pwr:xpon-power-management}?
     +--rw power-management-profile* [name]
        +--rw name             bbf-yang:string-ascii64
        +--rw mode*            identityref
        +--rw irxoff?          uint32
        +--rw iaware?          uint32
        +--rw ihold?           uint16
        +--rw idoze?           uint32
        +--rw icyclic-sleep?   uint32

  augment /if:interfaces/if:interface/bbf-xponani:ani:
    +--rw power-management! {bbf-xpon-pwr:xpon-power-management}?
       +--rw power-management-profile-ref    -> /xponani-power-management-profiles/power-management-profile/name
  augment /if:interfaces-state/if:interface/bbf-xponani:ani:
    +--ro power-management {bbf-xpon-pwr:xpon-power-management}?
       +--ro power-management-state?   identityref
       +--ro capability*               identityref
       +--ro itransinit?               uint16
       +--ro itxinit?                  uint16
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-xponani` `bbf-xpon-power-management`

## 8.65. bbf-xponani-v-enet-body

```text
submodule: bbf-xponani-v-enet-body (belongs-to bbf-xponani)

  augment /if:interfaces/if:interface:
    +--rw ani
  augment /if:interfaces/if:interface:
    +--rw onu-v-enet
       +--rw ani    if:interface-ref
  augment /if:interfaces/if:interface:
    +--rw onu-v-vrefpoint
       +--rw related-onu    if:interface-ref
  augment /if:interfaces-state/if:interface:
    +--ro ani
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `bbf-xponani-base`

## 8.66. bbf-xpon-base

```text
submodule: bbf-xpon-base (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |     +--rw activated?                             boolean
  |     +--rw all-ictp-proxies-all-channel-groups
  |        +--rw proxy* [name]
  |           +--rw name                                              bbf-yang:string-ascii64
  |           +--rw host                                              inet:host
  |           +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |           +--rw all-channel-terminations-proxied-by-this-proxy
  |              +--rw channel-termination* [channel-termination-ref]
  |                 +--rw channel-termination-ref               if:interface-ref
  |                 +--rw channel-termination-ictp-activated?   boolean
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `ietf-interfaces` `ietf-inet-types` `bbf-xpon-types` `bbf-xpon-if-type` `bbf-yang-types`

## 8.67. bbf-xpon-channel-group-body

```text
submodule: bbf-xpon-channel-group-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |     +--rw activated?                             boolean
  |     +--rw all-ictp-proxies-all-channel-groups
  |        +--rw proxy* [name]
  |           +--rw name                                              bbf-yang:string-ascii64
  |           +--rw host                                              inet:host
  |           +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |           +--rw all-channel-terminations-proxied-by-this-proxy
  |              +--rw channel-termination* [channel-termination-ref]
  |                 +--rw channel-termination-ref               if:interface-ref
  |                 +--rw channel-termination-ictp-activated?   boolean
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
       +--rw polling-period?     uint32
       +--rw raman-mitigation?   bbf-xpon-types:raman-mitigation-type
       +--rw system-id?          string
       +--rw pon-pools {bbf-xpon:pon-pools}?
          +--rw pon-pool* [name]
             +--rw name                       bbf-yang:string-ascii64
             +--rw channel-termination-ref?   if:interface-ref
             +--rw alloc-id-values?           alloc-id-values
             +--rw gemport-values?            gemport-values
             +--rw onu-id-values?             onu-id-values
  augment /if:interfaces/if:interface:
    +--rw channel-partition
       +--rw channel-group-ref                     if:interface-ref
       +--rw channel-partition-index?              uint8
       +--rw downstream-fec?                       boolean
       +--rw closest-onu-distance?                 uint16
       +--rw maximum-differential-xpon-distance?   uint16
       +--rw authentication-method?                bbf-xpon-types:auth-method-type
       +--rw multicast-aes-indicator?              boolean
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
       +--ro allocated-upstream-channel-ids
       |  +--ro channel-id*   bbf-xpon-types:composite-channel-id-type
       +--ro allocated-downstream-channel-ids
       |  +--ro downstream-channel-id*   bbf-xpon-types:composite-channel-id-type
       +--ro allocated-downstream-wavelengths
       |  +--ro wavelength*   bbf-xpon-types:composite-downstream-wavelength-type
       +--ro pon-pools {bbf-xpon:pon-pools}?
          +--ro pon-pool* [name]
             +--ro name                       bbf-yang:string-ascii64
             +--ro channel-termination-ref?   if:interface-state-ref
             +--ro consumed-resources
             |  +--ro alloc-id-values?   alloc-id-values
             |  +--ro gemport-values?    gemport-values
             |  +--ro onu-ids?           onu-id-values
             +--ro available-resources
                +--ro alloc-id-values?   alloc-id-values
                +--ro gemport-values?    gemport-values
                +--ro onu-ids?           onu-id-values
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `bbf-xpon-types` `bbf-yang-types` `ietf-interfaces` `bbf-xpon-if-type` `bbf-xpon-base` `bbf-xpon-channel-partition-body`

## 8.68. bbf-xpon-channel-pair-body

```text
submodule: bbf-xpon-channel-pair-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |  |  +--rw activated?                             boolean
  |  |  +--rw all-ictp-proxies-all-channel-groups
  |  |     +--rw proxy* [name]
  |  |        +--rw name                                              bbf-yang:string-ascii64
  |  |        +--rw host                                              inet:host
  |  |        +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |  |        +--rw all-channel-terminations-proxied-by-this-proxy
  |  |           +--rw channel-termination* [channel-termination-ref]
  |  |              +--rw channel-termination-ref               if:interface-ref
  |  |              +--rw channel-termination-ictp-activated?   boolean
  |  +--rw wavelength-profiles
  |     +--rw wavelength-profile* [name]
  |        +--rw name                     bbf-yang:string-ascii64
  |        +--rw upstream-channel-id?     uint8
  |        +--rw downstream-channel-id?   uint8
  |        +--rw downstream-wavelength?   uint32
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
       +--rw channel-group-ref?        if:interface-ref
       +--rw channel-partition-ref?    if:interface-ref
       +--rw wavelength-prof-ref?      wavelength-prof-ref
       +--rw channel-pair-type         identityref
       +--rw channel-pair-line-rate?   identityref
       +--rw gpon-pon-id-interval?     uint16
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
       +--ro actual-downstream-wavelength?   uint32
       +--ro primary-ct-assigned?            boolean
       +--ro secondary-ct-assigned?          boolean
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `bbf-xpon-types` `ietf-interfaces` `bbf-xpon-if-type` `bbf-xpon-base` `bbf-xpon-wavelength-profile-body`

## 8.69. bbf-xpon-channel-partition-body

```text
submodule: bbf-xpon-channel-partition-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |     +--rw activated?                             boolean
  |     +--rw all-ictp-proxies-all-channel-groups
  |        +--rw proxy* [name]
  |           +--rw name                                              bbf-yang:string-ascii64
  |           +--rw host                                              inet:host
  |           +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |           +--rw all-channel-terminations-proxied-by-this-proxy
  |              +--rw channel-termination* [channel-termination-ref]
  |                 +--rw channel-termination-ref               if:interface-ref
  |                 +--rw channel-termination-ictp-activated?   boolean
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
       +--rw channel-group-ref                     if:interface-ref
       +--rw channel-partition-index?              uint8
       +--rw downstream-fec?                       boolean
       +--rw closest-onu-distance?                 uint16
       +--rw maximum-differential-xpon-distance?   uint16
       +--rw authentication-method?                bbf-xpon-types:auth-method-type
       +--rw multicast-aes-indicator?              boolean
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `bbf-xpon-types` `bbf-xpon-base`

## 8.70. bbf-xpon-channel-termination-body

```text
submodule: bbf-xpon-channel-termination-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |     +--rw activated?                             boolean
  |     +--rw all-ictp-proxies-all-channel-groups
  |        +--rw proxy* [name]
  |           +--rw name                                              bbf-yang:string-ascii64
  |           +--rw host                                              inet:host
  |           +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |           +--rw all-channel-terminations-proxied-by-this-proxy
  |              +--rw channel-termination* [channel-termination-ref]
  |                 +--rw channel-termination-ref               if:interface-ref
  |                 +--rw channel-termination-ictp-activated?   boolean
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
       +--rw channel-pair-ref?                     if:interface-ref
       +--rw channel-termination-type              identityref
       +--rw meant-for-type-b-primary-role?        boolean
       +--rw ngpon2-twdm-admin-label?              uint32
       +--rw ngpon2-ptp-admin-label?               uint32
       +--rw xgs-pon-id?                           uint32
       +--rw xgpon-pon-id?                         uint32
       +--rw gpon-pon-id?                          bbf-xpon-types:string-hex14
       +--rw pon-tag?                              string
       +--rw ber-calc-period?                      uint32
       +--rw location?                             identityref
       +--rw onu-phase-drift-monitoring-control {bbf-xpon:onu-phase-drift-monitoring-control}?
       |  +--rw monitoring-interval?   union
       |  +--rw lower-threshold?       union
       |  +--rw upper-threshold?       union
       +--rw burst-profiles {bbf-xpon-burst-prof:configurable-burst-profile}?
       |  +--rw burst-profile* [profile-id]
       |     +--rw profile-id               uint8
       |     +--rw profile-version?         uint8
       |     +--rw delimiter?               string
       |     +--rw preamble?                string
       |     +--rw preamble-repeat-count?   uint8
       +--rw ranging-burst-profile-id?             uint8 {bbf-xpon-burst-prof:configurable-burst-profile}?
       +--rw notifiable-defect*                    identityref {bbf-xpon:channel-termination-defect-notifications}?
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
       +--ro pon-id-display?        bbf-xpon-types:pon-id-display-type
       +--ro type-b-state?          identityref
       +--ro location?              identityref
       +--ro active-defects*        identityref {bbf-xpon:channel-termination-defects}?
       +--ro sufi-affected-onus*    bbf-xpon-types:onu-serial-number {bbf-xpon:channel-termination-defects}?
       +---n defect-state-change {bbf-xpon:channel-termination-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-xpon-types` `bbf-xpon-if-type` `bbf-xpon-defects` `bbf-xpon-burst-profile` `bbf-xpon-base`

## 8.71. bbf-xpongemtcont-base

```text
submodule: bbf-xpongemtcont-base (belongs-to bbf-xpongemtcont)
  +--rw xpongemtcont
  +--ro xpongemtcont-state
```

- Module Dependencies: `N/A`

## 8.72. bbf-xpongemtcont-gemport-body

```text
submodule: bbf-xpongemtcont-gemport-body (belongs-to bbf-xpongemtcont)
  +--rw xpongemtcont
  |  +--rw traffic-descriptor-profiles
  |  |  +--rw traffic-descriptor-profile* [name]
  |  |     +--rw name                                   string
  |  |     +--rw fixed-bandwidth?                       uint64
  |  |     +--rw assured-bandwidth?                     uint64
  |  |     +--rw maximum-bandwidth                      uint64
  |  |     +--rw priority?                              uint8
  |  |     +--rw weight?                                uint8
  |  |     +--rw additional-bw-eligibility-indicator?   enumeration
  |  +--rw tconts
  |  |  +--rw tcont* [name]
  |  |     +--rw name                              bbf-yang:string-ascii64
  |  |     +--rw alloc-id?                         alloc-id {bbf-xpongemtcont:configurable-alloc-id}?
  |  |     +--rw interface-reference?              if:interface-ref
  |  |     +--rw traffic-descriptor-profile-ref?   -> /xpongemtcont/traffic-descriptor-profiles/traffic-descriptor-profile/name
  |  +--rw gemports
  |     +--rw gemport* [name]
  |        +--rw name                        bbf-yang:string-ascii64
  |        +--rw gemport-id?                 uint32 {bbf-xpongemtcont:configurable-gemport-id}?
  |        +--rw interface?                  if:interface-ref
  |        +--rw traffic-class?              uint8
  |        +--rw downstream-aes-indicator?   boolean
  |        +--rw upstream-aes-indicator?     boolean
  |        +--rw tcont-ref?                  -> /xpongemtcont/tconts/tcont/name
  +--ro xpongemtcont-state
     +--ro tconts
     |  +--ro tcont* [name]
     |     +--ro name               bbf-yang:string-ascii64
     |     +--ro actual-alloc-id?   alloc-id
     +--ro gemports
        +--ro gemport* [name]
           +--ro name                 bbf-yang:string-ascii64
           +--ro actual-gemport-id    uint32
```

- Module Dependencies: `ietf-interfaces` `bbf-yang-types` `bbf-xpongemtcont-base` `bbf-xpongemtcont-tcont-body`

## 8.73. bbf-xpongemtcont-gemport-performance-management

```text
module: bbf-xpongemtcont-gemport-performance-management

  augment /bbf-xpongemtcont:xpongemtcont/bbf-xpongemtcont:gemports/bbf-xpongemtcont:gemport:
    +--rw performance
       +--rw enable?   boolean
  augment /bbf-xpongemtcont:xpongemtcont-state/bbf-xpongemtcont:gemports/bbf-xpongemtcont:gemport:
    +--ro performance
       +--ro intervals-15min
       |  +--ro current
       |  |  +--ro ani-side
       |  |  |  +--ro out-frames?   bbf-yang:performance-counter64
       |  |  |  +--ro in-frames?    bbf-yang:performance-counter64
       |  |  +--ro v-ani-side
       |  |     +--ro out-frames?   bbf-yang:performance-counter64
       |  |     +--ro in-frames?    bbf-yang:performance-counter64
       |  +--ro number-of-intervals?   performance-15min-interval
       |  +--ro non-valid-intervals?   performance-15min-interval
       |  +--ro history* [interval-number]
       |     +--ro interval-number      bbf-if-pm:performance-15min-history-interval
       |     +--ro measured-time?       uint32
       |     +--ro invalid-data-flag?   boolean
       |     +--ro time-stamp?          yang:date-and-time
       |     +--ro ani-side
       |     |  +--ro out-frames?   bbf-yang:performance-counter64
       |     |  +--ro in-frames?    bbf-yang:performance-counter64
       |     +--ro v-ani-side
       |        +--ro out-frames?   bbf-yang:performance-counter64
       |        +--ro in-frames?    bbf-yang:performance-counter64
       +--ro intervals-24hr {bbf-if-pm:performance-24hr}?
          +--ro current
          |  +--ro ani-side
          |  |  +--ro out-frames?   bbf-yang:performance-counter64
          |  |  +--ro in-frames?    bbf-yang:performance-counter64
          |  +--ro v-ani-side
          |     +--ro out-frames?   bbf-yang:performance-counter64
          |     +--ro in-frames?    bbf-yang:performance-counter64
          +--ro number-of-intervals?   performance-24hr-interval
          +--ro non-valid-intervals?   performance-24hr-interval
          +--ro history* [interval-number]
             +--ro interval-number      bbf-if-pm:performance-24hr-history-interval
             +--ro measured-time?       uint32
             +--ro invalid-data-flag?   boolean
             +--ro time-stamp?          yang:date-and-time
             +--ro ani-side
             |  +--ro out-frames?   bbf-yang:performance-counter64
             |  +--ro in-frames?    bbf-yang:performance-counter64
             +--ro v-ani-side
                +--ro out-frames?   bbf-yang:performance-counter64
                +--ro in-frames?    bbf-yang:performance-counter64
```

- Module Dependencies: `bbf-interfaces-performance-management` `bbf-yang-types` `bbf-xpongemtcont`

## 8.74. bbf-xpongemtcont

```text
module: bbf-xpongemtcont
  +--rw xpongemtcont
  |  +--rw traffic-descriptor-profiles
  |  |  +--rw traffic-descriptor-profile* [name]
  |  |     +--rw name                                   string
  |  |     +--rw fixed-bandwidth?                       uint64
  |  |     +--rw assured-bandwidth?                     uint64
  |  |     +--rw maximum-bandwidth                      uint64
  |  |     +--rw priority?                              uint8
  |  |     +--rw weight?                                uint8
  |  |     +--rw additional-bw-eligibility-indicator?   enumeration
  |  +--rw tconts
  |  |  +--rw tcont* [name]
  |  |     +--rw name                              bbf-yang:string-ascii64
  |  |     +--rw alloc-id?                         alloc-id {bbf-xpongemtcont:configurable-alloc-id}?
  |  |     +--rw interface-reference?              if:interface-ref
  |  |     +--rw traffic-descriptor-profile-ref?   -> /xpongemtcont/traffic-descriptor-profiles/traffic-descriptor-profile/name
  |  +--rw gemports
  |     +--rw gemport* [name]
  |        +--rw name                        bbf-yang:string-ascii64
  |        +--rw gemport-id?                 uint32 {bbf-xpongemtcont:configurable-gemport-id}?
  |        +--rw interface?                  if:interface-ref
  |        +--rw traffic-class?              uint8
  |        +--rw downstream-aes-indicator?   boolean
  |        +--rw upstream-aes-indicator?     boolean
  |        +--rw tcont-ref?                  -> /xpongemtcont/tconts/tcont/name
  +--ro xpongemtcont-state
     +--ro tconts
     |  +--ro tcont* [name]
     |     +--ro name               bbf-yang:string-ascii64
     |     +--ro actual-alloc-id?   alloc-id
     +--ro gemports
        +--ro gemport* [name]
           +--ro name                 bbf-yang:string-ascii64
           +--ro actual-gemport-id    uint32
```

- Module Dependencies: `bbf-xpongemtcont-base` `bbf-xpongemtcont-traffic-descriptor-profile-body` `bbf-xpongemtcont-tcont-body` `bbf-xpongemtcont-gemport-body`

## 8.75. bbf-xpongemtcont-qos

```text
module: bbf-xpongemtcont-qos

  augment /bbf-xpongemtcont:xpongemtcont/bbf-xpongemtcont:tconts/bbf-xpongemtcont:tcont:
    +--rw tm-root!
       +--rw (children-type)?
       |  +--:(queues)
       |     +--rw queue* [local-queue-id]
       |     |  +--rw local-queue-id                queue-id
       |     |  +--rw bac-name?                     -> /bbf-qos-tm:tm-profiles/bac-entry/name
       |     |  +--rw (queue-scheduling-cfg-type)?
       |     |  |  +--:(inline)
       |     |  |  |  +--rw priority?               scheduling-priority
       |     |  |  |  +--rw weight?                 scheduling-weight
       |     |  |  |  +--rw extended-weight?        extended-scheduling-weight {bbf-qos-tm:extended-scheduling-weight}?
       |     |  |  +--:(dual-rate-scheduling) {bbf-qos-tm:dual-rate-scheduling-from-queue}?
       |     |  |     +--rw dual-rate-scheduling
       |     |  |        +--rw cir-traffic
       |     |  |        |  +--rw priority?          extended-scheduling-priority
       |     |  |        |  +--rw extended-weight?   extended-scheduling-weight
       |     |  |        +--rw eir-traffic
       |     |  |           +--rw priority?          extended-scheduling-priority
       |     |  |           +--rw extended-weight?   extended-scheduling-weight
       |     |  +--rw pre-emption?                  boolean {bbf-qos-tm:pre-emption}?
       |     +--rw queue-statistics-enable?           boolean {bbf-qos-tm:queue-statistics}?
       +--rw tc-id-2-queue-id-mapping-profile-name?   -> /bbf-qos-tm:tm-profiles/tc-id-2-queue-id-mapping-profile/name {bbf-qos-tm:tc-id-2-queue-id-mapping-config}?
```

- Module Dependencies: `bbf-xpongemtcont` `bbf-qos-traffic-mngt`

## 8.76. bbf-xpongemtcont-tcont-body

```text
submodule: bbf-xpongemtcont-tcont-body (belongs-to bbf-xpongemtcont)
  +--rw xpongemtcont
  |  +--rw traffic-descriptor-profiles
  |  |  +--rw traffic-descriptor-profile* [name]
  |  |     +--rw name                                   string
  |  |     +--rw fixed-bandwidth?                       uint64
  |  |     +--rw assured-bandwidth?                     uint64
  |  |     +--rw maximum-bandwidth                      uint64
  |  |     +--rw priority?                              uint8
  |  |     +--rw weight?                                uint8
  |  |     +--rw additional-bw-eligibility-indicator?   enumeration
  |  +--rw tconts
  |     +--rw tcont* [name]
  |        +--rw name                              bbf-yang:string-ascii64
  |        +--rw alloc-id?                         alloc-id {bbf-xpongemtcont:configurable-alloc-id}?
  |        +--rw interface-reference?              if:interface-ref
  |        +--rw traffic-descriptor-profile-ref?   -> /xpongemtcont/traffic-descriptor-profiles/traffic-descriptor-profile/name
  +--ro xpongemtcont-state
     +--ro tconts
        +--ro tcont* [name]
           +--ro name               bbf-yang:string-ascii64
           +--ro actual-alloc-id?   alloc-id
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `bbf-yang-types` `bbf-xpongemtcont-base` `bbf-xpongemtcont-traffic-descriptor-profile-body`

## 8.77. bbf-xpongemtcont-traffic-descriptor-profile-body

```text
submodule: bbf-xpongemtcont-traffic-descriptor-profile-body (belongs-to bbf-xpongemtcont)
  +--rw xpongemtcont
  |  +--rw traffic-descriptor-profiles
  |     +--rw traffic-descriptor-profile* [name]
  |        +--rw name                                   string
  |        +--rw fixed-bandwidth?                       uint64
  |        +--rw assured-bandwidth?                     uint64
  |        +--rw maximum-bandwidth                      uint64
  |        +--rw priority?                              uint8
  |        +--rw weight?                                uint8
  |        +--rw additional-bw-eligibility-indicator?   enumeration
  +--ro xpongemtcont-state
```

- Module Dependencies: `bbf-xpongemtcont-base`

## 8.78. bbf-xpon

```text
module: bbf-xpon
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |  |  +--rw activated?                             boolean
  |  |  +--rw all-ictp-proxies-all-channel-groups
  |  |     +--rw proxy* [name]
  |  |        +--rw name                                              bbf-yang:string-ascii64
  |  |        +--rw host                                              inet:host
  |  |        +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |  |        +--rw all-channel-terminations-proxied-by-this-proxy
  |  |           +--rw channel-termination* [channel-termination-ref]
  |  |              +--rw channel-termination-ref               if:interface-ref
  |  |              +--rw channel-termination-ictp-activated?   boolean
  |  +--rw wavelength-profiles
  |  |  +--rw wavelength-profile* [name]
  |  |     +--rw name                     bbf-yang:string-ascii64
  |  |     +--rw upstream-channel-id?     uint8
  |  |     +--rw downstream-channel-id?   uint8
  |  |     +--rw downstream-wavelength?   uint32
  |  +--rw multicast-gemports
  |  |  +--rw multicast-gemport* [name]
  |  |     +--rw name             bbf-yang:string-ascii64
  |  |     +--rw gemport-id?      uint32
  |  |     +--rw interface?       if:interface-ref
  |  |     +--rw traffic-class?   uint8
  |  |     +--rw is-broadcast?    boolean
  |  +--rw multicast-distribution-set
  |     +--rw multicast-set* [name]
  |        +--rw name                         bbf-yang:string-ascii64
  |        +--rw multicast-gemport-ref?       multicast-gemport-ref
  |        +--rw (multicast-vlans)?
  |           +--:(vlan-list)
  |           |  +--rw vlan-list* [multicast-vlan-id]
  |           |     +--rw multicast-vlan-id    uint16
  |           +--:(all-multicast-vlans)
  |              +--rw all-multicast-vlans?   empty
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
       +--rw polling-period?     uint32
       +--rw raman-mitigation?   bbf-xpon-types:raman-mitigation-type
       +--rw system-id?          string
       +--rw pon-pools {bbf-xpon:pon-pools}?
          +--rw pon-pool* [name]
             +--rw name                       bbf-yang:string-ascii64
             +--rw channel-termination-ref?   if:interface-ref
             +--rw alloc-id-values?           alloc-id-values
             +--rw gemport-values?            gemport-values
             +--rw onu-id-values?             onu-id-values
  augment /if:interfaces/if:interface:
    +--rw channel-partition
       +--rw channel-group-ref                     if:interface-ref
       +--rw channel-partition-index?              uint8
       +--rw downstream-fec?                       boolean
       +--rw closest-onu-distance?                 uint16
       +--rw maximum-differential-xpon-distance?   uint16
       +--rw authentication-method?                bbf-xpon-types:auth-method-type
       +--rw multicast-aes-indicator?              boolean
  augment /if:interfaces/if:interface:
    +--rw channel-pair
       +--rw channel-group-ref?        if:interface-ref
       +--rw channel-partition-ref?    if:interface-ref
       +--rw wavelength-prof-ref?      wavelength-prof-ref
       +--rw channel-pair-type         identityref
       +--rw channel-pair-line-rate?   identityref
       +--rw gpon-pon-id-interval?     uint16
  augment /if:interfaces/if:interface:
    +--rw channel-termination
       +--rw channel-pair-ref?                     if:interface-ref
       +--rw channel-termination-type              identityref
       +--rw meant-for-type-b-primary-role?        boolean
       +--rw ngpon2-twdm-admin-label?              uint32
       +--rw ngpon2-ptp-admin-label?               uint32
       +--rw xgs-pon-id?                           uint32
       +--rw xgpon-pon-id?                         uint32
       +--rw gpon-pon-id?                          bbf-xpon-types:string-hex14
       +--rw pon-tag?                              string
       +--rw ber-calc-period?                      uint32
       +--rw location?                             identityref
       +--rw onu-phase-drift-monitoring-control {bbf-xpon:onu-phase-drift-monitoring-control}?
       |  +--rw monitoring-interval?   union
       |  +--rw lower-threshold?       union
       |  +--rw upper-threshold?       union
       +--rw burst-profiles {bbf-xpon-burst-prof:configurable-burst-profile}?
       |  +--rw burst-profile* [profile-id]
       |     +--rw profile-id               uint8
       |     +--rw profile-version?         uint8
       |     +--rw delimiter?               string
       |     +--rw preamble?                string
       |     +--rw preamble-repeat-count?   uint8
       +--rw ranging-burst-profile-id?             uint8 {bbf-xpon-burst-prof:configurable-burst-profile}?
       +--rw notifiable-defect*                    identityref {bbf-xpon:channel-termination-defect-notifications}?
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
       +--ro allocated-upstream-channel-ids
       |  +--ro channel-id*   bbf-xpon-types:composite-channel-id-type
       +--ro allocated-downstream-channel-ids
       |  +--ro downstream-channel-id*   bbf-xpon-types:composite-channel-id-type
       +--ro allocated-downstream-wavelengths
       |  +--ro wavelength*   bbf-xpon-types:composite-downstream-wavelength-type
       +--ro pon-pools {bbf-xpon:pon-pools}?
          +--ro pon-pool* [name]
             +--ro name                       bbf-yang:string-ascii64
             +--ro channel-termination-ref?   if:interface-state-ref
             +--ro consumed-resources
             |  +--ro alloc-id-values?   alloc-id-values
             |  +--ro gemport-values?    gemport-values
             |  +--ro onu-ids?           onu-id-values
             +--ro available-resources
                +--ro alloc-id-values?   alloc-id-values
                +--ro gemport-values?    gemport-values
                +--ro onu-ids?           onu-id-values
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
       +--ro actual-downstream-wavelength?   uint32
       +--ro primary-ct-assigned?            boolean
       +--ro secondary-ct-assigned?          boolean
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
       +--ro pon-id-display?        bbf-xpon-types:pon-id-display-type
       +--ro type-b-state?          identityref
       +--ro location?              identityref
       +--ro active-defects*        identityref {bbf-xpon:channel-termination-defects}?
       +--ro sufi-affected-onus*    bbf-xpon-types:onu-serial-number {bbf-xpon:channel-termination-defects}?
       +---n defect-state-change {bbf-xpon:channel-termination-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `bbf-xpon-base` `bbf-xpon-wavelength-profile-body` `bbf-xpon-multicast-gemport-body` `bbf-xpon-channel-group-body` `bbf-xpon-channel-partition-body` `bbf-xpon-channel-pair-body` `bbf-xpon-channel-termination-body` `bbf-xpon-multicast-distribution-set-body`

## 8.79. bbf-xpon-multicast-distribution-set-body

```text
submodule: bbf-xpon-multicast-distribution-set-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |  |  +--rw activated?                             boolean
  |  |  +--rw all-ictp-proxies-all-channel-groups
  |  |     +--rw proxy* [name]
  |  |        +--rw name                                              bbf-yang:string-ascii64
  |  |        +--rw host                                              inet:host
  |  |        +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |  |        +--rw all-channel-terminations-proxied-by-this-proxy
  |  |           +--rw channel-termination* [channel-termination-ref]
  |  |              +--rw channel-termination-ref               if:interface-ref
  |  |              +--rw channel-termination-ictp-activated?   boolean
  |  +--rw multicast-gemports
  |  |  +--rw multicast-gemport* [name]
  |  |     +--rw name             bbf-yang:string-ascii64
  |  |     +--rw gemport-id?      uint32
  |  |     +--rw interface?       if:interface-ref
  |  |     +--rw traffic-class?   uint8
  |  |     +--rw is-broadcast?    boolean
  |  +--rw multicast-distribution-set
  |     +--rw multicast-set* [name]
  |        +--rw name                         bbf-yang:string-ascii64
  |        +--rw multicast-gemport-ref?       multicast-gemport-ref
  |        +--rw (multicast-vlans)?
  |           +--:(vlan-list)
  |           |  +--rw vlan-list* [multicast-vlan-id]
  |           |     +--rw multicast-vlan-id    uint16
  |           +--:(all-multicast-vlans)
  |              +--rw all-multicast-vlans?   empty
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `bbf-yang-types` `bbf-xpon-base` `bbf-xpon-multicast-gemport-body`

## 8.80. bbf-xpon-multicast-gemport-body

```text
submodule: bbf-xpon-multicast-gemport-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |  |  +--rw activated?                             boolean
  |  |  +--rw all-ictp-proxies-all-channel-groups
  |  |     +--rw proxy* [name]
  |  |        +--rw name                                              bbf-yang:string-ascii64
  |  |        +--rw host                                              inet:host
  |  |        +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |  |        +--rw all-channel-terminations-proxied-by-this-proxy
  |  |           +--rw channel-termination* [channel-termination-ref]
  |  |              +--rw channel-termination-ref               if:interface-ref
  |  |              +--rw channel-termination-ictp-activated?   boolean
  |  +--rw multicast-gemports
  |     +--rw multicast-gemport* [name]
  |        +--rw name             bbf-yang:string-ascii64
  |        +--rw gemport-id?      uint32
  |        +--rw interface?       if:interface-ref
  |        +--rw traffic-class?   uint8
  |        +--rw is-broadcast?    boolean
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `bbf-yang-types` `bbf-xpon-base`

## 8.81. bbf-xpon-onu-state

```text
module: bbf-xpon-onu-state

  augment /if:interfaces/if:interface/bbf-xpon:channel-termination:
    +--rw notifiable-onu-presence-states*   identityref
  augment /if:interfaces-state/if:interface/bbf-xpon:channel-termination:
    +--ro onus-present-on-local-channel-termination
    |  +--ro onu* [detected-serial-number]
    |     +--ro detected-serial-number      bbf-xpon-types:onu-serial-number
    |     +--ro onu-presence-state          identityref
    |     +--ro onu-id?                     bbf-xpon-types:onu-id
    |     +--ro detected-registration-id?   bbf-xpon-types:onu-registration-id
    |     +--ro v-ani-ref?                  if:interface-ref
    |     +--ro onu-detected-datetime       yang:date-and-time
    |     +--ro onu-state-last-change       yang:date-and-time
    +---n onu-presence-state-change
       +--ro detected-serial-number      bbf-xpon-types:onu-serial-number
       +--ro last-change                 yang:date-and-time
       +--ro onu-presence-state          identityref
       +--ro onu-id?                     bbf-xpon-types:onu-id
       +--ro detected-registration-id?   bbf-xpon-types:onu-registration-id
       +--ro v-ani-ref?                  if:interface-ref
```

- Module Dependencies: `ietf-interfaces` `ietf-yang-types` `bbf-xpon-types` `bbf-xpon-onu-types` `bbf-xpon`

## 8.82. bbf-xpon-performance-management

```text
module: bbf-xpon-performance-management

  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-15min/bbf-if-pm:current:
    +--ro xpon
       +--ro phy
       |  +--ro corrected-fec-bytes?           bbf-yang:performance-counter64
       |  +--ro corrected-fec-codewords?       bbf-yang:performance-counter64
       |  +--ro uncorrectable-fec-codewords?   bbf-yang:performance-counter64
       |  +--ro in-fec-codewords?              bbf-yang:performance-counter64
       |  +--ro fec-seconds?                   bbf-yang:performance-counter32
       |  +--ro in-bip-protected-words?        bbf-yang:performance-counter64
       |  +--ro in-bip-errors?                 bbf-yang:performance-counter64
       |  +--ro psbd-hec-errors?               bbf-yang:performance-counter64
       |  +--ro fs-hec-errors?                 bbf-yang:performance-counter64
       |  +--ro unknown-profiles?              bbf-yang:performance-counter64
       +--ro lods
       |  +--ro total-events?       bbf-yang:performance-counter64
       |  +--ro restored
       |  |  +--ro operating-twdm?        bbf-yang:performance-counter64
       |  |  +--ro precfg-protect-twdm?   bbf-yang:performance-counter64
       |  |  +--ro discretionary-twdm?    bbf-yang:performance-counter64
       |  +--ro onu-reactivation
       |     +--ro no-sync-reacquired?                    bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-precfg-twdm?          bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-discretionary-twdm?   bbf-yang:performance-counter64
       +--ro gemport
       |  +--ro out-frames?                  bbf-yang:performance-counter64
       |  +--ro in-frames?                   bbf-yang:performance-counter64
       |  +--ro out-frames-lf-bit-not-set?   bbf-yang:performance-counter64
       |  +--ro hec-errors?                  bbf-yang:performance-counter64
       |  +--ro fs-frame-words-lost?         bbf-yang:performance-counter64
       |  +--ro key-errors?                  bbf-yang:performance-counter64
       +--ro utilization
       |  +--ro out-bytes-non-idle-xgem-frames?   bbf-yang:performance-counter64
       |  +--ro in-bytes-non-idle-xgem-frames?    bbf-yang:performance-counter64
       |  +--ro bw-not-assigned?                  bbf-yang:performance-counter64
       +--ro ploam
       |  +--ro sn-grants?             bbf-yang:performance-counter64
       |  +--ro mic-errors?            bbf-yang:performance-counter64
       |  +--ro timeouts?              bbf-yang:performance-counter64
       |  +--ro dying-gasps?           bbf-yang:performance-counter64
       |  +--ro downstream-messages
       |  |  +--ro total?                   bbf-yang:performance-counter64
       |  |  +--ro system-profile?          bbf-yang:performance-counter64
       |  |  +--ro channel-profile?         bbf-yang:performance-counter64
       |  |  +--ro burst-profile?           bbf-yang:performance-counter64
       |  |  +--ro assign-onu-id?           bbf-yang:performance-counter64
       |  |  +--ro ranging-time?            bbf-yang:performance-counter64
       |  |  +--ro protect-control?         bbf-yang:performance-counter64
       |  |  +--ro adjust-tx-wavelength
       |  |  |  +--ro total?                  bbf-yang:performance-counter64
       |  |  |  +--ro wavelength-dithering?   bbf-yang:performance-counter64
       |  |  |  +--ro adjustment-amplitude?   bbf-yang:performance-counter64
       |  |  |  +--ro unsatisfied-requests?   bbf-yang:performance-counter64
       |  |  +--ro deactivate-onu-id?       bbf-yang:performance-counter64
       |  |  +--ro disable-serial-number?   bbf-yang:performance-counter64
       |  |  +--ro request-registration?    bbf-yang:performance-counter64
       |  |  +--ro assign-alloc-id?         bbf-yang:performance-counter64
       |  |  +--ro key-control?             bbf-yang:performance-counter64
       |  |  +--ro sleep-allow?             bbf-yang:performance-counter64
       |  |  +--ro tuning-control
       |  |     +--ro request-operation?    bbf-yang:performance-counter64
       |  |     +--ro complete-operation?   bbf-yang:performance-counter64
       |  +--ro calibration-request?   bbf-yang:performance-counter64
       |  +--ro upstream-messages
       |     +--ro total?                      bbf-yang:performance-counter64
       |     +--ro serial-number-onu
       |     |  +--ro inband?   bbf-yang:performance-counter64
       |     |  +--ro amcc?     bbf-yang:performance-counter64
       |     +--ro registration?               bbf-yang:performance-counter64
       |     +--ro key-report?                 bbf-yang:performance-counter64
       |     +--ro acknowledgement?            bbf-yang:performance-counter64
       |     +--ro sleep-request?              bbf-yang:performance-counter64
       |     +--ro tuning-response
       |     |  +--ro ack-nack?            bbf-yang:performance-counter64
       |     |  +--ro complete-rollback?   bbf-yang:performance-counter64
       |     +--ro power-consumption-report?   bbf-yang:performance-counter64
       +--ro activation
       |  +--ro non-discernible-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro foreign-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-new-activations
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-precfg-twdm-channel-handovers
       |     +--ro Inband?   bbf-yang:performance-counter64
       |     +--ro amcc?      bbf-yang:performance-counter64
       +--ro tuning-control
       |  +--ro requests-for-rx-or-rx-and-tx?       bbf-yang:performance-counter64
       |  +--ro requests-for-tx?                    bbf-yang:performance-counter64
       |  +--ro rejected-requests
       |  |  +--ro internal-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro void?    bbf-yang:performance-counter64
       |  |  |  +--ro part?    bbf-yang:performance-counter64
       |  |  |  +--ro tunr?    bbf-yang:performance-counter64
       |  |  |  +--ro lnrt?    bbf-yang:performance-counter64
       |  |  |  +--ro lncd?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro clbr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro fulfilled-requests?                 bbf-yang:performance-counter64
       |  +--ro failed-requests
       |  |  +--ro target-ds-wl-channel-not-found?        bbf-yang:performance-counter64
       |  |  +--ro no-feedback-in-target-us-wl-channel?   bbf-yang:performance-counter64
       |  +--ro resolved-requests?                  bbf-yang:performance-counter64
       |  +--ro failed-requests-rollback
       |  |  +--ro communication-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro lktp?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro failed-requests-onu-reactivation?   bbf-yang:performance-counter64
       +--ro power-levelling
       |  +--ro change-power-level-messages
       |     +--ro rejected?                 bbf-yang:performance-counter64
       |     +--ro without-completion-ack?   bbf-yang:performance-counter64
       +--ro omci
       |  +--ro baseline-messages?    bbf-yang:performance-counter64
       |  +--ro extended-messages?    bbf-yang:performance-counter64
       |  +--ro autonomus-messages?   bbf-yang:performance-counter64
       |  +--ro mic-errors?           bbf-yang:performance-counter64
       +--ro power-monitoring
       |  +--ro tx-optical-power-level?   int32
       +--ro energy-conservation
          +--ro time-spent-in-low-energy-state?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-15min/bbf-if-pm:history:
    +--ro xpon
       +--ro phy
       |  +--ro corrected-fec-bytes?           bbf-yang:performance-counter64
       |  +--ro corrected-fec-codewords?       bbf-yang:performance-counter64
       |  +--ro uncorrectable-fec-codewords?   bbf-yang:performance-counter64
       |  +--ro in-fec-codewords?              bbf-yang:performance-counter64
       |  +--ro fec-seconds?                   bbf-yang:performance-counter32
       |  +--ro in-bip-protected-words?        bbf-yang:performance-counter64
       |  +--ro in-bip-errors?                 bbf-yang:performance-counter64
       |  +--ro psbd-hec-errors?               bbf-yang:performance-counter64
       |  +--ro fs-hec-errors?                 bbf-yang:performance-counter64
       |  +--ro unknown-profiles?              bbf-yang:performance-counter64
       +--ro lods
       |  +--ro total-events?       bbf-yang:performance-counter64
       |  +--ro restored
       |  |  +--ro operating-twdm?        bbf-yang:performance-counter64
       |  |  +--ro precfg-protect-twdm?   bbf-yang:performance-counter64
       |  |  +--ro discretionary-twdm?    bbf-yang:performance-counter64
       |  +--ro onu-reactivation
       |     +--ro no-sync-reacquired?                    bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-precfg-twdm?          bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-discretionary-twdm?   bbf-yang:performance-counter64
       +--ro gemport
       |  +--ro out-frames?                  bbf-yang:performance-counter64
       |  +--ro in-frames?                   bbf-yang:performance-counter64
       |  +--ro out-frames-lf-bit-not-set?   bbf-yang:performance-counter64
       |  +--ro hec-errors?                  bbf-yang:performance-counter64
       |  +--ro fs-frame-words-lost?         bbf-yang:performance-counter64
       |  +--ro key-errors?                  bbf-yang:performance-counter64
       +--ro utilization
       |  +--ro out-bytes-non-idle-xgem-frames?   bbf-yang:performance-counter64
       |  +--ro in-bytes-non-idle-xgem-frames?    bbf-yang:performance-counter64
       |  +--ro bw-not-assigned?                  bbf-yang:performance-counter64
       +--ro ploam
       |  +--ro sn-grants?             bbf-yang:performance-counter64
       |  +--ro mic-errors?            bbf-yang:performance-counter64
       |  +--ro timeouts?              bbf-yang:performance-counter64
       |  +--ro dying-gasps?           bbf-yang:performance-counter64
       |  +--ro downstream-messages
       |  |  +--ro total?                   bbf-yang:performance-counter64
       |  |  +--ro system-profile?          bbf-yang:performance-counter64
       |  |  +--ro channel-profile?         bbf-yang:performance-counter64
       |  |  +--ro burst-profile?           bbf-yang:performance-counter64
       |  |  +--ro assign-onu-id?           bbf-yang:performance-counter64
       |  |  +--ro ranging-time?            bbf-yang:performance-counter64
       |  |  +--ro protect-control?         bbf-yang:performance-counter64
       |  |  +--ro adjust-tx-wavelength
       |  |  |  +--ro total?                  bbf-yang:performance-counter64
       |  |  |  +--ro wavelength-dithering?   bbf-yang:performance-counter64
       |  |  |  +--ro adjustment-amplitude?   bbf-yang:performance-counter64
       |  |  |  +--ro unsatisfied-requests?   bbf-yang:performance-counter64
       |  |  +--ro deactivate-onu-id?       bbf-yang:performance-counter64
       |  |  +--ro disable-serial-number?   bbf-yang:performance-counter64
       |  |  +--ro request-registration?    bbf-yang:performance-counter64
       |  |  +--ro assign-alloc-id?         bbf-yang:performance-counter64
       |  |  +--ro key-control?             bbf-yang:performance-counter64
       |  |  +--ro sleep-allow?             bbf-yang:performance-counter64
       |  |  +--ro tuning-control
       |  |     +--ro request-operation?    bbf-yang:performance-counter64
       |  |     +--ro complete-operation?   bbf-yang:performance-counter64
       |  +--ro calibration-request?   bbf-yang:performance-counter64
       |  +--ro upstream-messages
       |     +--ro total?                      bbf-yang:performance-counter64
       |     +--ro serial-number-onu
       |     |  +--ro inband?   bbf-yang:performance-counter64
       |     |  +--ro amcc?     bbf-yang:performance-counter64
       |     +--ro registration?               bbf-yang:performance-counter64
       |     +--ro key-report?                 bbf-yang:performance-counter64
       |     +--ro acknowledgement?            bbf-yang:performance-counter64
       |     +--ro sleep-request?              bbf-yang:performance-counter64
       |     +--ro tuning-response
       |     |  +--ro ack-nack?            bbf-yang:performance-counter64
       |     |  +--ro complete-rollback?   bbf-yang:performance-counter64
       |     +--ro power-consumption-report?   bbf-yang:performance-counter64
       +--ro activation
       |  +--ro non-discernible-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro foreign-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-new-activations
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-precfg-twdm-channel-handovers
       |     +--ro Inband?   bbf-yang:performance-counter64
       |     +--ro amcc?      bbf-yang:performance-counter64
       +--ro tuning-control
       |  +--ro requests-for-rx-or-rx-and-tx?       bbf-yang:performance-counter64
       |  +--ro requests-for-tx?                    bbf-yang:performance-counter64
       |  +--ro rejected-requests
       |  |  +--ro internal-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro void?    bbf-yang:performance-counter64
       |  |  |  +--ro part?    bbf-yang:performance-counter64
       |  |  |  +--ro tunr?    bbf-yang:performance-counter64
       |  |  |  +--ro lnrt?    bbf-yang:performance-counter64
       |  |  |  +--ro lncd?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro clbr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro fulfilled-requests?                 bbf-yang:performance-counter64
       |  +--ro failed-requests
       |  |  +--ro target-ds-wl-channel-not-found?        bbf-yang:performance-counter64
       |  |  +--ro no-feedback-in-target-us-wl-channel?   bbf-yang:performance-counter64
       |  +--ro resolved-requests?                  bbf-yang:performance-counter64
       |  +--ro failed-requests-rollback
       |  |  +--ro communication-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro lktp?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro failed-requests-onu-reactivation?   bbf-yang:performance-counter64
       +--ro power-levelling
       |  +--ro change-power-level-messages
       |     +--ro rejected?                 bbf-yang:performance-counter64
       |     +--ro without-completion-ack?   bbf-yang:performance-counter64
       +--ro omci
       |  +--ro baseline-messages?    bbf-yang:performance-counter64
       |  +--ro extended-messages?    bbf-yang:performance-counter64
       |  +--ro autonomus-messages?   bbf-yang:performance-counter64
       |  +--ro mic-errors?           bbf-yang:performance-counter64
       +--ro power-monitoring
       |  +--ro tx-optical-power-level?   int32
       +--ro energy-conservation
          +--ro time-spent-in-low-energy-state?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-24hr/bbf-if-pm:current:
    +--ro xpon {bbf-if-pm:performance-24hr}?
       +--ro phy
       |  +--ro corrected-fec-bytes?           bbf-yang:performance-counter64
       |  +--ro corrected-fec-codewords?       bbf-yang:performance-counter64
       |  +--ro uncorrectable-fec-codewords?   bbf-yang:performance-counter64
       |  +--ro in-fec-codewords?              bbf-yang:performance-counter64
       |  +--ro fec-seconds?                   bbf-yang:performance-counter32
       |  +--ro in-bip-protected-words?        bbf-yang:performance-counter64
       |  +--ro in-bip-errors?                 bbf-yang:performance-counter64
       |  +--ro psbd-hec-errors?               bbf-yang:performance-counter64
       |  +--ro fs-hec-errors?                 bbf-yang:performance-counter64
       |  +--ro unknown-profiles?              bbf-yang:performance-counter64
       +--ro lods
       |  +--ro total-events?       bbf-yang:performance-counter64
       |  +--ro restored
       |  |  +--ro operating-twdm?        bbf-yang:performance-counter64
       |  |  +--ro precfg-protect-twdm?   bbf-yang:performance-counter64
       |  |  +--ro discretionary-twdm?    bbf-yang:performance-counter64
       |  +--ro onu-reactivation
       |     +--ro no-sync-reacquired?                    bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-precfg-twdm?          bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-discretionary-twdm?   bbf-yang:performance-counter64
       +--ro gemport
       |  +--ro out-frames?                  bbf-yang:performance-counter64
       |  +--ro in-frames?                   bbf-yang:performance-counter64
       |  +--ro out-frames-lf-bit-not-set?   bbf-yang:performance-counter64
       |  +--ro hec-errors?                  bbf-yang:performance-counter64
       |  +--ro fs-frame-words-lost?         bbf-yang:performance-counter64
       |  +--ro key-errors?                  bbf-yang:performance-counter64
       +--ro utilization
       |  +--ro out-bytes-non-idle-xgem-frames?   bbf-yang:performance-counter64
       |  +--ro in-bytes-non-idle-xgem-frames?    bbf-yang:performance-counter64
       |  +--ro bw-not-assigned?                  bbf-yang:performance-counter64
       +--ro ploam
       |  +--ro sn-grants?             bbf-yang:performance-counter64
       |  +--ro mic-errors?            bbf-yang:performance-counter64
       |  +--ro timeouts?              bbf-yang:performance-counter64
       |  +--ro dying-gasps?           bbf-yang:performance-counter64
       |  +--ro downstream-messages
       |  |  +--ro total?                   bbf-yang:performance-counter64
       |  |  +--ro system-profile?          bbf-yang:performance-counter64
       |  |  +--ro channel-profile?         bbf-yang:performance-counter64
       |  |  +--ro burst-profile?           bbf-yang:performance-counter64
       |  |  +--ro assign-onu-id?           bbf-yang:performance-counter64
       |  |  +--ro ranging-time?            bbf-yang:performance-counter64
       |  |  +--ro protect-control?         bbf-yang:performance-counter64
       |  |  +--ro adjust-tx-wavelength
       |  |  |  +--ro total?                  bbf-yang:performance-counter64
       |  |  |  +--ro wavelength-dithering?   bbf-yang:performance-counter64
       |  |  |  +--ro adjustment-amplitude?   bbf-yang:performance-counter64
       |  |  |  +--ro unsatisfied-requests?   bbf-yang:performance-counter64
       |  |  +--ro deactivate-onu-id?       bbf-yang:performance-counter64
       |  |  +--ro disable-serial-number?   bbf-yang:performance-counter64
       |  |  +--ro request-registration?    bbf-yang:performance-counter64
       |  |  +--ro assign-alloc-id?         bbf-yang:performance-counter64
       |  |  +--ro key-control?             bbf-yang:performance-counter64
       |  |  +--ro sleep-allow?             bbf-yang:performance-counter64
       |  |  +--ro tuning-control
       |  |     +--ro request-operation?    bbf-yang:performance-counter64
       |  |     +--ro complete-operation?   bbf-yang:performance-counter64
       |  +--ro calibration-request?   bbf-yang:performance-counter64
       |  +--ro upstream-messages
       |     +--ro total?                      bbf-yang:performance-counter64
       |     +--ro serial-number-onu
       |     |  +--ro inband?   bbf-yang:performance-counter64
       |     |  +--ro amcc?     bbf-yang:performance-counter64
       |     +--ro registration?               bbf-yang:performance-counter64
       |     +--ro key-report?                 bbf-yang:performance-counter64
       |     +--ro acknowledgement?            bbf-yang:performance-counter64
       |     +--ro sleep-request?              bbf-yang:performance-counter64
       |     +--ro tuning-response
       |     |  +--ro ack-nack?            bbf-yang:performance-counter64
       |     |  +--ro complete-rollback?   bbf-yang:performance-counter64
       |     +--ro power-consumption-report?   bbf-yang:performance-counter64
       +--ro activation
       |  +--ro non-discernible-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro foreign-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-new-activations
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-precfg-twdm-channel-handovers
       |     +--ro Inband?   bbf-yang:performance-counter64
       |     +--ro amcc?      bbf-yang:performance-counter64
       +--ro tuning-control
       |  +--ro requests-for-rx-or-rx-and-tx?       bbf-yang:performance-counter64
       |  +--ro requests-for-tx?                    bbf-yang:performance-counter64
       |  +--ro rejected-requests
       |  |  +--ro internal-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro void?    bbf-yang:performance-counter64
       |  |  |  +--ro part?    bbf-yang:performance-counter64
       |  |  |  +--ro tunr?    bbf-yang:performance-counter64
       |  |  |  +--ro lnrt?    bbf-yang:performance-counter64
       |  |  |  +--ro lncd?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro clbr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro fulfilled-requests?                 bbf-yang:performance-counter64
       |  +--ro failed-requests
       |  |  +--ro target-ds-wl-channel-not-found?        bbf-yang:performance-counter64
       |  |  +--ro no-feedback-in-target-us-wl-channel?   bbf-yang:performance-counter64
       |  +--ro resolved-requests?                  bbf-yang:performance-counter64
       |  +--ro failed-requests-rollback
       |  |  +--ro communication-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro lktp?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro failed-requests-onu-reactivation?   bbf-yang:performance-counter64
       +--ro power-levelling
       |  +--ro change-power-level-messages
       |     +--ro rejected?                 bbf-yang:performance-counter64
       |     +--ro without-completion-ack?   bbf-yang:performance-counter64
       +--ro omci
       |  +--ro baseline-messages?    bbf-yang:performance-counter64
       |  +--ro extended-messages?    bbf-yang:performance-counter64
       |  +--ro autonomus-messages?   bbf-yang:performance-counter64
       |  +--ro mic-errors?           bbf-yang:performance-counter64
       +--ro power-monitoring
       |  +--ro tx-optical-power-level?   int32
       +--ro energy-conservation
          +--ro time-spent-in-low-energy-state?   bbf-yang:performance-counter64
  augment /if:interfaces-state/if:interface/bbf-if-pm:performance/bbf-if-pm:intervals-24hr/bbf-if-pm:history:
    +--ro xpon {bbf-if-pm:performance-24hr}?
       +--ro phy
       |  +--ro corrected-fec-bytes?           bbf-yang:performance-counter64
       |  +--ro corrected-fec-codewords?       bbf-yang:performance-counter64
       |  +--ro uncorrectable-fec-codewords?   bbf-yang:performance-counter64
       |  +--ro in-fec-codewords?              bbf-yang:performance-counter64
       |  +--ro fec-seconds?                   bbf-yang:performance-counter32
       |  +--ro in-bip-protected-words?        bbf-yang:performance-counter64
       |  +--ro in-bip-errors?                 bbf-yang:performance-counter64
       |  +--ro psbd-hec-errors?               bbf-yang:performance-counter64
       |  +--ro fs-hec-errors?                 bbf-yang:performance-counter64
       |  +--ro unknown-profiles?              bbf-yang:performance-counter64
       +--ro lods
       |  +--ro total-events?       bbf-yang:performance-counter64
       |  +--ro restored
       |  |  +--ro operating-twdm?        bbf-yang:performance-counter64
       |  |  +--ro precfg-protect-twdm?   bbf-yang:performance-counter64
       |  |  +--ro discretionary-twdm?    bbf-yang:performance-counter64
       |  +--ro onu-reactivation
       |     +--ro no-sync-reacquired?                    bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-precfg-twdm?          bbf-yang:performance-counter64
       |     +--ro after-us-hs-fail-discretionary-twdm?   bbf-yang:performance-counter64
       +--ro gemport
       |  +--ro out-frames?                  bbf-yang:performance-counter64
       |  +--ro in-frames?                   bbf-yang:performance-counter64
       |  +--ro out-frames-lf-bit-not-set?   bbf-yang:performance-counter64
       |  +--ro hec-errors?                  bbf-yang:performance-counter64
       |  +--ro fs-frame-words-lost?         bbf-yang:performance-counter64
       |  +--ro key-errors?                  bbf-yang:performance-counter64
       +--ro utilization
       |  +--ro out-bytes-non-idle-xgem-frames?   bbf-yang:performance-counter64
       |  +--ro in-bytes-non-idle-xgem-frames?    bbf-yang:performance-counter64
       |  +--ro bw-not-assigned?                  bbf-yang:performance-counter64
       +--ro ploam
       |  +--ro sn-grants?             bbf-yang:performance-counter64
       |  +--ro mic-errors?            bbf-yang:performance-counter64
       |  +--ro timeouts?              bbf-yang:performance-counter64
       |  +--ro dying-gasps?           bbf-yang:performance-counter64
       |  +--ro downstream-messages
       |  |  +--ro total?                   bbf-yang:performance-counter64
       |  |  +--ro system-profile?          bbf-yang:performance-counter64
       |  |  +--ro channel-profile?         bbf-yang:performance-counter64
       |  |  +--ro burst-profile?           bbf-yang:performance-counter64
       |  |  +--ro assign-onu-id?           bbf-yang:performance-counter64
       |  |  +--ro ranging-time?            bbf-yang:performance-counter64
       |  |  +--ro protect-control?         bbf-yang:performance-counter64
       |  |  +--ro adjust-tx-wavelength
       |  |  |  +--ro total?                  bbf-yang:performance-counter64
       |  |  |  +--ro wavelength-dithering?   bbf-yang:performance-counter64
       |  |  |  +--ro adjustment-amplitude?   bbf-yang:performance-counter64
       |  |  |  +--ro unsatisfied-requests?   bbf-yang:performance-counter64
       |  |  +--ro deactivate-onu-id?       bbf-yang:performance-counter64
       |  |  +--ro disable-serial-number?   bbf-yang:performance-counter64
       |  |  +--ro request-registration?    bbf-yang:performance-counter64
       |  |  +--ro assign-alloc-id?         bbf-yang:performance-counter64
       |  |  +--ro key-control?             bbf-yang:performance-counter64
       |  |  +--ro sleep-allow?             bbf-yang:performance-counter64
       |  |  +--ro tuning-control
       |  |     +--ro request-operation?    bbf-yang:performance-counter64
       |  |     +--ro complete-operation?   bbf-yang:performance-counter64
       |  +--ro calibration-request?   bbf-yang:performance-counter64
       |  +--ro upstream-messages
       |     +--ro total?                      bbf-yang:performance-counter64
       |     +--ro serial-number-onu
       |     |  +--ro inband?   bbf-yang:performance-counter64
       |     |  +--ro amcc?     bbf-yang:performance-counter64
       |     +--ro registration?               bbf-yang:performance-counter64
       |     +--ro key-report?                 bbf-yang:performance-counter64
       |     +--ro acknowledgement?            bbf-yang:performance-counter64
       |     +--ro sleep-request?              bbf-yang:performance-counter64
       |     +--ro tuning-response
       |     |  +--ro ack-nack?            bbf-yang:performance-counter64
       |     |  +--ro complete-rollback?   bbf-yang:performance-counter64
       |     +--ro power-consumption-report?   bbf-yang:performance-counter64
       +--ro activation
       |  +--ro non-discernible-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro foreign-activation-attempts
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-new-activations
       |  |  +--ro Inband?   bbf-yang:performance-counter64
       |  |  +--ro amcc?      bbf-yang:performance-counter64
       |  +--ro successful-precfg-twdm-channel-handovers
       |     +--ro Inband?   bbf-yang:performance-counter64
       |     +--ro amcc?      bbf-yang:performance-counter64
       +--ro tuning-control
       |  +--ro requests-for-rx-or-rx-and-tx?       bbf-yang:performance-counter64
       |  +--ro requests-for-tx?                    bbf-yang:performance-counter64
       |  +--ro rejected-requests
       |  |  +--ro internal-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro void?    bbf-yang:performance-counter64
       |  |  |  +--ro part?    bbf-yang:performance-counter64
       |  |  |  +--ro tunr?    bbf-yang:performance-counter64
       |  |  |  +--ro lnrt?    bbf-yang:performance-counter64
       |  |  |  +--ro lncd?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro clbr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro fulfilled-requests?                 bbf-yang:performance-counter64
       |  +--ro failed-requests
       |  |  +--ro target-ds-wl-channel-not-found?        bbf-yang:performance-counter64
       |  |  +--ro no-feedback-in-target-us-wl-channel?   bbf-yang:performance-counter64
       |  +--ro resolved-requests?                  bbf-yang:performance-counter64
       |  +--ro failed-requests-rollback
       |  |  +--ro communication-condition?   bbf-yang:performance-counter64
       |  |  +--ro downstream
       |  |  |  +--ro total?   bbf-yang:performance-counter64
       |  |  |  +--ro albl?    bbf-yang:performance-counter64
       |  |  |  +--ro lktp?    bbf-yang:performance-counter64
       |  |  +--ro upstream
       |  |     +--ro total?   bbf-yang:performance-counter64
       |  |     +--ro albl?    bbf-yang:performance-counter64
       |  |     +--ro void?    bbf-yang:performance-counter64
       |  |     +--ro tunr?    bbf-yang:performance-counter64
       |  |     +--ro lktp?    bbf-yang:performance-counter64
       |  |     +--ro lnrt?    bbf-yang:performance-counter64
       |  |     +--ro lncd?    bbf-yang:performance-counter64
       |  +--ro failed-requests-onu-reactivation?   bbf-yang:performance-counter64
       +--ro power-levelling
       |  +--ro change-power-level-messages
       |     +--ro rejected?                 bbf-yang:performance-counter64
       |     +--ro without-completion-ack?   bbf-yang:performance-counter64
       +--ro omci
       |  +--ro baseline-messages?    bbf-yang:performance-counter64
       |  +--ro extended-messages?    bbf-yang:performance-counter64
       |  +--ro autonomus-messages?   bbf-yang:performance-counter64
       |  +--ro mic-errors?           bbf-yang:performance-counter64
       +--ro power-monitoring
       |  +--ro tx-optical-power-level?   int32
       +--ro energy-conservation
          +--ro time-spent-in-low-energy-state?   bbf-yang:performance-counter64
```

- Module Dependencies: `bbf-yang-types` `ietf-interfaces` `bbf-xpon-if-type` `bbf-interfaces-performance-management`

## 8.83. bbf-xponvani-base

```text
submodule: bbf-xponvani-base (belongs-to bbf-xponvani)

  augment /if:interfaces/if:interface:
    +--rw v-ani
  augment /if:interfaces/if:interface:
    +--rw olt-v-enet
  augment /if:interfaces-state/if:interface:
    +--ro v-ani
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type`

## 8.84. bbf-xponvani

```text
module: bbf-xponvani

  augment /if:interfaces/if:interface:
    +--rw v-ani
       +--rw onu-id?                             bbf-xpon-types:onu-id {bbf-xponvani:configurable-v-ani-onu-id}?
       +--rw channel-partition?                  if:interface-ref
       +--rw expected-serial-number?             bbf-xpon-types:onu-serial-number
       +--rw expected-registration-id?           bbf-xpon-types:onu-registration-id
       +--rw preferred-channel-pair?             if:interface-ref
       +--rw protection-channel-pair?            if:interface-ref
       +--rw upstream-channel-speed?             yang:gauge64
       +--rw upstream-fec?                       boolean
       +--rw management-gemport-id?              uint32 {bbf-xponvani:configurable-v-ani-management-gem-port-id}?
       +--rw management-gemport-aes-indicator?   boolean
       +---x in-service-onu-tune-request
       |  +---w input
       |  |  +---w target-channel-termination-ref?   if:interface-ref
       |  +--rw output
       |     +--rw onu-tune-request-status?          identityref
       |     +--rw onu-tune-request-reject-string?   string
       +--rw burst-profiles {bbf-xpon-burst-prof:configurable-burst-profile}?
       |  +--rw burst-profile* [profile-id]
       |     +--rw profile-id               uint8
       |     +--rw profile-version?         uint8
       |     +--rw delimiter?               string
       |     +--rw preamble?                string
       |     +--rw preamble-repeat-count?   uint8
       +--rw data-burst-profile-id?              uint8 {bbf-xpon-burst-prof:configurable-burst-profile}?
       +--rw notifiable-defect*                  identityref {bbf-xponvani:v-ani-defect-notifications}?
  augment /if:interfaces/if:interface:
    +--rw olt-v-enet
       +--rw lower-layer-interface    if:interface-ref
  augment /if:interfaces-state/if:interface:
    +--ro v-ani
       +--ro onu-id?                      bbf-xpon-types:onu-id
       +--ro management-tcont-alloc-id?   uint32
       +--ro management-gemport-id?       uint32
       +--ro onu-presence-state?          identityref
       +--ro active-defects*              identityref
       +--ro onu-present-on-this-olt!
       |  +--ro onu-present-on-this-channel-pair           if:interface-ref
       |  +--ro onu-present-on-this-channel-termination    if:interface-ref
       |  +--ro onu-fiber-distance?                        uint32
       |  +--ro detected-serial-number?                    bbf-xpon-types:onu-serial-number
       |  +--ro detected-registration-id?                  bbf-xpon-types:onu-registration-id
       +--ro onu-wl-protected!
       |  +--ro wl-protecting-ct-announced-to-onu?   if:interface-ref
       +---n defect-state-change {bbf-xponvani:v-ani-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `bbf-xponvani-base` `bbf-xponvani-v-ani-body` `bbf-xponvani-v-enet-body`

## 8.85. bbf-xponvani-power-management

```text
module: bbf-xponvani-power-management
  +--rw xponvani-power-management-profiles {bbf-xpon-pwr:xpon-power-management}?
     +--rw power-management-profile* [name]
        +--rw name          bbf-yang:string-ascii64
        +--rw ilowpower?    uint32
        +--rw iaware?       uint32
        +--rw itransinit?   uint32
        +--rw itxinit?      uint32
        +--rw irxoff?       uint32

  augment /if:interfaces/if:interface/bbf-xponvani:v-ani:
    +--rw power-management! {bbf-xpon-pwr:xpon-power-management}?
       +--rw power-management-profile-ref    -> /xponvani-power-management-profiles/power-management-profile/name

  notifications:
    +---n onu-power-state-change {bbf-xpon-pwr:xpon-power-management}?
       +--ro v-ani-ref                if:interface-ref
       +--ro onu-state-last-change    yang:date-and-time
       +--ro previous-state?          identityref
       +--ro current-state?           identityref
```

- Module Dependencies: `ietf-yang-types` `bbf-yang-types` `ietf-interfaces` `bbf-xponvani` `bbf-xpon-power-management`

## 8.86. bbf-xponvani-v-ani-body

```text
submodule: bbf-xponvani-v-ani-body (belongs-to bbf-xponvani)

  augment /if:interfaces/if:interface:
    +--rw v-ani
       +--rw onu-id?                             bbf-xpon-types:onu-id {bbf-xponvani:configurable-v-ani-onu-id}?
       +--rw channel-partition?                  if:interface-ref
       +--rw expected-serial-number?             bbf-xpon-types:onu-serial-number
       +--rw expected-registration-id?           bbf-xpon-types:onu-registration-id
       +--rw preferred-channel-pair?             if:interface-ref
       +--rw protection-channel-pair?            if:interface-ref
       +--rw upstream-channel-speed?             yang:gauge64
       +--rw upstream-fec?                       boolean
       +--rw management-gemport-id?              uint32 {bbf-xponvani:configurable-v-ani-management-gem-port-id}?
       +--rw management-gemport-aes-indicator?   boolean
       +---x in-service-onu-tune-request
       |  +---w input
       |  |  +---w target-channel-termination-ref?   if:interface-ref
       |  +--rw output
       |     +--rw onu-tune-request-status?          identityref
       |     +--rw onu-tune-request-reject-string?   string
       +--rw burst-profiles {bbf-xpon-burst-prof:configurable-burst-profile}?
       |  +--rw burst-profile* [profile-id]
       |     +--rw profile-id               uint8
       |     +--rw profile-version?         uint8
       |     +--rw delimiter?               string
       |     +--rw preamble?                string
       |     +--rw preamble-repeat-count?   uint8
       +--rw data-burst-profile-id?              uint8 {bbf-xpon-burst-prof:configurable-burst-profile}?
       +--rw notifiable-defect*                  identityref {bbf-xponvani:v-ani-defect-notifications}?
  augment /if:interfaces/if:interface:
    +--rw olt-v-enet
  augment /if:interfaces-state/if:interface:
    +--ro v-ani
       +--ro onu-id?                      bbf-xpon-types:onu-id
       +--ro management-tcont-alloc-id?   uint32
       +--ro management-gemport-id?       uint32
       +--ro onu-presence-state?          identityref
       +--ro active-defects*              identityref
       +--ro onu-present-on-this-olt!
       |  +--ro onu-present-on-this-channel-pair           if:interface-ref
       |  +--ro onu-present-on-this-channel-termination    if:interface-ref
       |  +--ro onu-fiber-distance?                        uint32
       |  +--ro detected-serial-number?                    bbf-xpon-types:onu-serial-number
       |  +--ro detected-registration-id?                  bbf-xpon-types:onu-registration-id
       +--ro onu-wl-protected!
       |  +--ro wl-protecting-ct-announced-to-onu?   if:interface-ref
       +---n defect-state-change {bbf-xponvani:v-ani-defect-notifications}?
          +--ro defect* [type]
          |  +--ro type     identityref
          |  +--ro state    bbf-xpon-def:defect-state
          +--ro last-change    yang:date-and-time
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `ietf-yang-types` `bbf-xpon` `bbf-xpon-types` `bbf-xpon-onu-types` `bbf-xpon-defects` `bbf-xpon-burst-profile` `bbf-xponvani-base`

## 8.87. bbf-xponvani-v-enet-body

```text
submodule: bbf-xponvani-v-enet-body (belongs-to bbf-xponvani)

  augment /if:interfaces/if:interface:
    +--rw v-ani
  augment /if:interfaces/if:interface:
    +--rw olt-v-enet
       +--rw lower-layer-interface    if:interface-ref
  augment /if:interfaces-state/if:interface:
    +--ro v-ani
```

- Module Dependencies: `ietf-interfaces` `bbf-xpon-if-type` `bbf-xponvani-base`

## 8.88. bbf-xpon-wavelength-profile-body

```text
submodule: bbf-xpon-wavelength-profile-body (belongs-to bbf-xpon)
  +--rw xpon
  |  +--rw ictp {ictp-support}?
  |  |  +--rw activated?                             boolean
  |  |  +--rw all-ictp-proxies-all-channel-groups
  |  |     +--rw proxy* [name]
  |  |        +--rw name                                              bbf-yang:string-ascii64
  |  |        +--rw host                                              inet:host
  |  |        +--rw tcp-port?                                         inet:port-number {configurable-ictp-proxy-tcp-port}?
  |  |        +--rw all-channel-terminations-proxied-by-this-proxy
  |  |           +--rw channel-termination* [channel-termination-ref]
  |  |              +--rw channel-termination-ref               if:interface-ref
  |  |              +--rw channel-termination-ictp-activated?   boolean
  |  +--rw wavelength-profiles
  |     +--rw wavelength-profile* [name]
  |        +--rw name                     bbf-yang:string-ascii64
  |        +--rw upstream-channel-id?     uint8
  |        +--rw downstream-channel-id?   uint8
  |        +--rw downstream-wavelength?   uint32
  +--ro xpon-state
     +--ro ictp {ictp-support}?
        +--ro all-ictp-proxies-all-channel-groups
           +--ro proxy* [name]
              +--ro name                       bbf-yang:string-ascii64
              +--ro proxy-ip-address?          bbf-xpon-types:ip-address-or-unresolved
              +--ro negotiated-ictp-version?   union
              +--ro supported-ictp-version*    uint8
              +--ro known-peered-proxies
                 +--ro proxy* [name]
                    +--ro name                    bbf-yang:string-ascii64
                    +--ro ip-address?             bbf-xpon-types:ip-address-or-unresolved
                    +--ro tcp-connection-state?   identityref
                    +--ro source-tcp-port?        inet:port-number
                    +--ro destination-tcp-port?   inet:port-number

  augment /if:interfaces/if:interface:
    +--rw channel-group
  augment /if:interfaces/if:interface:
    +--rw channel-partition
  augment /if:interfaces/if:interface:
    +--rw channel-pair
  augment /if:interfaces/if:interface:
    +--rw channel-termination
  augment /if:interfaces-state/if:interface:
    +--ro channel-group
  augment /if:interfaces-state/if:interface:
    +--ro channel-pair
  augment /if:interfaces-state/if:interface:
    +--ro channel-termination
```

- Module Dependencies: `bbf-yang-types` `bbf-xpon-base`

## 8.89. iana-ssh-encryption-algs

```text
module: iana-ssh-encryption-algs
  +--ro supported-algorithms
     +--ro supported-algorithm*   identityref
```

- Module Dependencies: `N/A`

## 8.90. iana-ssh-key-exchange-algs

```text
module: iana-ssh-key-exchange-algs
  +--ro supported-algorithms
     +--ro supported-algorithm*   identityref
```

- Module Dependencies: `N/A`

## 8.91. iana-ssh-mac-algs

```text
module: iana-ssh-mac-algs
  +--ro supported-algorithms
     +--ro supported-algorithm*   identityref
```

- Module Dependencies: `N/A`

## 8.92. iana-ssh-public-key-algs

```text
module: iana-ssh-public-key-algs
  +--ro supported-algorithms
     +--ro supported-algorithm*   identityref
```

- Module Dependencies: `N/A`

## 8.93. iana-symmetric-algs

```text
module: iana-symmetric-algs
  +--ro supported-symmetric-algorithms
     +--ro supported-symmetric-algorithm* [algorithm]
        +--ro algorithm    symmetric-algorithm-type
```

- Module Dependencies: `N/A`

## 8.94. iana-tls-cipher-suite-algs

```text
module: iana-tls-cipher-suite-algs
  +--ro supported-algorithms
     +--ro supported-algorithm*   identityref
```

- Module Dependencies: `N/A`

## 8.95. ieee802-dot1ax

```text
module: ieee802-dot1ax
  +--rw lag-system
     +--rw aggregating-system* [agg-system]
        +--rw agg-system         string
        +--rw system-id?         ieee:mac-address
        +--rw system-priority?   uint32

  augment /if:interfaces/if:interface:
    +--rw aggregator
       +--rw name?                        string
       +--rw agg-system-name?             string
       +--rw admin-state?                 enumeration
       +--rw link-up-down-notification?   enumeration
       +--rw collector-max-delay?         int16
       +--rw aggregator-lacp
          +--rw actor-admin-key?   int16
  augment /if:interfaces-state/if:interface:
    +--ro aggregator
       +--ro id?                         uint32
       +--ro description?                string
       +--ro aggregate-or-individual?    boolean
       +--ro collector-max-delay?        int16
       +--ro oper-state?                 enumeration
       +--ro time-of-last-oper-change?   yang:counter32
       +--ro data-rate?                  uint64
       +--ro aggregator-lacp
       |  +--ro actor-oper-key?            int16
       |  +--ro agg-mac-address?           ieee:mac-address
       |  +--ro partner-system-id?         ieee:mac-address
       |  +--ro partner-system-priority?   uint16
       |  +--ro partner-oper-key?          int16
       +--ro statistics
          +--ro octets-tx?                 yang:counter64
          +--ro octets-rx?                 yang:counter64
          +--ro frames-tx?                 yang:counter64
          +--ro frames-rx?                 yang:counter64
          +--ro multicast-frames-tx?       yang:counter64
          +--ro multicast-frames-rx?       yang:counter64
          +--ro broadcast-frames-tx?       yang:counter64
          +--ro broadcast-frames-rx?       yang:counter64
          +--ro frames-discarded-on-tx?    yang:counter64
          +--ro frames-discarded-on-rx?    yang:counter64
          +--ro frames-with-tx-errors?     yang:counter64
          +--ro frames-with-rx-errors?     yang:counter64
          +--ro unknown-protocol-frames?   yang:counter64
  augment /if:interfaces/if:interface:
    +--rw aggregation-port
       +--rw aggregation-port-lacp
          +--rw actor-system-priority?           int16
          +--rw actor-admin-key?                 int16
          +--rw partner-admin-system-priority?   int16
          +--rw partner-admin-system-id?         ieee:mac-address
          +--rw partner-admin-key?               int16
          +--rw actor-port-priority?             int16
          +--rw partner-admin-port?              int16
          +--rw partner-admin-port-priority?     int16
          +--rw actor-admin-state?               bits
          +--rw partner-admin-state?             bits
  augment /if:interfaces-state/if:interface:
    +--ro aggregation-port
       +--ro id?                        int16
       +--ro aggregate-or-individual?   boolean
       +--ro aggregation-port-lacp
          +--ro actor-system-id?                ieee:mac-address
          +--ro actor-oper-key?                 int16
          +--ro partner-oper-system-priority?   int16
          +--ro partner-oper-system-id?         ieee:mac-address
          +--ro partner-oper-key?               int16
          +--ro selected-agg-id?                int16
          +--ro attached-agg-id?                int16
          +--ro actor-port?                     int16
          +--ro partner-oper-port?              int16
          +--ro partner-oper-port-priority?     int16
          +--ro actor-oper-state?               bits
          +--ro partner-oper-state?             bits
          +--ro aggregation-port-stats
             +--ro stats-id?                 int16
             +--ro lacp-pdu-rx?              yang:counter64
             +--ro marker-pdu-rx?            yang:counter64
             +--ro marker-response-pdu-rx?   yang:counter64
             +--ro unknown-rx?               yang:counter64
             +--ro illegal-rx?               yang:counter64
             +--ro lacp-pdu-tx?              yang:counter64
             +--ro marker-pdu-tx?            yang:counter64
             +--ro marker-response-pdu-tx?   yang:counter64
```

- Module Dependencies: `ieee802-types` `ietf-yang-types` `ietf-interfaces` `iana-if-type`

## 8.96. ieee802-dot1q-cfm

```text
module: ieee802-dot1q-cfm
  +--rw cfm
     +--rw maintenance-domain* [md-id]
     |  +--rw md-id                              cfm-types:name-key-type
     |  +--rw (md-name)?
     |  |  +--:(none)
     |  |  |  +--rw none?                        empty
     |  |  +--:(dns-like-name)
     |  |  |  +--rw dns-like-name?               string
     |  |  +--:(mac-address-and-uint)
     |  |  |  +--rw mac-address-and-uint-type
     |  |  |     +--rw address    ieee:mac-address
     |  |  |     +--rw int        uint16
     |  |  +--:(char-string)
     |  |     +--rw char-string?                 string
     |  +--rw md-level?                          cfm-types:md-level-type
     |  +--rw mhf-creation?                      cfm-types:mhf-creation-type
     |  +--rw id-permission?                     cfm-types:sender-id-permission-type
     |  +--rw fault-alarm-transmission?          cfm-types:fault-alarm-type
     |  +--rw maintenance-association* [ma-id]
     |     +--rw ma-id                          cfm-types:name-key-type
     |     +--rw (ma-name)
     |     |  +--:(primary-vid)
     |     |  |  +--rw primary-vid?             dot1q-types:vlanid
     |     |  +--:(char-string)
     |     |  |  +--rw char-string?             string
     |     |  +--:(unsigned-int16)
     |     |  |  +--rw unsigned-int16?          uint16
     |     |  +--:(rfc2865-vpn-id)
     |     |     +--rw vpn-id
     |     |        +--rw vpn-oui      uint32
     |     |        +--rw vpn-index    uint32
     |     +--rw ccm-interval?                  cfm-types:ccm-interval-type
     |     +--rw fault-alarm-transmission?      cfm-types:fault-alarm-type
     |     +--rw mhf-creation?                  cfm-types:mhf-creation-type
     |     +--rw id-permission?                 cfm-types:sender-id-permission-type
     |     +--rw maintenance-association-mep* [mep-id]
     |        +--rw mep-id    cfm-types:mep-id-type
     +--rw maintenance-group* [maintenance-group-id]
        +--rw maintenance-group-id    cfm-types:name-key-type
        +--rw md-id                   -> /cfm/maintenance-domain/md-id
        +--rw ma-id                   -> /cfm/maintenance-domain[md-id = current()/../md-id]/maintenance-association/ma-id
        +--rw mep* [mep-id]
           +--rw mep-id                 -> /cfm/maintenance-domain[md-id = current()/../../md-id]/maintenance-association[ma-id = current()/../../ma-id]/maintenance-association-mep/mep-id
           +--rw direction              cfm-types:mp-direction-type
           +--rw enabled?               boolean
           +--rw ccm-ltm-priority?      dot1q-types:priority-type
           +--ro mac-address            ieee:mac-address
           +--rw inactive-remote-mep* [inactive-rmep-id]
           |  +--rw inactive-rmep-id    -> /cfm/maintenance-domain[md-id = current()/../../../md-id]/maintenance-association[ma-id = current()/../../../ma-id]/maintenance-association-mep/mep-id
           +--ro mep-db* [rmep-id]
           |  +--ro rmep-id                     cfm-types:mep-id-type
           |  +--ro rmep-state                  cfm-types:remote-mep-state-type
           |  +--ro rmep-failed-ok-time         yang:timeticks
           |  +--ro mac-address                 ieee:mac-address
           |  +--ro rdi                         boolean
           |  +--ro port-status-tlv?            cfm-types:port-status-tlv-value-type
           |  +--ro interface-status-tlv?       cfm-types:interface-status-tlv-value-type
           |  +--ro chassis-id-subtype?         ieee:chassis-id-subtype-type
           |  +--ro chassis-id?                 ieee:chassis-id-type
           |  +--ro transport-service-domain
           |  |  +--ro domain?                  yang:object-identifier-128
           |  |  +--ro (management-address)
           |  |     +--:(ip)
           |  |     |  +--ro ip-address         inet:ip-address
           |  |     |  +--ro ip-port            inet:port-number
           |  |     +--:(local)
           |  |     |  +--ro local-address      string
           |  |     +--:(dns)
           |  |     |  +--ro dns-address        string
           |  |     +--:(other)
           |  |        +--ro unknown-address?   binary
           |  +--ro rmep-is-active?             boolean
           +--rw continuity-check
           |  +--rw ccm-enabled?                boolean
           |  +--ro fng-state?                  cfm-types:fng-state-type
           |  +--rw fault-alarm-transmission?   cfm-types:fault-alarm-type
           |  +--rw lowest-priority-defect?     cfm-types:lowest-alarm-priority-type
           |  +--rw fng-alarm-time?             uint16
           |  +--rw fng-reset-time?             uint16
           |  +--ro highest-priority-defect     cfm-types:highest-defect-priority-type
           |  +--ro defects                     cfm-types:mep-defects-type
           |  +--ro error-ccm-last-failure?     binary
           |  +--ro xcon-ccm-last-failure?      binary
           +--ro stats
           |  +--ro mep-ccm-sequence-errors    yang:counter64
           |  +--ro mep-ccms-sent              yang:counter64
           |  +--ro mep-lbr-in                 yang:counter64
           |  +--ro mep-lbr-in-out-of-order    yang:counter64
           |  +--ro mep-lbr-bad-msdu           yang:counter64
           |  +--ro mep-unexpected-ltr-in      yang:counter64
           |  +--ro mep-lbr-out                yang:counter64
           +---x transmit-loopback
           |  +---w input
           |  |  +---w (lbm-destination)
           |  |  |  +--:(dest-ucast-mac-address)
           |  |  |  |  +---w lbm-dest-ucast-mac-address?          cfm-types:unicast-mac-address-type
           |  |  |  +--:(dest-mcast-class1-mac-address)
           |  |  |  |  +---w lbm-dest-mcast-class1-mac-address?   cfm-types:multicast-class1-mac-address-type
           |  |  |  +--:(dest-mep-id)
           |  |  |     +---w lbm-dest-mep-id?                     cfm-types:mep-id-type
           |  |  +---w lbm-messages?                              uint16
           |  |  +---w lbm-priority?                              dot1q-types:priority-type
           |  |  +---w lbm-drop-eligible?                         boolean
           |  |  +---w lbm-data-tlv?                              cfm-types:lbm-data-tlv-type
           |  +--ro output
           |     +--ro lbm-request-id    cfm-types:seq-number-type
           +---x transmit-linktrace
           |  +---w input
           |  |  +---w (ltr-target)
           |  |  |  +--:(target-ucast-mac-address)
           |  |  |  |  +---w ltm-target-mac-address?   cfm-types:unicast-mac-address-type
           |  |  |  +--:(target-mep-id)
           |  |  |     +---w ltm-target-mep-id?        cfm-types:mep-id-type
           |  |  +---w ltm-ttl?                        uint8
           |  |  +---w ltm-flags?                      cfm-types:mep-tx-ltm-flags-type
           |  +--ro output
           |     +--ro ltm-transaction-id       cfm-types:seq-number-type
           |     +--ro ltm-egress-identifier
           |        +--ro int        uint16
           |        +--ro address    ieee:mac-address
           +--ro linktrace-reply* [ltr-transaction-id]
              +--ro ltr-transaction-id    cfm-types:seq-number-type
              +--ro linktrace-input
              |  +--ro (ltr-target)
              |  |  +--:(target-ucast-mac-address)
              |  |  |  +--ro ltm-target-mac-address?   cfm-types:unicast-mac-address-type
              |  |  +--:(target-mep-id)
              |  |     +--ro ltm-target-mep-id?        cfm-types:mep-id-type
              |  +--ro ltm-ttl?                        uint8
              |  +--ro ltm-flags?                      cfm-types:mep-tx-ltm-flags-type
              +--ro responses* [ltr-receive-order]
                 +--ro ltr-receive-order                uint32
                 +--ro ltr-ttl                          uint8
                 +--ro ltr-forwarded                    boolean
                 +--ro ltr-terminal-mep                 boolean
                 +--ro ltr-last-egress-identifier
                 |  +--ro int        uint16
                 |  +--ro address    ieee:mac-address
                 +--ro ltr-next-egress-identifier
                 |  +--ro int        uint16
                 |  +--ro address    ieee:mac-address
                 +--ro ltr-relay                        cfm-types:relay-action-field-value-type
                 +--ro ltr-chassis-id-subtype?          ieee:chassis-id-subtype-type
                 +--ro ltr-chassis-id                   ieee:chassis-id-type
                 +--ro ltr-transport-service-domain
                 |  +--ro domain?                  yang:object-identifier-128
                 |  +--ro (management-address)
                 |     +--:(ip)
                 |     |  +--ro ip-address         inet:ip-address
                 |     |  +--ro ip-port            inet:port-number
                 |     +--:(local)
                 |     |  +--ro local-address      string
                 |     +--:(dns)
                 |     |  +--ro dns-address        string
                 |     +--:(other)
                 |        +--ro unknown-address?   binary
                 +--ro ltr-ingress?                     cfm-types:ingress-action-field-value-type
                 +--ro ltr-ingress-mac                  ieee:mac-address
                 +--ro ltr-ingress-port-id-subtype?     ieee:port-id-subtype-type
                 +--ro ltr-ingress-port-id              ieee:port-id-type
                 +--ro ltr-egress?                      cfm-types:egress-action-field-value-type
                 +--ro ltr-egress-mac                   ieee:mac-address
                 +--ro ltr-egress-port-id-subtype?      ieee:port-id-subtype-type
                 +--ro ltr-egress-port-id               ieee:port-id-type
                 +--ro ltr-organization-specific-tlv?   binary
```

- Module Dependencies: `ieee802-dot1q-cfm-types` `ieee802-dot1q-types` `ieee802-types` `ietf-yang-types` `ietf-inet-types`

## 8.97. ieee802-ethernet-interface

```text
module: ieee802-ethernet-interface

  augment /if:interfaces/if:interface:
    +--rw ethernet
       +--rw auto-negotiation!
       |  +--rw enable?               boolean
       |  +--ro negotiation-status?   enumeration
       +--rw duplex?                          duplex-type
       +--rw speed?                           eth-if-speed-type
       +--rw flow-control
       |  +--rw pause {ethernet-pause}?
       |  |  +--rw direction?    pause-fc-direction-type
       |  |  +--ro statistics
       |  |     +--ro in-frames-pause?    yang:counter64
       |  |     +--ro out-frames-pause?   yang:counter64
       |  +--rw pfc {ethernet-pfc}?
       |  |  +--rw enable?       boolean
       |  |  +--ro statistics
       |  |     +--ro in-frames-pfc?    yang:counter64
       |  |     +--ro out-frames-pfc?   yang:counter64
       |  +--rw force-flow-control?   boolean
       +--ro max-frame-length?                uint16
       +--ro mac-control-extension-control?   boolean
       +--ro frame-limit-slow-protocol?       uint64
       +--ro capabilities
       |  +--ro auto-negotiation?   boolean
       +--ro statistics
          +--ro frame
          |  +--ro in-total-frames?                 yang:counter64
          |  +--ro in-total-octets?                 yang:counter64
          |  +--ro in-frames?                       yang:counter64
          |  +--ro in-multicast-frames?             yang:counter64
          |  +--ro in-broadcast-frames?             yang:counter64
          |  +--ro in-error-fcs-frames?             yang:counter64
          |  +--ro in-error-undersize-frames?       yang:counter64
          |  +--ro in-error-oversize-frames?        yang:counter64
          |  +--ro in-error-mac-internal-frames?    yang:counter64
          |  +--ro out-frames?                      yang:counter64
          |  +--ro out-multicast-frames?            yang:counter64
          |  +--ro out-broadcast-frames?            yang:counter64
          |  +--ro out-error-mac-internal-frames?   yang:counter64
          +--ro phy
          |  +--ro in-error-symbol?   yang:counter64
          |  +--ro lpi
          |     +--ro in-lpi-transitions?    yang:counter64
          |     +--ro in-lpi-time?           decimal64
          |     +--ro out-lpi-transitions?   yang:counter64
          |     +--ro out-lpi-time?          decimal64
          +--ro mac-control
             +--ro in-frames-mac-control-unknown?      yang:counter64
             +--ro in-frames-mac-control-extension?    yang:counter64
             +--ro out-frames-mac-control-extension?   yang:counter64
```

- Module Dependencies: `ietf-yang-types` `ietf-interfaces` `iana-if-type`

## 8.98. ietf-access-control-list

```text
module: ietf-access-control-list
  +--rw acls
     +--rw acl* [name]
     |  +--rw name    string
     |  +--rw type?   acl-type
     |  +--rw aces
     |     +--rw ace* [name]
     |        +--rw name          string
     |        +--rw matches
     |        |  +--rw (l2)?
     |        |  |  +--:(eth)
     |        |  |     +--rw eth {match-on-eth}?
     |        |  |        +--rw destination-mac-address?        yang:mac-address
     |        |  |        +--rw destination-mac-address-mask?   yang:mac-address
     |        |  |        +--rw source-mac-address?             yang:mac-address
     |        |  |        +--rw source-mac-address-mask?        yang:mac-address
     |        |  |        +--rw ethertype?                      eth:ethertype
     |        |  +--rw (l3)?
     |        |  |  +--:(ipv4)
     |        |  |  |  +--rw ipv4 {match-on-ipv4}?
     |        |  |  |     +--rw dscp?                             inet:dscp
     |        |  |  |     +--rw ecn?                              uint8
     |        |  |  |     +--rw length?                           uint16
     |        |  |  |     +--rw ttl?                              uint8
     |        |  |  |     +--rw protocol?                         uint8
     |        |  |  |     +--rw ihl?                              uint8
     |        |  |  |     +--rw flags?                            bits
     |        |  |  |     +--rw offset?                           uint16
     |        |  |  |     +--rw identification?                   uint16
     |        |  |  |     +--rw (destination-network)?
     |        |  |  |     |  +--:(destination-ipv4-network)
     |        |  |  |     |     +--rw destination-ipv4-network?   inet:ipv4-prefix
     |        |  |  |     +--rw (source-network)?
     |        |  |  |        +--:(source-ipv4-network)
     |        |  |  |           +--rw source-ipv4-network?        inet:ipv4-prefix
     |        |  |  +--:(ipv6)
     |        |  |     +--rw ipv6 {match-on-ipv6}?
     |        |  |        +--rw dscp?                             inet:dscp
     |        |  |        +--rw ecn?                              uint8
     |        |  |        +--rw length?                           uint16
     |        |  |        +--rw ttl?                              uint8
     |        |  |        +--rw protocol?                         uint8
     |        |  |        +--rw (destination-network)?
     |        |  |        |  +--:(destination-ipv6-network)
     |        |  |        |     +--rw destination-ipv6-network?   inet:ipv6-prefix
     |        |  |        +--rw (source-network)?
     |        |  |        |  +--:(source-ipv6-network)
     |        |  |        |     +--rw source-ipv6-network?        inet:ipv6-prefix
     |        |  |        +--rw flow-label?                       inet:ipv6-flow-label
     |        |  +--rw (l4)?
     |        |  |  +--:(tcp)
     |        |  |  |  +--rw tcp {match-on-tcp}?
     |        |  |  |     +--rw sequence-number?          uint32
     |        |  |  |     +--rw acknowledgement-number?   uint32
     |        |  |  |     +--rw data-offset?              uint8
     |        |  |  |     +--rw reserved?                 uint8
     |        |  |  |     +--rw flags?                    bits
     |        |  |  |     +--rw window-size?              uint16
     |        |  |  |     +--rw urgent-pointer?           uint16
     |        |  |  |     +--rw options?                  binary
     |        |  |  |     +--rw source-port
     |        |  |  |     |  +--rw (source-port)?
     |        |  |  |     |     +--:(range-or-operator)
     |        |  |  |     |        +--rw (port-range-or-operator)?
     |        |  |  |     |           +--:(range)
     |        |  |  |     |           |  +--rw lower-port    inet:port-number
     |        |  |  |     |           |  +--rw upper-port    inet:port-number
     |        |  |  |     |           +--:(operator)
     |        |  |  |     |              +--rw operator?     operator
     |        |  |  |     |              +--rw port          inet:port-number
     |        |  |  |     +--rw destination-port
     |        |  |  |        +--rw (destination-port)?
     |        |  |  |           +--:(range-or-operator)
     |        |  |  |              +--rw (port-range-or-operator)?
     |        |  |  |                 +--:(range)
     |        |  |  |                 |  +--rw lower-port    inet:port-number
     |        |  |  |                 |  +--rw upper-port    inet:port-number
     |        |  |  |                 +--:(operator)
     |        |  |  |                    +--rw operator?     operator
     |        |  |  |                    +--rw port          inet:port-number
     |        |  |  +--:(udp)
     |        |  |  |  +--rw udp {match-on-udp}?
     |        |  |  |     +--rw length?             uint16
     |        |  |  |     +--rw source-port
     |        |  |  |     |  +--rw (source-port)?
     |        |  |  |     |     +--:(range-or-operator)
     |        |  |  |     |        +--rw (port-range-or-operator)?
     |        |  |  |     |           +--:(range)
     |        |  |  |     |           |  +--rw lower-port    inet:port-number
     |        |  |  |     |           |  +--rw upper-port    inet:port-number
     |        |  |  |     |           +--:(operator)
     |        |  |  |     |              +--rw operator?     operator
     |        |  |  |     |              +--rw port          inet:port-number
     |        |  |  |     +--rw destination-port
     |        |  |  |        +--rw (destination-port)?
     |        |  |  |           +--:(range-or-operator)
     |        |  |  |              +--rw (port-range-or-operator)?
     |        |  |  |                 +--:(range)
     |        |  |  |                 |  +--rw lower-port    inet:port-number
     |        |  |  |                 |  +--rw upper-port    inet:port-number
     |        |  |  |                 +--:(operator)
     |        |  |  |                    +--rw operator?     operator
     |        |  |  |                    +--rw port          inet:port-number
     |        |  |  +--:(icmp)
     |        |  |     +--rw icmp {match-on-icmp}?
     |        |  |        +--rw type?             uint8
     |        |  |        +--rw code?             uint8
     |        |  |        +--rw rest-of-header?   binary
     |        |  +--rw egress-interface?    if:interface-ref
     |        |  +--rw ingress-interface?   if:interface-ref
     |        +--rw actions
     |        |  +--rw forwarding    identityref
     |        |  +--rw logging?      identityref
     |        +--ro statistics {acl-aggregate-stats}?
     |           +--ro matched-packets?   yang:counter64
     |           +--ro matched-octets?    yang:counter64
     +--rw attachment-points
        +--rw interface* [interface-id] {interface-attachment}?
           +--rw interface-id    if:interface-ref
           +--rw ingress
           |  +--rw acl-sets
           |     +--rw acl-set* [name]
           |        +--rw name              -> /acls/acl/name
           |        +--ro ace-statistics* [name] {interface-stats}?
           |           +--ro name               -> /acls/acl/aces/ace/name
           |           +--ro matched-packets?   yang:counter64
           |           +--ro matched-octets?    yang:counter64
           +--rw egress
              +--rw acl-sets
                 +--rw acl-set* [name]
                    +--rw name              -> /acls/acl/name
                    +--ro ace-statistics* [name] {interface-stats}?
                       +--ro name               -> /acls/acl/aces/ace/name
                       +--ro matched-packets?   yang:counter64
                       +--ro matched-octets?    yang:counter64
```

- Module Dependencies: `ietf-yang-types` `ietf-packet-fields` `ietf-interfaces`

## 8.99. ietf-alarms

```text
module: ietf-alarms
  +--rw alarms
     +--rw control
     |  +--rw max-alarm-status-changes?   union
     |  +--rw notify-status-changes?      enumeration
     |  +--rw notify-severity-level?      severity
     |  +--rw alarm-shelving {alarm-shelving}?
     |     +--rw shelf* [name]
     |        +--rw name           string
     |        +--rw resource*      resource-match
     |        +--rw alarm-type* [alarm-type-id alarm-type-qualifier-match]
     |        |  +--rw alarm-type-id                 alarm-type-id
     |        |  +--rw alarm-type-qualifier-match    string
     |        +--rw description?   string
     +--ro alarm-inventory
     |  +--ro alarm-type* [alarm-type-id alarm-type-qualifier]
     |     +--ro alarm-type-id           alarm-type-id
     |     +--ro alarm-type-qualifier    alarm-type-qualifier
     |     +--ro resource*               resource-match
     |     +--ro will-clear              boolean
     |     +--ro severity-level*         severity
     |     +--ro description             string
     +--ro summary {alarm-summary}?
     |  +--ro alarm-summary* [severity]
     |  |  +--ro severity                  severity
     |  |  +--ro total?                    yang:gauge32
     |  |  +--ro not-cleared?              yang:gauge32
     |  |  +--ro cleared?                  yang:gauge32
     |  |  +--ro cleared-not-closed?       yang:gauge32 {operator-actions}?
     |  |  +--ro cleared-closed?           yang:gauge32 {operator-actions}?
     |  |  +--ro not-cleared-closed?       yang:gauge32 {operator-actions}?
     |  |  +--ro not-cleared-not-closed?   yang:gauge32 {operator-actions}?
     |  +--ro shelves-active?   empty {alarm-shelving}?
     +--ro alarm-list
     |  +--ro number-of-alarms?   yang:gauge32
     |  +--ro last-changed?       yang:date-and-time
     |  +--ro alarm* [resource alarm-type-id alarm-type-qualifier]
     |  |  +--ro resource                 resource
     |  |  +--ro alarm-type-id            alarm-type-id
     |  |  +--ro alarm-type-qualifier     alarm-type-qualifier
     |  |  +--ro alt-resource*            resource
     |  |  +--ro related-alarm* [resource alarm-type-id alarm-type-qualifier] {alarm-correlation}?
     |  |  |  +--ro resource                -> /alarms/alarm-list/alarm/resource
     |  |  |  +--ro alarm-type-id           -> /alarms/alarm-list/alarm[resource=current()/../resource]/alarm-type-id
     |  |  |  +--ro alarm-type-qualifier    -> /alarms/alarm-list/alarm[resource=current()/../resource][alarm-type-id=current()/../alarm-type-id]/alarm-type-qualifier
     |  |  +--ro impacted-resource*       resource {service-impact-analysis}?
     |  |  +--ro root-cause-resource*     resource {root-cause-analysis}?
     |  |  +--ro time-created             yang:date-and-time
     |  |  +--ro is-cleared               boolean
     |  |  +--ro last-raised              yang:date-and-time
     |  |  +--ro last-changed             yang:date-and-time
     |  |  +--ro perceived-severity       severity
     |  |  +--ro alarm-text               alarm-text
     |  |  +--ro status-change* [time] {alarm-history}?
     |  |  |  +--ro time                  yang:date-and-time
     |  |  |  +--ro perceived-severity    severity-with-clear
     |  |  |  +--ro alarm-text            alarm-text
     |  |  +--ro operator-state-change* [time] {operator-actions}?
     |  |  |  +--ro time        yang:date-and-time
     |  |  |  +--ro operator    string
     |  |  |  +--ro state       operator-state
     |  |  |  +--ro text?       string
     |  |  +---x set-operator-state {operator-actions}?
     |  |  |  +---w input
     |  |  |     +---w state    writable-operator-state
     |  |  |     +---w text?    string
     |  |  +---n operator-action {operator-actions}?
     |  |     +-- time        yang:date-and-time
     |  |     +-- operator    string
     |  |     +-- state       operator-state
     |  |     +-- text?       string
     |  +---x purge-alarms
     |  |  +---w input
     |  |  |  +---w alarm-clearance-status    enumeration
     |  |  |  +---w older-than!
     |  |  |  |  +---w (age-spec)?
     |  |  |  |     +--:(seconds)
     |  |  |  |     |  +---w seconds?   uint16
     |  |  |  |     +--:(minutes)
     |  |  |  |     |  +---w minutes?   uint16
     |  |  |  |     +--:(hours)
     |  |  |  |     |  +---w hours?     uint16
     |  |  |  |     +--:(days)
     |  |  |  |     |  +---w days?      uint16
     |  |  |  |     +--:(weeks)
     |  |  |  |        +---w weeks?     uint16
     |  |  |  +---w severity!
     |  |  |  |  +---w (sev-spec)?
     |  |  |  |     +--:(below)
     |  |  |  |     |  +---w below?   severity
     |  |  |  |     +--:(is)
     |  |  |  |     |  +---w is?      severity
     |  |  |  |     +--:(above)
     |  |  |  |        +---w above?   severity
     |  |  |  +---w operator-state-filter! {operator-actions}?
     |  |  |     +---w state?   operator-state
     |  |  |     +---w user?    string
     |  |  +--ro output
     |  |     +--ro purged-alarms?   uint32
     |  +---x compress-alarms {alarm-history}?
     |     +---w input
     |     |  +---w resource?               resource-match
     |     |  +---w alarm-type-id?          -> /alarms/alarm-list/alarm/alarm-type-id
     |     |  +---w alarm-type-qualifier?   -> /alarms/alarm-list/alarm/alarm-type-qualifier
     |     +--ro output
     |        +--ro compressed-alarms?   uint32
     +--ro shelved-alarms {alarm-shelving}?
     |  +--ro number-of-shelved-alarms?      yang:gauge32
     |  +--ro shelved-alarms-last-changed?   yang:date-and-time
     |  +--ro shelved-alarm* [resource alarm-type-id alarm-type-qualifier]
     |  |  +--ro resource                 resource
     |  |  +--ro alarm-type-id            alarm-type-id
     |  |  +--ro alarm-type-qualifier     alarm-type-qualifier
     |  |  +--ro alt-resource*            resource
     |  |  +--ro related-alarm* [resource alarm-type-id alarm-type-qualifier] {alarm-correlation}?
     |  |  |  +--ro resource                -> /alarms/alarm-list/alarm/resource
     |  |  |  +--ro alarm-type-id           -> /alarms/alarm-list/alarm[resource=current()/../resource]/alarm-type-id
     |  |  |  +--ro alarm-type-qualifier    -> /alarms/alarm-list/alarm[resource=current()/../resource][alarm-type-id=current()/../alarm-type-id]/alarm-type-qualifier
     |  |  +--ro impacted-resource*       resource {service-impact-analysis}?
     |  |  +--ro root-cause-resource*     resource {root-cause-analysis}?
     |  |  +--ro shelf-name?              -> /alarms/control/alarm-shelving/shelf/name
     |  |  +--ro is-cleared               boolean
     |  |  +--ro last-raised              yang:date-and-time
     |  |  +--ro last-changed             yang:date-and-time
     |  |  +--ro perceived-severity       severity
     |  |  +--ro alarm-text               alarm-text
     |  |  +--ro status-change* [time] {alarm-history}?
     |  |  |  +--ro time                  yang:date-and-time
     |  |  |  +--ro perceived-severity    severity-with-clear
     |  |  |  +--ro alarm-text            alarm-text
     |  |  +--ro operator-state-change* [time] {operator-actions}?
     |  |     +--ro time        yang:date-and-time
     |  |     +--ro operator    string
     |  |     +--ro state       operator-state
     |  |     +--ro text?       string
     |  +---x purge-shelved-alarms
     |  |  +---w input
     |  |  |  +---w alarm-clearance-status    enumeration
     |  |  |  +---w older-than!
     |  |  |  |  +---w (age-spec)?
     |  |  |  |     +--:(seconds)
     |  |  |  |     |  +---w seconds?   uint16
     |  |  |  |     +--:(minutes)
     |  |  |  |     |  +---w minutes?   uint16
     |  |  |  |     +--:(hours)
     |  |  |  |     |  +---w hours?     uint16
     |  |  |  |     +--:(days)
     |  |  |  |     |  +---w days?      uint16
     |  |  |  |     +--:(weeks)
     |  |  |  |        +---w weeks?     uint16
     |  |  |  +---w severity!
     |  |  |  |  +---w (sev-spec)?
     |  |  |  |     +--:(below)
     |  |  |  |     |  +---w below?   severity
     |  |  |  |     +--:(is)
     |  |  |  |     |  +---w is?      severity
     |  |  |  |     +--:(above)
     |  |  |  |        +---w above?   severity
     |  |  |  +---w operator-state-filter! {operator-actions}?
     |  |  |     +---w state?   operator-state
     |  |  |     +---w user?    string
     |  |  +--ro output
     |  |     +--ro purged-alarms?   uint32
     |  +---x compress-shelved-alarms {alarm-history}?
     |     +---w input
     |     |  +---w resource?               -> /alarms/shelved-alarms/shelved-alarm/resource
     |     |  +---w alarm-type-id?          -> /alarms/shelved-alarms/shelved-alarm/alarm-type-id
     |     |  +---w alarm-type-qualifier?   -> /alarms/shelved-alarms/shelved-alarm/alarm-type-qualifier
     |     +--ro output
     |        +--ro compressed-alarms?   uint32
     +--rw alarm-profile* [alarm-type-id alarm-type-qualifier-match resource] {alarm-profile}?
        +--rw alarm-type-id                        alarm-type-id
        +--rw alarm-type-qualifier-match           string
        +--rw resource                             resource-match
        +--rw description                          string
        +--rw alarm-severity-assignment-profile {severity-assignment}?
           +--rw severity-level*   severity

  notifications:
    +---n alarm-notification
    |  +--ro resource                resource
    |  +--ro alarm-type-id           alarm-type-id
    |  +--ro alarm-type-qualifier?   alarm-type-qualifier
    |  +--ro alt-resource*           resource
    |  +--ro related-alarm* [resource alarm-type-id alarm-type-qualifier] {alarm-correlation}?
    |  |  +--ro resource                -> /alarms/alarm-list/alarm/resource
    |  |  +--ro alarm-type-id           -> /alarms/alarm-list/alarm[resource=current()/../resource]/alarm-type-id
    |  |  +--ro alarm-type-qualifier    -> /alarms/alarm-list/alarm[resource=current()/../resource][alarm-type-id=current()/../alarm-type-id]/alarm-type-qualifier
    |  +--ro impacted-resource*      resource {service-impact-analysis}?
    |  +--ro root-cause-resource*    resource {root-cause-analysis}?
    |  +--ro time                    yang:date-and-time
    |  +--ro perceived-severity      severity-with-clear
    |  +--ro alarm-text              alarm-text
    +---n alarm-inventory-changed
```

- Module Dependencies: `ietf-yang-types`

## 8.100. ietf-alarms-x733

```text
module: ietf-alarms-x733

  augment /al:alarms/al:alarm-inventory/al:alarm-type:
    +--ro event-type?              event-type
    +--ro probable-cause?          uint32
    +--ro probable-cause-string?   string
  augment /al:alarms/al:control:
    +--rw x733-mapping* [alarm-type-id alarm-type-qualifier-match] {configure-x733-mapping}?
       +--rw alarm-type-id                 al:alarm-type-id
       +--rw alarm-type-qualifier-match    string
       +--rw event-type?                   event-type
       +--rw probable-cause?               uint32
       +--rw probable-cause-string?        string
  augment /al:alarms/al:alarm-list/al:alarm:
    +--ro event-type?                event-type
    +--ro probable-cause?            uint32
    +--ro probable-cause-string?     string
    +--ro threshold-information
    |  +--ro triggered-threshold?   string
    |  +--ro observed-value?        value-type
    |  +--ro (threshold-level)?
    |  |  +--:(up)
    |  |  |  +--ro up-high?         value-type
    |  |  |  +--ro up-low?          value-type
    |  |  +--:(down)
    |  |     +--ro down-low?        value-type
    |  |     +--ro down-high?       value-type
    |  +--ro arm-time?              yang:date-and-time
    +--ro monitored-attributes* [id]
    |  +--ro id       al:resource
    |  +--ro value?   string
    +--ro proposed-repair-actions*   string
    +--ro trend-indication?          trend
    +--ro backedup-status?           boolean
    +--ro backup-object?             al:resource
    +--ro additional-information* [identifier]
    |  +--ro identifier     string
    |  +--ro significant?   boolean
    |  +--ro information?   string
    +--ro security-alarm-detector?   al:resource
    +--ro service-user?              al:resource
    +--ro service-provider?          al:resource
  augment /al:alarms/al:shelved-alarms/al:shelved-alarm:
    +--ro event-type?                event-type
    +--ro probable-cause?            uint32
    +--ro probable-cause-string?     string
    +--ro threshold-information
    |  +--ro triggered-threshold?   string
    |  +--ro observed-value?        value-type
    |  +--ro (threshold-level)?
    |  |  +--:(up)
    |  |  |  +--ro up-high?         value-type
    |  |  |  +--ro up-low?          value-type
    |  |  +--:(down)
    |  |     +--ro down-low?        value-type
    |  |     +--ro down-high?       value-type
    |  +--ro arm-time?              yang:date-and-time
    +--ro monitored-attributes* [id]
    |  +--ro id       al:resource
    |  +--ro value?   string
    +--ro proposed-repair-actions*   string
    +--ro trend-indication?          trend
    +--ro backedup-status?           boolean
    +--ro backup-object?             al:resource
    +--ro additional-information* [identifier]
    |  +--ro identifier     string
    |  +--ro significant?   boolean
    |  +--ro information?   string
    +--ro security-alarm-detector?   al:resource
    +--ro service-user?              al:resource
    +--ro service-provider?          al:resource
  augment /al:alarm-notification:
    +--ro event-type?                event-type
    +--ro probable-cause?            uint32
    +--ro probable-cause-string?     string
    +--ro threshold-information
    |  +--ro triggered-threshold?   string
    |  +--ro observed-value?        value-type
    |  +--ro (threshold-level)?
    |  |  +--:(up)
    |  |  |  +--ro up-high?         value-type
    |  |  |  +--ro up-low?          value-type
    |  |  +--:(down)
    |  |     +--ro down-low?        value-type
    |  |     +--ro down-high?       value-type
    |  +--ro arm-time?              yang:date-and-time
    +--ro monitored-attributes* [id]
    |  +--ro id       al:resource
    |  +--ro value?   string
    +--ro proposed-repair-actions*   string
    +--ro trend-indication?          trend
    +--ro backedup-status?           boolean
    +--ro backup-object?             al:resource
    +--ro additional-information* [identifier]
    |  +--ro identifier     string
    |  +--ro significant?   boolean
    |  +--ro information?   string
    +--ro security-alarm-detector?   al:resource
    +--ro service-user?              al:resource
    +--ro service-provider?          al:resource
```

- Module Dependencies: `ietf-alarms` `ietf-yang-types`

## 8.101. ietf-dhcp

```text
module: ietf-dhcp
  +--rw dhcp
     +--rw server {server}?
     |  +--rw lease-time?            uint32
     |  +--rw ping-packet-number?    uint8
     |  +--rw ping-packet-timeout?   uint16
     |  +--rw option
     |  |  +--rw dhcp-server-identifier?   inet:ip-address
     |  |  +--rw domain-name?              string
     |  |  +--rw domain-name-server?       inet:ip-address
     |  |  +--rw interface-mtu?            uint32
     |  |  +--rw netbios-name-server?      inet:ip-address
     |  |  +--rw netbios-node-type?        uint32
     |  |  +--rw netbios-scope?            string
     |  +--rw ip-pool* [ip-pool-name]
     |  |  +--rw ip-pool-name         string
     |  |  +--rw interface?           if:interface-ref
     |  |  +--rw gateway-ip?          inet:ip-address
     |  |  +--rw gateway-mask?        inet:ip-prefix
     |  |  +--rw lease-time?          uint32
     |  |  +--rw manual-allocation* [mac-address ip-address]
     |  |  |  +--rw mac-address    yang:mac-address
     |  |  |  +--rw ip-address     inet:ip-address
     |  |  +--rw section* [start-ip]
     |  |  |  +--rw start-ip    inet:ipv4-address
     |  |  |  +--rw end-ip?     inet:ipv4-address
     |  |  +--rw option
     |  |  |  +--rw dhcp-server-identifier?   inet:ip-address
     |  |  |  +--rw domain-name?              string
     |  |  |  +--rw domain-name-server?       inet:ip-address
     |  |  |  +--rw interface-mtu?            uint32
     |  |  |  +--rw netbios-name-server?      inet:ip-address
     |  |  |  +--rw netbios-node-type?        uint32
     |  |  |  +--rw netbios-scope?            string
     |  |  +--ro used-ip-count?       uint32
     |  |  +--ro idle-ip-count?       uint32
     |  |  +--ro conflict-ip-count?   uint32
     |  |  +--ro total-ip-count?      uint32
     |  +--ro packet-statistics* [interface]
     |  |  +--ro interface           if:interface-ref
     |  |  +---x clean-statistics
     |  |  +--ro receive
     |  |  |  +--ro decline-packet?    uint32
     |  |  |  +--ro discover-packet?   uint32
     |  |  |  +--ro request-packet?    uint32
     |  |  |  +--ro release-packet?    uint32
     |  |  |  +--ro inform-packet?     uint32
     |  |  +--ro send
     |  |     +--ro offer-packet?   uint32
     |  |     +--ro ack-packet?     uint32
     |  |     +--ro nack-packet?    uint32
     |  +--ro host
     |     +--ro interface?               if:interface-ref
     |     +--ro host-ip?                 string
     |     +--ro host-hardware-address?   string
     |     +--ro lease?                   uint32
     |     +--ro type?                    allocate-type
     +--rw relay {relay}?
     |  +--rw server-group* [server-group-name]
     |     +--rw server-group-name    string
     |     +--rw interface?           if:interface-ref
     |     +--rw gateway-address?     inet:ipv4-address
     |     +--rw server-address*      inet:ipv4-address
     |     +--ro packet-statistics
     |        +---x clean-statistics
     |        +--ro receive
     |        |  +--ro offer-packet?      uint32
     |        |  +--ro ack-packet?        uint32
     |        |  +--ro nack-packet?       uint32
     |        |  +--ro decline-packet?    uint32
     |        |  +--ro discover-packet?   uint32
     |        |  +--ro request-packet?    uint32
     |        |  +--ro release-packet?    uint32
     |        |  +--ro inform-packet?     uint32
     |        +--ro send
     |           +--ro offer-packet?      uint32
     |           +--ro ack-packet?        uint32
     |           +--ro nack-packet?       uint32
     |           +--ro decline-packet?    uint32
     |           +--ro discover-packet?   uint32
     |           +--ro request-packet?    uint32
     |           +--ro release-packet?    uint32
     |           +--ro inform-packet?     uint32
     +--rw client {client}?
        +--rw interfaces* [interface]
           +--rw interface            if:interface-ref
           +--rw client-id?           string
           +--rw lease?               uint32
           +--ro packet-statistics
              +---x clean-statistics
              +--ro receive
              |  +--ro offer-packet?   uint32
              |  +--ro ack-packet?     uint32
              |  +--ro nack-packet?    uint32
              +--ro send
                 +--ro decline-packet?    uint32
                 +--ro discover-packet?   uint32
                 +--ro request-packet?    uint32
                 +--ro release-packet?    uint32
                 +--ro inform-packet?     uint32
```

- Module Dependencies: `ietf-inet-types` `ietf-yang-types` `ietf-interfaces`

## 8.102. ietf-hardware

```text
module: ietf-hardware
  +--rw hardware
     +--ro last-change?   yang:date-and-time
     +--rw component* [name]
        +--rw name              string
        +--rw class             identityref
        +--ro physical-index?   int32 {entity-mib}?
        +--ro description?      string
        +--rw parent?           -> ../../component/name
        +--rw parent-rel-pos?   int32
        +--ro contains-child*   -> ../../component/name
        +--ro hardware-rev?     string
        +--ro firmware-rev?     string
        +--ro software-rev?     string
        +--ro serial-num?       string
        +--ro mfg-name?         string
        +--ro model-name?       string
        +--rw alias?            string
        +--rw asset-id?         string
        +--ro is-fru?           boolean
        +--ro mfg-date?         yang:date-and-time
        +--rw uri*              inet:uri
        +--ro uuid?             yang:uuid
        +--rw state {hardware-state}?
        |  +--ro state-last-changed?   yang:date-and-time
        |  +--rw admin-state?          admin-state
        |  +--ro oper-state?           oper-state
        |  +--ro usage-state?          usage-state
        |  +--ro alarm-state?          alarm-state
        |  +--ro standby-state?        standby-state
        +--ro sensor-data {hardware-sensor}?
           +--ro value?               sensor-value
           +--ro value-type?          sensor-value-type
           +--ro value-scale?         sensor-value-scale
           +--ro value-precision?     sensor-value-precision
           +--ro oper-status?         sensor-status
           +--ro units-display?       string
           +--ro value-timestamp?     yang:date-and-time
           +--ro value-update-rate?   uint32

  notifications:
    +---n hardware-state-change
    +---n hardware-state-oper-enabled {hardware-state}?
    |  +--ro name?          -> /hardware/component/name
    |  +--ro admin-state?   -> /hardware/component/state/admin-state
    |  +--ro alarm-state?   -> /hardware/component/state/alarm-state
    +---n hardware-state-oper-disabled {hardware-state}?
       +--ro name?          -> /hardware/component/name
       +--ro admin-state?   -> /hardware/component/state/admin-state
       +--ro alarm-state?   -> /hardware/component/state/alarm-state
```

- Module Dependencies: `ietf-inet-types` `ietf-yang-types` `iana-hardware`

## 8.103. ietf-hardware-state

```text
module: ietf-hardware-state
  x--ro hardware
     x--ro last-change?   yang:date-and-time
     x--ro component* [name]
        x--ro name              string
        x--ro class             identityref
        x--ro physical-index?   int32 {entity-mib}?
        x--ro description?      string
        x--ro parent?           -> ../../component/name
        x--ro parent-rel-pos?   int32
        x--ro contains-child*   -> ../../component/name
        x--ro hardware-rev?     string
        x--ro firmware-rev?     string
        x--ro software-rev?     string
        x--ro serial-num?       string
        x--ro mfg-name?         string
        x--ro model-name?       string
        x--ro alias?            string
        x--ro asset-id?         string
        x--ro is-fru?           boolean
        x--ro mfg-date?         yang:date-and-time
        x--ro uri*              inet:uri
        x--ro uuid?             yang:uuid
        x--ro state {hardware-state}?
        |  x--ro state-last-changed?   yang:date-and-time
        |  x--ro admin-state?          hw:admin-state
        |  x--ro oper-state?           hw:oper-state
        |  x--ro usage-state?          hw:usage-state
        |  x--ro alarm-state?          hw:alarm-state
        |  x--ro standby-state?        hw:standby-state
        x--ro sensor-data {hardware-sensor}?
           x--ro value?               hw:sensor-value
           x--ro value-type?          hw:sensor-value-type
           x--ro value-scale?         hw:sensor-value-scale
           x--ro value-precision?     hw:sensor-value-precision
           x--ro oper-status?         hw:sensor-status
           x--ro units-display?       string
           x--ro value-timestamp?     yang:date-and-time
           x--ro value-update-rate?   uint32

  notifications:
    x---n hardware-state-change
    x---n hardware-state-oper-enabled {hardware-state}?
    |  x--ro name?          -> /hardware/component/name
    |  x--ro admin-state?   -> /hardware/component/state/admin-state
    |  x--ro alarm-state?   -> /hardware/component/state/alarm-state
    x---n hardware-state-oper-disabled {hardware-state}?
       x--ro name?          -> /hardware/component/name
       x--ro admin-state?   -> /hardware/component/state/admin-state
       x--ro alarm-state?   -> /hardware/component/state/alarm-state
```

- Module Dependencies: `ietf-inet-types` `ietf-yang-types` `iana-hardware` `ietf-hardware`

## 8.104. ietf-interfaces

```text
module: ietf-interfaces
  +--rw interfaces
  |  +--rw interface* [name]
  |     +--rw name                        string
  |     +--rw description?                string
  |     +--rw type                        identityref
  |     +--rw enabled?                    boolean
  |     +--rw link-up-down-trap-enable?   enumeration {if-mib}?
  |     +--ro admin-status                enumeration {if-mib}?
  |     +--ro oper-status                 enumeration
  |     +--ro last-change?                yang:date-and-time
  |     +--ro if-index                    int32 {if-mib}?
  |     +--ro phys-address?               yang:phys-address
  |     +--ro higher-layer-if*            interface-ref
  |     +--ro lower-layer-if*             interface-ref
  |     +--ro speed?                      yang:gauge64
  |     +--ro statistics
  |        +--ro discontinuity-time    yang:date-and-time
  |        +--ro in-octets?            yang:counter64
  |        +--ro in-unicast-pkts?      yang:counter64
  |        +--ro in-broadcast-pkts?    yang:counter64
  |        +--ro in-multicast-pkts?    yang:counter64
  |        +--ro in-discards?          yang:counter32
  |        +--ro in-errors?            yang:counter32
  |        +--ro in-unknown-protos?    yang:counter32
  |        +--ro out-octets?           yang:counter64
  |        +--ro out-unicast-pkts?     yang:counter64
  |        +--ro out-broadcast-pkts?   yang:counter64
  |        +--ro out-multicast-pkts?   yang:counter64
  |        +--ro out-discards?         yang:counter32
  |        +--ro out-errors?           yang:counter32
  x--ro interfaces-state
     x--ro interface* [name]
        x--ro name               string
        x--ro type               identityref
        x--ro admin-status       enumeration {if-mib}?
        x--ro oper-status        enumeration
        x--ro last-change?       yang:date-and-time
        x--ro if-index           int32 {if-mib}?
        x--ro phys-address?      yang:phys-address
        x--ro higher-layer-if*   interface-state-ref
        x--ro lower-layer-if*    interface-state-ref
        x--ro speed?             yang:gauge64
        x--ro statistics
           x--ro discontinuity-time    yang:date-and-time
           x--ro in-octets?            yang:counter64
           x--ro in-unicast-pkts?      yang:counter64
           x--ro in-broadcast-pkts?    yang:counter64
           x--ro in-multicast-pkts?    yang:counter64
           x--ro in-discards?          yang:counter32
           x--ro in-errors?            yang:counter32
           x--ro in-unknown-protos?    yang:counter32
           x--ro out-octets?           yang:counter64
           x--ro out-unicast-pkts?     yang:counter64
           x--ro out-broadcast-pkts?   yang:counter64
           x--ro out-multicast-pkts?   yang:counter64
           x--ro out-discards?         yang:counter32
           x--ro out-errors?           yang:counter32
```

- Module Dependencies: `ietf-yang-types`

## 8.105. ietf-ip

```text
module: ietf-ip

  augment /if:interfaces/if:interface:
    +--rw ipv4!
    |  +--rw enabled?      boolean
    |  +--rw forwarding?   boolean
    |  +--rw mtu?          uint16
    |  +--rw address* [ip]
    |  |  +--rw ip                     inet:ipv4-address-no-zone
    |  |  +--rw (subnet)
    |  |  |  +--:(prefix-length)
    |  |  |  |  +--rw prefix-length?   uint8
    |  |  |  +--:(netmask)
    |  |  |     +--rw netmask?         yang:dotted-quad {ipv4-non-contiguous-netmasks}?
    |  |  +--ro origin?                ip-address-origin
    |  +--rw neighbor* [ip]
    |     +--rw ip                    inet:ipv4-address-no-zone
    |     +--rw link-layer-address    yang:phys-address
    |     +--ro origin?               neighbor-origin
    +--rw ipv6!
       +--rw enabled?                     boolean
       +--rw forwarding?                  boolean
       +--rw mtu?                         uint32
       +--rw address* [ip]
       |  +--rw ip               inet:ipv6-address-no-zone
       |  +--rw prefix-length    uint8
       |  +--ro origin?          ip-address-origin
       |  +--ro status?          enumeration
       +--rw neighbor* [ip]
       |  +--rw ip                    inet:ipv6-address-no-zone
       |  +--rw link-layer-address    yang:phys-address
       |  +--ro origin?               neighbor-origin
       |  +--ro is-router?            empty
       |  +--ro state?                enumeration
       +--rw dup-addr-detect-transmits?   uint32
       +--rw autoconf
          +--rw create-global-addresses?        boolean
          +--rw create-temporary-addresses?     boolean {ipv6-privacy-autoconf}?
          +--rw temporary-valid-lifetime?       uint32 {ipv6-privacy-autoconf}?
          +--rw temporary-preferred-lifetime?   uint32 {ipv6-privacy-autoconf}?
  augment /if:interfaces-state/if:interface:
    x--ro ipv4!
    |  x--ro forwarding?   boolean
    |  x--ro mtu?          uint16
    |  x--ro address* [ip]
    |  |  x--ro ip                     inet:ipv4-address-no-zone
    |  |  x--ro (subnet)?
    |  |  |  x--:(prefix-length)
    |  |  |  |  x--ro prefix-length?   uint8
    |  |  |  x--:(netmask)
    |  |  |     x--ro netmask?         yang:dotted-quad {ipv4-non-contiguous-netmasks}?
    |  |  x--ro origin?                ip-address-origin
    |  x--ro neighbor* [ip]
    |     x--ro ip                    inet:ipv4-address-no-zone
    |     x--ro link-layer-address?   yang:phys-address
    |     x--ro origin?               neighbor-origin
    x--ro ipv6!
       x--ro forwarding?   boolean
       x--ro mtu?          uint32
       x--ro address* [ip]
       |  x--ro ip               inet:ipv6-address-no-zone
       |  x--ro prefix-length    uint8
       |  x--ro origin?          ip-address-origin
       |  x--ro status?          enumeration
       x--ro neighbor* [ip]
          x--ro ip                    inet:ipv6-address-no-zone
          x--ro link-layer-address?   yang:phys-address
          x--ro origin?               neighbor-origin
          x--ro is-router?            empty
          x--ro state?                enumeration
```

- Module Dependencies: `ietf-interfaces` `ietf-inet-types` `ietf-yang-types`

## 8.106. ietf-keystore

```text
module: ietf-keystore
  +--rw keystore
     +--rw asymmetric-keys
     |  +--rw asymmetric-key* [name]
     |     +--rw name                                    string
     |     +--rw public-key-format                       identityref
     |     +--rw public-key                              binary
     |     +--rw private-key-format?                     identityref
     |     +--rw (private-key-type)
     |     |  +--:(cleartext-private-key)
     |     |  |  +--rw cleartext-private-key?            binary
     |     |  +--:(hidden-private-key)
     |     |  |  +--rw hidden-private-key?               empty
     |     |  +--:(encrypted-private-key) {private-key-encryption}?
     |     |     +--rw encrypted-private-key
     |     |        +--rw encrypted-by
     |     |        |  +--rw (encrypted-by-choice)
     |     |        |     +--:(symmetric-key-ref)
     |     |        |     |  +--rw symmetric-key-ref?    ks:symmetric-key-ref
     |     |        |     +--:(asymmetric-key-ref)
     |     |        |        +--rw asymmetric-key-ref?   ks:asymmetric-key-ref
     |     |        +--rw encrypted-value-format    identityref
     |     |        +--rw encrypted-value           binary
     |     +--rw certificates
     |     |  +--rw certificate* [name]
     |     |     +--rw name                      string
     |     |     +--rw cert-data                 end-entity-cert-cms
     |     |     +---n certificate-expiration {certificate-expiration-notification}?
     |     |        +-- expiration-date    yang:date-and-time
     |     +---x generate-certificate-signing-request {certificate-signing-request-generation}?
     |        +---w input
     |        |  +---w csr-info    ct:csr-info
     |        +--ro output
     |           +--ro certificate-signing-request    ct:csr
     +--rw symmetric-keys
        +--rw symmetric-key* [name]
           +--rw name                   string
           +--rw key-format?            identityref
           +--rw (key-type)
              +--:(cleartext-key)
              |  +--rw cleartext-key?   binary
              +--:(hidden-key)
              |  +--rw hidden-key?      empty
              +--:(encrypted-key) {symmetric-key-encryption}?
                 +--rw encrypted-key
                    +--rw encrypted-by
                    |  +--rw (encrypted-by-choice)
                    |     +--:(symmetric-key-ref)
                    |     |  +--rw symmetric-key-ref?    ks:symmetric-key-ref
                    |     +--:(asymmetric-key-ref)
                    |        +--rw asymmetric-key-ref?   ks:asymmetric-key-ref
                    +--rw encrypted-value-format    identityref
                    +--rw encrypted-value           binary
```

- Module Dependencies: `ietf-netconf-acm` `ietf-crypto-types`

## 8.107. ietf-netconf-acm

```text
module: ietf-netconf-acm
  +--rw nacm
     +--rw enable-nacm?              boolean
     +--rw read-default?             action-type
     +--rw write-default?            action-type
     +--rw exec-default?             action-type
     +--rw enable-external-groups?   boolean
     +--ro denied-operations         yang:zero-based-counter32
     +--ro denied-data-writes        yang:zero-based-counter32
     +--ro denied-notifications      yang:zero-based-counter32
     +--rw groups
     |  +--rw group* [name]
     |     +--rw name         group-name-type
     |     +--rw user-name*   user-name-type
     +--rw rule-list* [name]
        +--rw name     string
        +--rw group*   union
        +--rw rule* [name]
           +--rw name                       string
           +--rw module-name?               union
           +--rw (rule-type)?
           |  +--:(protocol-operation)
           |  |  +--rw rpc-name?            union
           |  +--:(notification)
           |  |  +--rw notification-name?   union
           |  +--:(data-node)
           |     +--rw path                 node-instance-identifier
           +--rw access-operations?         union
           +--rw action                     action-type
           +--rw comment?                   string
```

- Module Dependencies: `ietf-yang-types`

## 8.108. ietf-netconf

```text
module: ietf-netconf

  rpcs:
    +---x get-config
    |  +---w input
    |  |  +---w source
    |  |  |  +---w (config-source)
    |  |  |     +--:(candidate)
    |  |  |     |  +---w candidate?   empty {candidate}?
    |  |  |     +--:(running)
    |  |  |     |  +---w running?     empty
    |  |  |     +--:(startup)
    |  |  |        +---w startup?     empty {startup}?
    |  |  +---w filter?   <anyxml>
    |  +--ro output
    |     +--ro data?   <anyxml>
    +---x edit-config
    |  +---w input
    |     +---w target
    |     |  +---w (config-target)
    |     |     +--:(candidate)
    |     |     |  +---w candidate?   empty {candidate}?
    |     |     +--:(running)
    |     |        +---w running?     empty {writable-running}?
    |     +---w default-operation?   enumeration
    |     +---w test-option?         enumeration {validate}?
    |     +---w error-option?        enumeration
    |     +---w (edit-content)
    |        +--:(config)
    |        |  +---w config?        <anyxml>
    |        +--:(url)
    |           +---w url?           inet:uri {url}?
    +---x copy-config
    |  +---w input
    |     +---w target
    |     |  +---w (config-target)
    |     |     +--:(candidate)
    |     |     |  +---w candidate?   empty {candidate}?
    |     |     +--:(running)
    |     |     |  +---w running?     empty {writable-running}?
    |     |     +--:(startup)
    |     |     |  +---w startup?     empty {startup}?
    |     |     +--:(url)
    |     |        +---w url?         inet:uri {url}?
    |     +---w source
    |        +---w (config-source)
    |           +--:(candidate)
    |           |  +---w candidate?   empty {candidate}?
    |           +--:(running)
    |           |  +---w running?     empty
    |           +--:(startup)
    |           |  +---w startup?     empty {startup}?
    |           +--:(url)
    |           |  +---w url?         inet:uri {url}?
    |           +--:(config)
    |              +---w config?      <anyxml>
    +---x delete-config
    |  +---w input
    |     +---w target
    |        +---w (config-target)
    |           +--:(startup)
    |           |  +---w startup?   empty {startup}?
    |           +--:(url)
    |              +---w url?       inet:uri {url}?
    +---x lock
    |  +---w input
    |     +---w target
    |        +---w (config-target)
    |           +--:(candidate)
    |           |  +---w candidate?   empty {candidate}?
    |           +--:(running)
    |           |  +---w running?     empty
    |           +--:(startup)
    |              +---w startup?     empty {startup}?
    +---x unlock
    |  +---w input
    |     +---w target
    |        +---w (config-target)
    |           +--:(candidate)
    |           |  +---w candidate?   empty {candidate}?
    |           +--:(running)
    |           |  +---w running?     empty
    |           +--:(startup)
    |              +---w startup?     empty {startup}?
    +---x get
    |  +---w input
    |  |  +---w filter?   <anyxml>
    |  +--ro output
    |     +--ro data?   <anyxml>
    +---x close-session
    +---x kill-session
    |  +---w input
    |     +---w session-id    session-id-type
    +---x commit {candidate}?
    |  +---w input
    |     +---w confirmed?         empty {confirmed-commit}?
    |     +---w confirm-timeout?   uint32 {confirmed-commit}?
    |     +---w persist?           string {confirmed-commit}?
    |     +---w persist-id?        string {confirmed-commit}?
    +---x discard-changes {candidate}?
    +---x cancel-commit {confirmed-commit}?
    |  +---w input
    |     +---w persist-id?   string
    +---x validate {validate}?
       +---w input
          +---w source
             +---w (config-source)
                +--:(candidate)
                |  +---w candidate?   empty {candidate}?
                +--:(running)
                |  +---w running?     empty
                +--:(startup)
                |  +---w startup?     empty {startup}?
                +--:(url)
                |  +---w url?         inet:uri {url}?
                +--:(config)
                   +---w config?      <anyxml>
```

- Module Dependencies: `ietf-inet-types`

## 8.109. ietf-netconf-monitoring

```text
module: ietf-netconf-monitoring
  +--ro netconf-state
     +--ro capabilities
     |  +--ro capability*   inet:uri
     +--ro datastores
     |  +--ro datastore* [name]
     |     +--ro name     netconf-datastore-type
     |     +--ro locks!
     |        +--ro (lock-type)?
     |           +--:(global-lock)
     |           |  +--ro global-lock
     |           |     +--ro locked-by-session    uint32
     |           |     +--ro locked-time          yang:date-and-time
     |           +--:(partial-lock)
     |              +--ro partial-lock* [lock-id]
     |                 +--ro lock-id              uint32
     |                 +--ro locked-by-session    uint32
     |                 +--ro locked-time          yang:date-and-time
     |                 +--ro select*              yang:xpath1.0
     |                 +--ro locked-node*         instance-identifier
     +--ro schemas
     |  +--ro schema* [identifier version format]
     |     +--ro identifier    string
     |     +--ro version       string
     |     +--ro format        identityref
     |     +--ro namespace     inet:uri
     |     +--ro location*     union
     +--ro sessions
     |  +--ro session* [session-id]
     |     +--ro session-id           uint32
     |     +--ro transport            identityref
     |     +--ro username             string
     |     +--ro source-host?         inet:host
     |     +--ro login-time           yang:date-and-time
     |     +--ro in-rpcs?             yang:zero-based-counter32
     |     +--ro in-bad-rpcs?         yang:zero-based-counter32
     |     +--ro out-rpc-errors?      yang:zero-based-counter32
     |     +--ro out-notifications?   yang:zero-based-counter32
     +--ro statistics
        +--ro netconf-start-time?   yang:date-and-time
        +--ro in-bad-hellos?        yang:zero-based-counter32
        +--ro in-sessions?          yang:zero-based-counter32
        +--ro dropped-sessions?     yang:zero-based-counter32
        +--ro in-rpcs?              yang:zero-based-counter32
        +--ro in-bad-rpcs?          yang:zero-based-counter32
        +--ro out-rpc-errors?       yang:zero-based-counter32
        +--ro out-notifications?    yang:zero-based-counter32

  rpcs:
    +---x get-schema
       +---w input
       |  +---w identifier    string
       |  +---w version?      string
       |  +---w format?       identityref
       +--ro output
          +--ro data?   <anyxml>
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types`

## 8.110. ietf-netconf-nmda

```text
module: ietf-netconf-nmda

  augment /nc:lock/nc:input/nc:target/nc:config-target:
    +-- datastore?   ds:datastore-ref
  augment /nc:unlock/nc:input/nc:target/nc:config-target:
    +-- datastore?   ds:datastore-ref
  augment /nc:validate/nc:input/nc:source/nc:config-source:
    +-- datastore?   ds:datastore-ref

  rpcs:
    +---x get-data
    |  +---w input
    |  |  +---w datastore                      ds:datastore-ref
    |  |  +---w (filter-spec)?
    |  |  |  +--:(subtree-filter)
    |  |  |  |  +---w subtree-filter?          <anydata>
    |  |  |  +--:(xpath-filter)
    |  |  |     +---w xpath-filter?            yang:xpath1.0 {nc:xpath}?
    |  |  +---w config-filter?                 boolean
    |  |  +---w (origin-filters)? {origin}?
    |  |  |  +--:(origin-filter)
    |  |  |  |  +---w origin-filter*           or:origin-ref
    |  |  |  +--:(negated-origin-filter)
    |  |  |     +---w negated-origin-filter*   or:origin-ref
    |  |  +---w max-depth?                     union
    |  |  +---w with-origin?                   empty {origin}?
    |  |  +---w with-defaults?                 with-defaults-mode {with-defaults}?
    |  +--ro output
    |     +--ro data?   <anydata>
    +---x edit-data
       +---w input
          +---w datastore            ds:datastore-ref
          +---w default-operation?   enumeration
          +---w (edit-content)
             +--:(config)
             |  +---w config?        <anydata>
             +--:(url)
                +---w url?           inet:uri {nc:url}?
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types` `ietf-datastores` `ietf-origin` `ietf-netconf` `ietf-netconf-with-defaults`

## 8.111. ietf-netconf-notifications

```text
module: ietf-netconf-notifications

  notifications:
    +---n netconf-config-change
    |  +--ro changed-by
    |  |  +--ro (server-or-user)
    |  |     +--:(server)
    |  |     |  +--ro server?        empty
    |  |     +--:(by-user)
    |  |        +--ro username       string
    |  |        +--ro session-id     nc:session-id-or-zero-type
    |  |        +--ro source-host?   inet:ip-address
    |  +--ro datastore?    enumeration
    |  +--ro edit* []
    |     +--ro target?      instance-identifier
    |     +--ro operation?   nc:edit-operation-type
    +---n netconf-capability-change
    |  +--ro changed-by
    |  |  +--ro (server-or-user)
    |  |     +--:(server)
    |  |     |  +--ro server?        empty
    |  |     +--:(by-user)
    |  |        +--ro username       string
    |  |        +--ro session-id     nc:session-id-or-zero-type
    |  |        +--ro source-host?   inet:ip-address
    |  +--ro added-capability*      inet:uri
    |  +--ro deleted-capability*    inet:uri
    |  +--ro modified-capability*   inet:uri
    +---n netconf-session-start
    |  +--ro username       string
    |  +--ro session-id     nc:session-id-or-zero-type
    |  +--ro source-host?   inet:ip-address
    +---n netconf-session-end
    |  +--ro username              string
    |  +--ro session-id            nc:session-id-or-zero-type
    |  +--ro source-host?          inet:ip-address
    |  +--ro killed-by?            nc:session-id-type
    |  +--ro termination-reason    enumeration
    +---n netconf-confirmed-commit
       +--ro username         string
       +--ro session-id       nc:session-id-or-zero-type
       +--ro source-host?     inet:ip-address
       +--ro confirm-event    enumeration
       +--ro timeout?         uint32
```

- Module Dependencies: `ietf-inet-types` `ietf-netconf`

## 8.112. ietf-netconf-partial-lock

```text
module: ietf-netconf-partial-lock

  rpcs:
    +---x partial-lock
    |  +---w input
    |  |  +---w select*   string
    |  +--ro output
    |     +--ro lock-id?       lock-id-type
    |     +--ro locked-node*   instance-identifier
    +---x partial-unlock
       +---w input
          +---w lock-id?   lock-id-type
```

- Module Dependencies: `N/A`

## 8.113. ietf-netconf-with-defaults

```text
module: ietf-netconf-with-defaults

  augment /nc:get-config/nc:input:
    +---w with-defaults?   with-defaults-mode
  augment /nc:get/nc:input:
    +---w with-defaults?   with-defaults-mode
  augment /nc:copy-config/nc:input:
    +---w with-defaults?   with-defaults-mode
```

- Module Dependencies: `ietf-netconf`

## 8.114. ietf-network-bridge

```text
module: ietf-network-bridge
  +--rw bridge
     +--rw ports
        +--rw port* [name]
           +--rw name     string
           +--rw index?   uint64

  augment /if:interfaces/if:interface:
    +--rw port-name?   -> /bridge/ports/port/name
```

- Module Dependencies: `ietf-interfaces`

## 8.115. ietf-network-instance

```text
module: ietf-network-instance
  +--rw network-instances
     +--rw network-instance* [name]
        +--rw name              string
        +--rw enabled?          boolean
        +--rw description?      string
        +--rw (ni-type)?
        +--rw (root-type)
           +--:(vrf-root)
           |  +--rw vrf-root
           +--:(vsi-root)
           |  +--rw vsi-root
           +--:(vv-root)
              +--rw vv-root

  augment /if:interfaces/if:interface:
    +--rw bind-ni-name?   -> /network-instances/network-instance/name
  augment /if:interfaces/if:interface/ip:ipv4:
    +--rw bind-ni-name?   -> /network-instances/network-instance/name
  augment /if:interfaces/if:interface/ip:ipv6:
    +--rw bind-ni-name?   -> /network-instances/network-instance/name

  notifications:
    +---n bind-ni-name-failed
       +--ro name          -> /if:interfaces/interface/name
       +--ro interface
       |  +--ro bind-ni-name?   -> /if:interfaces/interface/ni:bind-ni-name
       +--ro ipv4
       |  +--ro bind-ni-name?   -> /if:interfaces/interface/ip:ipv4/ni:bind-ni-name
       +--ro ipv6
       |  +--ro bind-ni-name?   -> /if:interfaces/interface/ip:ipv6/ni:bind-ni-name
       +--ro error-info?   string
```

- Module Dependencies: `ietf-interfaces` `ietf-ip` `ietf-yang-schema-mount`

## 8.116. ietf-network

```text
module: ietf-network
  +--rw networks
     +--rw network* [network-id]
        +--rw network-id            network-id
        +--rw network-types
        +--rw supporting-network* [network-ref]
        |  +--rw network-ref    -> /networks/network/network-id
        +--rw node* [node-id]
           +--rw node-id            node-id
           +--rw supporting-node* [network-ref node-ref]
              +--rw network-ref    -> ../../../supporting-network/network-ref
              +--rw node-ref       -> /networks/network/node/node-id
```

- Module Dependencies: `ietf-inet-types`

## 8.117. ietf-network-topology

```text
module: ietf-network-topology

  augment /nw:networks/nw:network:
    +--rw link* [link-id]
       +--rw link-id            link-id
       +--rw source
       |  +--rw source-node?   -> ../../../nw:node/node-id
       |  +--rw source-tp?     -> ../../../nw:node[nw:node-id=current()/../source-node]/termination-point/tp-id
       +--rw destination
       |  +--rw dest-node?   -> ../../../nw:node/node-id
       |  +--rw dest-tp?     -> ../../../nw:node[nw:node-id=current()/../dest-node]/termination-point/tp-id
       +--rw supporting-link* [network-ref link-ref]
          +--rw network-ref    -> ../../../nw:supporting-network/network-ref
          +--rw link-ref       -> /nw:networks/network[nw:network-id=current()/../network-ref]/link/link-id
  augment /nw:networks/nw:network/nw:node:
    +--rw termination-point* [tp-id]
       +--rw tp-id                           tp-id
       +--rw supporting-termination-point* [network-ref node-ref tp-ref]
          +--rw network-ref    -> ../../../nw:supporting-node/network-ref
          +--rw node-ref       -> ../../../nw:supporting-node/node-ref
          +--rw tp-ref         -> /nw:networks/network[nw:network-id=current()/../network-ref]/node[nw:node-id=current()/../node-ref]/termination-point/tp-id
```

- Module Dependencies: `ietf-inet-types` `ietf-network`

## 8.118. ietf-ntp

```text
module: ietf-ntp
  +--rw ntp!
     +--rw port?                    inet:port-number {ntp-port}?
     +--rw refclock-master!
     |  +--rw master-stratum?   ntp-stratum
     +--rw authentication {authentication}?
     |  +--rw auth-enabled?          boolean
     |  +--rw authentication-keys* [key-id]
     |     +--rw key-id       uint32
     |     +--rw algorithm?   identityref
     |     +--rw key
     |     |  +--rw (key-string-style)?
     |     |     +--:(keystring)
     |     |     |  +--rw keystring?            string {deprecated}?
     |     |     +--:(hexadecimal) {hex-key-string}?
     |     |        +--rw hexadecimal-string?   yang:hex-string
     |     +--rw istrusted?   boolean
     +--rw access-rules {access-rules}?
     |  +--rw access-rule* [access-mode]
     |     +--rw access-mode    identityref
     |     +--rw acl?           -> /acl:acls/acl/name
     +--ro clock-state
     |  +--ro system-status
     |     +--ro clock-state                  identityref
     |     +--ro clock-stratum                ntp-stratum
     |     +--ro clock-refid                  refid
     |     +--ro associations-address?        -> /ntp/associations/association/address
     |     +--ro associations-local-mode?     -> /ntp/associations/association/local-mode
     |     +--ro associations-isconfigured?   -> /ntp/associations/association/isconfigured
     |     +--ro nominal-freq                 decimal64
     |     +--ro actual-freq                  decimal64
     |     +--ro clock-precision              log2seconds
     |     +--ro clock-offset?                decimal64
     |     +--ro root-delay?                  decimal64
     |     +--ro root-dispersion?             decimal64
     |     +--ro reference-time?              ntp-date-and-time
     |     +--ro sync-state                   identityref
     +--rw unicast-configuration* [address type] {unicast-configuration}?
     |  +--rw address           inet:ip-address
     |  +--rw type              identityref
     |  +--rw authentication {authentication}?
     |  |  +--rw (authentication-type)?
     |  |     +--:(symmetric-key)
     |  |        +--rw key-id?   -> /ntp/authentication/authentication-keys/key-id
     |  +--rw prefer?           boolean
     |  +--rw burst?            boolean
     |  +--rw iburst?           boolean
     |  +--rw source?           if:interface-ref
     |  +--rw minpoll?          log2seconds
     |  +--rw maxpoll?          log2seconds
     |  +--rw port?             inet:port-number {ntp-port}?
     |  +--rw version?          ntp-version
     +--rw associations
     |  +--ro association* [address local-mode isconfigured]
     |     +--ro address           inet:ip-address
     |     +--ro local-mode        identityref
     |     +--ro isconfigured      boolean
     |     +--ro stratum?          ntp-stratum
     |     +--ro refid?            refid
     |     +--ro authentication?   -> /ntp/authentication/authentication-keys/key-id {authentication}?
     |     +--ro prefer?           boolean
     |     +--ro peer-interface?   if:interface-ref
     |     +--ro minpoll?          log2seconds
     |     +--ro maxpoll?          log2seconds
     |     +--ro port?             inet:port-number {ntp-port}?
     |     +--ro version?          ntp-version
     |     +--ro reach?            uint8
     |     +--ro unreach?          uint8
     |     +--ro poll?             log2seconds
     |     +--ro now?              uint32
     |     +--ro offset?           decimal64
     |     +--ro delay?            decimal64
     |     +--ro dispersion?       decimal64
     |     +--ro originate-time?   ntp-date-and-time
     |     +--ro receive-time?     ntp-date-and-time
     |     +--ro transmit-time?    ntp-date-and-time
     |     +--ro input-time?       ntp-date-and-time
     |     +--ro ntp-statistics
     |        +--ro discontinuity-time?   ntp-date-and-time
     |        +--ro packet-sent?          yang:counter32
     |        +--ro packet-sent-fail?     yang:counter32
     |        +--ro packet-received?      yang:counter32
     |        +--ro packet-dropped?       yang:counter32
     +--rw interfaces
     |  +--rw interface* [name]
     |     +--rw name                if:interface-ref
     |     +--rw broadcast-server! {broadcast-server}?
     |     |  +--rw ttl?              uint8
     |     |  +--rw authentication {authentication}?
     |     |  |  +--rw (authentication-type)?
     |     |  |     +--:(symmetric-key)
     |     |  |        +--rw key-id?   -> /ntp/authentication/authentication-keys/key-id
     |     |  +--rw minpoll?          log2seconds
     |     |  +--rw maxpoll?          log2seconds
     |     |  +--rw port?             inet:port-number {ntp-port}?
     |     |  +--rw version?          ntp-version
     |     +--rw broadcast-client! {broadcast-client}?
     |     +--rw multicast-server* [address] {multicast-server}?
     |     |  +--rw address           rt-types:ip-multicast-group-address
     |     |  +--rw ttl?              uint8
     |     |  +--rw authentication {authentication}?
     |     |  |  +--rw (authentication-type)?
     |     |  |     +--:(symmetric-key)
     |     |  |        +--rw key-id?   -> /ntp/authentication/authentication-keys/key-id
     |     |  +--rw minpoll?          log2seconds
     |     |  +--rw maxpoll?          log2seconds
     |     |  +--rw port?             inet:port-number {ntp-port}?
     |     |  +--rw version?          ntp-version
     |     +--rw multicast-client* [address] {multicast-client}?
     |     |  +--rw address    rt-types:ip-multicast-group-address
     |     +--rw manycast-server* [address] {manycast-server}?
     |     |  +--rw address    rt-types:ip-multicast-group-address
     |     +--rw manycast-client* [address] {manycast-client}?
     |        +--rw address           rt-types:ip-multicast-group-address
     |        +--rw authentication {authentication}?
     |        |  +--rw (authentication-type)?
     |        |     +--:(symmetric-key)
     |        |        +--rw key-id?   -> /ntp/authentication/authentication-keys/key-id
     |        +--rw ttl?              uint8
     |        +--rw minclock?         uint8
     |        +--rw maxclock?         uint8
     |        +--rw beacon?           log2seconds
     |        +--rw minpoll?          log2seconds
     |        +--rw maxpoll?          log2seconds
     |        +--rw port?             inet:port-number {ntp-port}?
     |        +--rw version?          ntp-version
     +--ro ntp-statistics
        +--ro discontinuity-time?   ntp-date-and-time
        +--ro packet-sent?          yang:counter32
        +--ro packet-sent-fail?     yang:counter32
        +--ro packet-received?      yang:counter32
        +--ro packet-dropped?       yang:counter32

  rpcs:
    +---x statistics-reset
       +---w input
          +---w (association-or-all)?
             +--:(association)
             |  +---w associations-address?        -> /ntp/associations/association/address
             |  +---w associations-local-mode?     -> /ntp/associations/association/local-mode
             |  +---w associations-isconfigured?   -> /ntp/associations/association/isconfigured
             +--:(all)
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types` `ietf-interfaces` `ietf-system` `ietf-access-control-list` `ietf-routing-types` `ietf-netconf-acm`

## 8.119. ietf-routing

```text
module: ietf-routing
  +--rw routing
  |  +--rw router-id?                 yang:dotted-quad {router-id}?
  |  +--ro interfaces
  |  |  +--ro interface*   if:interface-ref
  |  +--rw control-plane-protocols
  |  |  +--rw control-plane-protocol* [type name]
  |  |     +--rw type             identityref
  |  |     +--rw name             string
  |  |     +--rw description?     string
  |  |     +--rw static-routes
  |  +--rw ribs
  |     +--rw rib* [name]
  |        +--rw name              string
  |        +--rw address-family    identityref
  |        +--ro default-rib?      boolean {multiple-ribs}?
  |        +--ro routes
  |        |  +--ro route* []
  |        |     +--ro route-preference?   route-preference
  |        |     +--ro next-hop
  |        |     |  +--ro (next-hop-options)
  |        |     |     +--:(simple-next-hop)
  |        |     |     |  +--ro outgoing-interface?   if:interface-ref
  |        |     |     +--:(special-next-hop)
  |        |     |     |  +--ro special-next-hop?     enumeration
  |        |     |     +--:(next-hop-list)
  |        |     |        +--ro next-hop-list
  |        |     |           +--ro next-hop* []
  |        |     |              +--ro outgoing-interface?   if:interface-ref
  |        |     +--ro source-protocol     identityref
  |        |     +--ro active?             empty
  |        |     +--ro last-updated?       yang:date-and-time
  |        +---x active-route
  |        |  +--ro output
  |        |     +--ro route
  |        |        +--ro next-hop
  |        |        |  +--ro (next-hop-options)
  |        |        |     +--:(simple-next-hop)
  |        |        |     |  +--ro outgoing-interface?   if:interface-ref
  |        |        |     +--:(special-next-hop)
  |        |        |     |  +--ro special-next-hop?     enumeration
  |        |        |     +--:(next-hop-list)
  |        |        |        +--ro next-hop-list
  |        |        |           +--ro next-hop* []
  |        |        |              +--ro outgoing-interface?   if:interface-ref
  |        |        +--ro source-protocol    identityref
  |        |        +--ro active?            empty
  |        |        +--ro last-updated?      yang:date-and-time
  |        +--rw description?      string
  o--ro routing-state
     +--ro router-id?                 yang:dotted-quad
     o--ro interfaces
     |  o--ro interface*   if:interface-state-ref
     o--ro control-plane-protocols
     |  o--ro control-plane-protocol* [type name]
     |     o--ro type    identityref
     |     o--ro name    string
     o--ro ribs
        o--ro rib* [name]
           o--ro name              string
           +--ro address-family    identityref
           o--ro default-rib?      boolean {multiple-ribs}?
           o--ro routes
           |  o--ro route* []
           |     o--ro route-preference?   route-preference
           |     o--ro next-hop
           |     |  +--ro (next-hop-options)
           |     |     +--:(simple-next-hop)
           |     |     |  +--ro outgoing-interface?   if:interface-ref
           |     |     +--:(special-next-hop)
           |     |     |  +--ro special-next-hop?     enumeration
           |     |     +--:(next-hop-list)
           |     |        +--ro next-hop-list
           |     |           +--ro next-hop* []
           |     |              +--ro outgoing-interface?   if:interface-ref
           |     +--ro source-protocol     identityref
           |     +--ro active?             empty
           |     +--ro last-updated?       yang:date-and-time
           o---x active-route
              +--ro output
                 o--ro route
                    o--ro next-hop
                    |  +--ro (next-hop-options)
                    |     +--:(simple-next-hop)
                    |     |  +--ro outgoing-interface?   if:interface-ref
                    |     +--:(special-next-hop)
                    |     |  +--ro special-next-hop?     enumeration
                    |     +--:(next-hop-list)
                    |        +--ro next-hop-list
                    |           +--ro next-hop* []
                    |              +--ro outgoing-interface?   if:interface-ref
                    +--ro source-protocol    identityref
                    +--ro active?            empty
                    +--ro last-updated?      yang:date-and-time
```

- Module Dependencies: `ietf-yang-types` `ietf-interfaces`

## 8.120. ietf-subscribed-notifications

```text
module: ietf-subscribed-notifications
  +--ro streams
  |  +--ro stream* [name]
  |     +--ro name                        string
  |     +--ro description?                string
  |     +--ro replay-support?             empty {replay}?
  |     +--ro replay-log-creation-time    yang:date-and-time {replay}?
  |     +--ro replay-log-aged-time?       yang:date-and-time {replay}?
  +--rw filters
  |  +--rw stream-filter* [name]
  |     +--rw name                           string
  |     +--rw (filter-spec)?
  |        +--:(stream-subtree-filter)
  |        |  +--rw stream-subtree-filter?   <anydata> {subtree}?
  |        +--:(stream-xpath-filter)
  |           +--rw stream-xpath-filter?     yang:xpath1.0 {xpath}?
  +--rw subscriptions
     +--rw subscription* [id]
        +--rw id                                         subscription-id
        +--rw (target)
        |  +--:(stream)
        |     +--rw (stream-filter)?
        |     |  +--:(by-reference)
        |     |  |  +--rw stream-filter-name             stream-filter-ref
        |     |  +--:(within-subscription)
        |     |     +--rw (filter-spec)?
        |     |        +--:(stream-subtree-filter)
        |     |        |  +--rw stream-subtree-filter?   <anydata> {subtree}?
        |     |        +--:(stream-xpath-filter)
        |     |           +--rw stream-xpath-filter?     yang:xpath1.0 {xpath}?
        |     +--rw stream                               stream-ref
        |     +--ro replay-start-time?                   yang:date-and-time {replay}?
        |     +--rw configured-replay?                   empty {configured,replay}?
        +--rw stop-time?                                 yang:date-and-time
        +--rw dscp?                                      inet:dscp {dscp}?
        +--rw weighting?                                 uint8 {qos}?
        +--rw dependency?                                subscription-id {qos}?
        +--rw transport?                                 transport {configured}?
        +--rw encoding?                                  encoding
        +--rw purpose?                                   string {configured}?
        +--rw (notification-message-origin)? {configured}?
        |  +--:(interface-originated)
        |  |  +--rw source-interface?                    if:interface-ref {interface-designation}?
        |  +--:(address-originated)
        |     +--rw source-vrf?                          -> /ni:network-instances/network-instance/name {supports-vrf}?
        |     +--rw source-address?                      inet:ip-address-no-zone
        +--ro configured-subscription-state?             enumeration {configured}?
        +--rw receivers
           +--rw receiver* [name]
              +--rw name                      string
              +--ro sent-event-records?       yang:zero-based-counter64
              +--ro excluded-event-records?   yang:zero-based-counter64
              +--ro state                     enumeration
              +---x reset {configured}?
                 +--ro output
                    +--ro time    yang:date-and-time

  rpcs:
    +---x establish-subscription
    |  +---w input
    |  |  +---w (target)
    |  |  |  +--:(stream)
    |  |  |     +---w (stream-filter)?
    |  |  |     |  +--:(by-reference)
    |  |  |     |  |  +---w stream-filter-name             stream-filter-ref
    |  |  |     |  +--:(within-subscription)
    |  |  |     |     +---w (filter-spec)?
    |  |  |     |        +--:(stream-subtree-filter)
    |  |  |     |        |  +---w stream-subtree-filter?   <anydata> {subtree}?
    |  |  |     |        +--:(stream-xpath-filter)
    |  |  |     |           +---w stream-xpath-filter?     yang:xpath1.0 {xpath}?
    |  |  |     +---w stream                               stream-ref
    |  |  |     +---w replay-start-time?                   yang:date-and-time {replay}?
    |  |  +---w stop-time?                                 yang:date-and-time
    |  |  +---w dscp?                                      inet:dscp {dscp}?
    |  |  +---w weighting?                                 uint8 {qos}?
    |  |  +---w dependency?                                subscription-id {qos}?
    |  |  +---w encoding?                                  encoding
    |  +--ro output
    |     +--ro id                            subscription-id
    |     +--ro replay-start-time-revision?   yang:date-and-time {replay}?
    +---x modify-subscription
    |  +---w input
    |     +---w id                                         subscription-id
    |     +---w (target)
    |     |  +--:(stream)
    |     |     +---w (stream-filter)?
    |     |        +--:(by-reference)
    |     |        |  +---w stream-filter-name             stream-filter-ref
    |     |        +--:(within-subscription)
    |     |           +---w (filter-spec)?
    |     |              +--:(stream-subtree-filter)
    |     |              |  +---w stream-subtree-filter?   <anydata> {subtree}?
    |     |              +--:(stream-xpath-filter)
    |     |                 +---w stream-xpath-filter?     yang:xpath1.0 {xpath}?
    |     +---w stop-time?                                 yang:date-and-time
    +---x delete-subscription
    |  +---w input
    |     +---w id    subscription-id
    +---x kill-subscription
       +---w input
          +---w id    subscription-id

  notifications:
    +---n replay-completed {replay}?
    |  +--ro id    subscription-id
    +---n subscription-completed {configured}?
    |  +--ro id    subscription-id
    +---n subscription-modified
    |  +--ro id                                         subscription-id
    |  +--ro (target)
    |  |  +--:(stream)
    |  |     +--ro (stream-filter)?
    |  |     |  +--:(by-reference)
    |  |     |  |  +--ro stream-filter-name             stream-filter-ref
    |  |     |  +--:(within-subscription)
    |  |     |     +--ro (filter-spec)?
    |  |     |        +--:(stream-subtree-filter)
    |  |     |        |  +--ro stream-subtree-filter?   <anydata> {subtree}?
    |  |     |        +--:(stream-xpath-filter)
    |  |     |           +--ro stream-xpath-filter?     yang:xpath1.0 {xpath}?
    |  |     +--ro stream                               stream-ref
    |  |     +--ro replay-start-time?                   yang:date-and-time {replay}?
    |  +--ro stop-time?                                 yang:date-and-time
    |  +--ro dscp?                                      inet:dscp {dscp}?
    |  +--ro weighting?                                 uint8 {qos}?
    |  +--ro dependency?                                subscription-id {qos}?
    |  +--ro transport?                                 transport {configured}?
    |  +--ro encoding?                                  encoding
    |  +--ro purpose?                                   string {configured}?
    +---n subscription-resumed
    |  +--ro id    subscription-id
    +---n subscription-started {configured}?
    |  +--ro id                                         subscription-id
    |  +--ro (target)
    |  |  +--:(stream)
    |  |     +--ro (stream-filter)?
    |  |     |  +--:(by-reference)
    |  |     |  |  +--ro stream-filter-name             stream-filter-ref
    |  |     |  +--:(within-subscription)
    |  |     |     +--ro (filter-spec)?
    |  |     |        +--:(stream-subtree-filter)
    |  |     |        |  +--ro stream-subtree-filter?   <anydata> {subtree}?
    |  |     |        +--:(stream-xpath-filter)
    |  |     |           +--ro stream-xpath-filter?     yang:xpath1.0 {xpath}?
    |  |     +--ro stream                               stream-ref
    |  |     +--ro replay-start-time?                   yang:date-and-time {replay}?
    |  |     +--ro replay-previous-event-time?          yang:date-and-time {replay}?
    |  +--ro stop-time?                                 yang:date-and-time
    |  +--ro dscp?                                      inet:dscp {dscp}?
    |  +--ro weighting?                                 uint8 {qos}?
    |  +--ro dependency?                                subscription-id {qos}?
    |  +--ro transport?                                 transport {configured}?
    |  +--ro encoding?                                  encoding
    |  +--ro purpose?                                   string {configured}?
    +---n subscription-suspended
    |  +--ro id        subscription-id
    |  +--ro reason    identityref
    +---n subscription-terminated
       +--ro id        subscription-id
       +--ro reason    identityref
```

- Module Dependencies: `ietf-inet-types` `ietf-interfaces` `ietf-netconf-acm` `ietf-network-instance` `ietf-restconf` `ietf-yang-types`

## 8.121. ietf-subscribed-notif-receivers

```text
module: ietf-subscribed-notif-receivers

  augment /sn:subscriptions:
    +--rw receiver-instances
       +--rw receiver-instance* [name]
          +--rw name    string
          +--rw (transport-type)
  augment /sn:subscriptions/sn:subscription/sn:receivers/sn:receiver:
    +--rw receiver-instance-ref?   -> /sn:subscriptions/snr:receiver-instances/receiver-instance/name
```

- Module Dependencies: `ietf-subscribed-notifications`

## 8.122. ietf-system-aaa

```text
module: ietf-system-aaa

  augment /sys:system:
    +--rw authorization {authorization}?
    |  +--rw user-authorization-order*   identityref
    |  +--rw events
    |     +--rw event* [event-type]
    |        +--rw event-type    identityref
    +--rw accouting {accouting}?
       +--rw user-accouting-order*   identityref
       +--rw events
          +--rw event* [event-type]
             +--rw event-type    identityref
             +--rw record?       enumeration
```

- Module Dependencies: `ietf-system` `ietf-netconf-acm`

## 8.123. ietf-system-capabilities

```text
module: ietf-system-capabilities
  +--ro system-capabilities
     +--ro datastore-capabilities* [datastore]
        +--ro datastore                -> /yanglib:yang-library/datastore/name
        +--ro per-node-capabilities* []
           +--ro (node-selection)?
              +--:(node-selector)
                 +--ro node-selector?   nacm:node-instance-identifier
```

- Module Dependencies: `ietf-netconf-acm` `ietf-yang-library`

## 8.124. ietf-system-dns-resolver

```text
module: ietf-system-dns-resolver

  augment /sys:system/sys:dns-resolver:
    +--rw resource-record* [name]
       +--rw name       string
       +--rw address    inet:ip-address
```

- Module Dependencies: `ietf-inet-types` `ietf-system`

## 8.125. ietf-system

```text
module: ietf-system
  +--rw system
  |  +--rw contact?          string
  |  +--rw hostname?         inet:domain-name
  |  +--rw location?         string
  |  +--rw clock
  |  |  +--rw (timezone)?
  |  |     +--:(timezone-name) {timezone-name}?
  |  |     |  +--rw timezone-name?         timezone-name
  |  |     +--:(timezone-utc-offset)
  |  |        +--rw timezone-utc-offset?   int16
  |  +--rw ntp! {ntp}?
  |  |  +--rw enabled?   boolean
  |  |  +--rw server* [name]
  |  |     +--rw name                string
  |  |     +--rw (transport)
  |  |     |  +--:(udp)
  |  |     |     +--rw udp
  |  |     |        +--rw address    inet:host
  |  |     |        +--rw port?      inet:port-number {ntp-udp-port}?
  |  |     +--rw association-type?   enumeration
  |  |     +--rw iburst?             boolean
  |  |     +--rw prefer?             boolean
  |  +--rw dns-resolver
  |  |  +--rw search*    inet:domain-name
  |  |  +--rw server* [name]
  |  |  |  +--rw name                 string
  |  |  |  +--rw (transport)
  |  |  |     +--:(udp-and-tcp)
  |  |  |        +--rw udp-and-tcp
  |  |  |           +--rw address    inet:ip-address
  |  |  |           +--rw port?      inet:port-number {dns-udp-tcp-port}?
  |  |  +--rw options
  |  |     +--rw timeout?    uint8
  |  |     +--rw attempts?   uint8
  |  +--rw radius {radius}?
  |  |  +--rw server* [name]
  |  |  |  +--rw name                   string
  |  |  |  +--rw (transport)
  |  |  |  |  +--:(udp)
  |  |  |  |     +--rw udp
  |  |  |  |        +--rw address                inet:host
  |  |  |  |        +--rw authentication-port?   inet:port-number
  |  |  |  |        +--rw shared-secret          string
  |  |  |  +--rw authentication-type?   identityref
  |  |  +--rw options
  |  |     +--rw timeout?    uint8
  |  |     +--rw attempts?   uint8
  |  +--rw authentication {authentication}?
  |     +--rw user-authentication-order*   identityref
  |     +--rw user* [name] {local-users}?
  |        +--rw name              string
  |        +--rw password?         ianach:crypt-hash
  |        +--rw authorized-key* [name]
  |           +--rw name         string
  |           +--rw algorithm    string
  |           +--rw key-data     binary
  +--ro system-state
     +--ro platform
     |  +--ro os-name?      string
     |  +--ro os-release?   string
     |  +--ro os-version?   string
     |  +--ro machine?      string
     +--ro clock
        +--ro current-datetime?   yang:date-and-time
        +--ro boot-datetime?      yang:date-and-time

  rpcs:
    +---x set-current-datetime
    |  +---w input
    |     +---w current-datetime    yang:date-and-time
    +---x system-restart
    +---x system-shutdown
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types` `ietf-netconf-acm` `iana-crypt-hash`

## 8.126. ietf-system-tacacs-plus

```text
module: ietf-system-tacacs-plus

  augment /sys:system:
    +--rw tacacs-plus
       +--rw server* [name]
          +--rw name                      string
          +--rw server-type               tacacs-plus-server-type
          +--rw address                   inet:host
          +--rw port?                     inet:port-number
          +--rw (security)
          |  +--:(obfuscation)
          |     +--rw shared-secret?      string
          +--rw (source-type)?
          |  +--:(source-ip)
          |  |  +--rw source-ip?          inet:ip-address
          |  +--:(source-interface)
          |     +--rw source-interface?   if:interface-ref
          +--rw vrf-instance?             -> /ni:network-instances/network-instance/name
          +--rw single-connection?        boolean
          +--rw timeout?                  uint16
          +--ro statistics
             +--ro connection-opens?      yang:counter64
             +--ro connection-closes?     yang:counter64
             +--ro connection-aborts?     yang:counter64
             +--ro connection-failures?   yang:counter64
             +--ro connection-timeouts?   yang:counter64
             +--ro messages-sent?         yang:counter64
             +--ro messages-received?     yang:counter64
             +--ro errors-received?       yang:counter64
             +--ro sessions?              yang:counter64
```

- Module Dependencies: `ietf-inet-types` `ietf-yang-types` `ietf-network-instance` `ietf-interfaces` `ietf-system` `ietf-netconf-acm`

## 8.127. ietf-truststore

```text
module: ietf-truststore
  +--rw truststore
     +--rw certificate-bags! {certificates}?
     |  +--rw certificate-bag* [name]
     |     +--rw name           string
     |     +--rw description?   string
     |     +--rw certificate* [name]
     |        +--rw name                      string
     |        +--rw cert-data                 trust-anchor-cert-cms
     |        +---n certificate-expiration {certificate-expiration-notification}?
     |           +-- expiration-date    yang:date-and-time
     +--rw public-key-bags! {public-keys}?
        +--rw public-key-bag* [name]
           +--rw name           string
           +--rw description?   string
           +--rw public-key* [name]
              +--rw name                 string
              +--rw public-key-format    identityref
              +--rw public-key           binary
```

- Module Dependencies: `ietf-netconf-acm` `ietf-crypto-types`

## 8.128. ietf-yang-library

```text
module: ietf-yang-library
  +--ro yang-library
  |  +--ro module-set* [name]
  |  |  +--ro name                  string
  |  |  +--ro module* [name]
  |  |  |  +--ro name         yang:yang-identifier
  |  |  |  +--ro revision?    revision-identifier
  |  |  |  +--ro namespace    inet:uri
  |  |  |  +--ro location*    inet:uri
  |  |  |  +--ro submodule* [name]
  |  |  |  |  +--ro name        yang:yang-identifier
  |  |  |  |  +--ro revision?   revision-identifier
  |  |  |  |  +--ro location*   inet:uri
  |  |  |  +--ro feature*     yang:yang-identifier
  |  |  |  +--ro deviation*   -> ../../module/name
  |  |  +--ro import-only-module* [name revision]
  |  |     +--ro name         yang:yang-identifier
  |  |     +--ro revision     union
  |  |     +--ro namespace    inet:uri
  |  |     +--ro location*    inet:uri
  |  |     +--ro submodule* [name]
  |  |        +--ro name        yang:yang-identifier
  |  |        +--ro revision?   revision-identifier
  |  |        +--ro location*   inet:uri
  |  +--ro schema* [name]
  |  |  +--ro name          string
  |  |  +--ro module-set*   -> ../../module-set/name
  |  +--ro datastore* [name]
  |  |  +--ro name      ds:datastore-ref
  |  |  +--ro schema    -> ../../schema/name
  |  +--ro content-id    string
  x--ro modules-state
     x--ro module-set-id    string
     x--ro module* [name revision]
        x--ro name                yang:yang-identifier
        x--ro revision            union
        +--ro schema?             inet:uri
        x--ro namespace           inet:uri
        x--ro feature*            yang:yang-identifier
        x--ro deviation* [name revision]
        |  x--ro name        yang:yang-identifier
        |  x--ro revision    union
        x--ro conformance-type    enumeration
        x--ro submodule* [name revision]
           x--ro name        yang:yang-identifier
           x--ro revision    union
           +--ro schema?     inet:uri

  notifications:
    +---n yang-library-update
    |  +--ro content-id    -> /yang-library/content-id
    x---n yang-library-change
       x--ro module-set-id    -> /modules-state/module-set-id
```

- Module Dependencies: `ietf-yang-types` `ietf-inet-types` `ietf-datastores`

## 8.129. ietf-yang-schema-mount

```text
module: ietf-yang-schema-mount
  +--ro schema-mounts
     +--ro namespace* [prefix]
     |  +--ro prefix    yang:yang-identifier
     |  +--ro uri?      inet:uri
     +--ro mount-point* [module label]
        +--ro module                 yang:yang-identifier
        +--ro label                  yang:yang-identifier
        +--ro config?                boolean
        +--ro (schema-ref)
           +--:(inline)
           |  +--ro inline!
           +--:(shared-schema)
              +--ro shared-schema!
                 +--ro parent-reference*   yang:xpath1.0
```

- Module Dependencies: `ietf-inet-types` `ietf-yang-types`

## 8.130. nc-notifications

```text
module: nc-notifications
  +--ro netconf
     +--ro streams
        +--ro stream* [name]
           +--ro name                     ncEvent:streamNameType
           +--ro description              string
           +--ro replaySupport            boolean
           +--ro replayLogCreationTime?   yang:date-and-time

  notifications:
    +---n replayComplete
    +---n notificationComplete
```

- Module Dependencies: `ietf-yang-types` `notifications`

## 8.131. notifications

```text
module: notifications
  +--ro notification
     +--ro eventTime    yang:date-and-time

  rpcs:
    +---x create-subscription
       +---w input
          +---w stream?      streamNameType
          +---w filter?      <anyxml>
          +---w startTime?   yang:date-and-time
          +---w stopTime?    yang:date-and-time
```

- Module Dependencies: `ietf-yang-types`

---

# 9. Abbreviations

| **Term**     | **Definition**                                                   |
| -------- | ------------------------------------------------------------ |
| ADZ      | Attenuation Dead Zone                                        |
| ALLOC-ID | is a 12-bit number that the OLT assigns to an ONU to identify a traffic-bearing entity that is a recipient of upstream bandwidth allocations within that ONU. This traffic-bearing entity is also called T-CONT |
| AN       | Access Node                                                  |
| ANI      | Access Node Interface                                        |
| ANM      | Access Network Map                                           |
| API      | Application Programming Interface                            |
| ARP      | Address Resolution Protocol                                  |
| BBF      | BroadBand Forum                                              |
| BE       | Best Effort                                                  |
| BNG      | Broadband Network Gateway (PON association)                  |
| BRAS     | Broadband Remote Access Server (xDSL association)            |
| BSS      | Business Support System                                      |
| BW       | Bandwidth                                                    |
| CFS      | Customer Facing Service                                      |
| CLI      | Command Line Interface                                       |
| CloudCO  | Cloud Central Office                                         |
| CMTS     | Cable Modem Termination System                               |
| CNCF     | Cloud Native Computing Foundation                            |
| CNF      | Cloud-native (aka container) Network Function                |
| CO       | Central Office                                               |
| CoS      | Class of Service                                             |
| CP       | Control Plane                                                |
| CPE      | Customer Premise Equipment                                   |
| CRUD     | Create, Retrieve, Update, Delete                             |
| CVID     | Customer Virtual Identifier                                  |
| DAA      | Distributed Access Architecture                              |
| DC       | Data Centre                                                  |
| DHCP     | Dynamic Host Configuration Protocol                          |
| DMI      | Diagnostic Monitoring Interface                              |
| DMZ      | DeMilitarized Zone                                           |
| DOCSIS   | Data Over Cable Service Interface Specification              |
| DS       | Downstream                                                   |
| DSCP     | DiffServ Code Point                                          |
| E2E      | End to End                                                   |
| EDZ      | Event Dead Zone                                              |
| EMS      | Element Management System                                    |
| EPON     | Ethernet Passive Optical Network                             |
| ETSI     | European Telecommunications Standards Institute              |
| FDF      | Fibre Distribution Frame                                     |
| FFS      | For Future Study                                             |
| FMA      | Flexible MAC Architecture                                    |
| FTP      | File Transfer Protocol                                       |
| FTTB     | Fibre To The Building                                        |
| FTTC     | Fibre To The Curb                                            |
| FTTCS    | Fibre To The Cell Site                                       |
| FTTdp    | Fibre To The Distribution Point                              |
| FTTH     | Fibre To The Home                                            |
| FTTN     | Fibre To The Node                                            |
| FTTP     | Fibre To The Premise                                         |
| GEM      | GPON Encapsulation Method                                    |
| GEMport  | GPON Encapsulation Method port                               |
| GPON     | Gigabit-capable Passive Optical Network                      |
| gRPC     | Google Remote Procedure Call                                 |
| HNF      | Hybrid Network Function                                      |
| HTTP     | HyperText Transfer Protocol                                  |
| HTTPS    | HTTP operating over SSL                                      |
| IETF     | Internet Engineering Task Force                              |
| IGMP     | Internet Group Management Protocol                           |
| IP       | Internet Protcol                                             |
| IPDR     | IP Data Record                                               |
| IPFIX    | Internet Protocol Flow Information Export                    |
| IPoE     | IP over Ethernet                                             |
| JSON     | JavaScript Object Notation Interchange Format                |
| JWT      | JSON Web Token                                               |
| K8S      | Kubernetes                                                   |
| KPI      | Key Performance Indicator                                    |
| L2TP     | Layer 2 Tunneling Protocol                                   |
| LAN      | Local Area Network                                           |
| LLID     | Logical Link Identifier                                      |
| Lx       | Layer x (x=1,2,3…)                                           |
| M2M      | Machine-to-Machine                                           |
| MAC      | Media Access Control                                         |
| MANO     | Management and Orchestration                                 |
| MCO      | Management and Control Orchestration                         |
| ME       | Managed Entity                                               |
| MP       | Management Plane                                             |
| MSBN     | Multi Service Broadband Network                              |
| MVNO     | Mobile Virtual Network Operator                              |
| NETCONF  | Network Configuration Protocol                               |
| NFS      | Network Facing Service                                       |
| NFVI     | Network Functions Virtualization Infrastructure              |
| NFVO     | Network Functions Virtualization Orchestrator                |
| NNI      | Network to Network Interface                                 |
| NS       | Network Sharing                                              |
| NTP      | Network Time Protocol                                        |
| OAM      | Operations, Administration, and Maintenance                  |
| OAN      | Optical Access Network                                       |
| OAuth    | Open Authentication standard                                 |
| ODN      | Optical Distribution Network                                 |
| OLM      | Optical-Layer Management                                     |
| OLO      | Other Licensed Operator                                      |
| OLT      | Optical Line Terminal                                        |
| OMCI     | ONT Management Control Interface                             |
| ONU      | Optical Network Unit                                         |
| OPM      | Optical Parameter Measurement                                |
| OSS      | Operational Support System                                   |
| OTDR     | Optical Time Domain Reflectrometry                           |
| OTDS     | Optical Test and Diagnostics Subsystem                       |
| OTF      | Optical Test Function                                        |
| OTMF     | Optical Test-Management Function                             |
| Pbit     | Priority bits                                                |
| PDU      | Protocol Data Unit                                           |
| PLOAM    | Physical Layer Operations, Administration and Maintenance    |
| PON      | Passive Optical Network (a point-to-multipoint FTTP architecture) |
| PPP      | Point-to-Point Protocol                                      |
| PPPoA    | Point-to-Point Protocol over ATM                             |
| PPPoE    | Point-to-Point Protocol over Ethernet                        |
| Q-in-Q   | 802.1Q Tunnelling Configuration                              |
| QoS      | Quality of Service                                           |
| REST     | Representational State Transfer                              |
| RESTCONF | REST variant of a NETCONF subset                             |
| RFS      | Resource Facing Service                                      |
| RG       | Residential Gateway                                          |
| R-MACPHY | Remote MAC layer and Physical layer                          |
| RMD      | R-MACPHY Device                                              |
| RPC      | Remote Procedure Call                                        |
| RSSI     | Received Signal Strength Indication                          |
| SA       | Service Assurance                                            |
| SDN      | Software Defined Network                                     |
| SDN M&C  | SDN Management and Control                                   |
| SLA      | Service Level Agreement                                      |
| SN       | Service Node                                                 |
| SNI      | Service Node Interface                                       |
| SNMP     | Simple Network Management Protocol                           |
| SP       | Service Provider                                             |
| SSH      | Secure SHell protocol                                        |
| SSL      | Secure Sockets Layer                                         |
| SVID     | Service Virtual Identifier                                   |
| SYSLOG   | Message Logging standard                                     |
| T-CONT   | Traffic Container                                            |
| TCP      | Transmission Control Protocol                                |
| TFP      | Trivial File Transfer Protocol                               |
| TLS      | Transport Layer Security protocol (formerly known as SSL)    |
| TR       | Technical Report                                             |
| UDP      | User Datagram Protocol                                       |
| UNI      | User to Network Interface                                    |
| UP       | User Plane                                                   |
| US       | Upstream                                                     |
| UUID     | Universally Unique IDentifier                                |
| vAN      | Virtual Access Node                                          |
| vBNG     | Virtual Broadband Network Gateway                            |
| VID      | Virtual Identifier                                           |
| VIM      | Virtualizatio Infrastructure Manager                         |
| VLAN     | Virtual LAN                                                  |
| VNF      | Virtual Network Function                                     |
| VNFM     | Virtualized Network Function Manager                         |
| VNO      | Virtual Network Operator                                     |
| VXLAN    | Virtual Extensible LAN                                       |
| WAN      | Wide Area Network                                            |
| WDM      | Wavelength Division Multiplexing                             |
| WG       | Working Group                                                |
| XGPON    | neXt-Generation Passive Optical Network                      |
| XGS-PON   | neXt-Generation Symmetric Passive Optical Network           |
| XML      | Extensible Markup Language                                   |
| XNF      | Any Network Function                                         |
| YAML     | YAML Ain’t Markup Language                                   |

---

# 10. Terminology

| **Term**     | **Definition**                                                   |
| -------- | ------------------------------------------------------------ |
| ODN      | Optical Distribution Network: The physical medium that connects an OLT to its subtended ONUs. The ODN is comprised of various passive components, including the optical fibre, splitter or splitters, and optical connectors. |
| OLT      | Optical Line Terminal (OLT): A device that terminates the common  (root) endpoint of an ODN, implements a PON protocol, and adapts PON PDUs for uplink communications over the provider service interface. The OLT provides management and maintenance functions for the subtended ODN and ONUs. |
| ONU      | Optical Network Unit (ONU): A generic term denoting a functional element that terminates any one of the distributed (leaf) endpoints of an ODN, implements a PON protocol, and adapts PON PDUs to subscriber service interfaces. In some contexts, an ONU supports interfaces for multiple subscribers |
| PON      | Passive Optical Network. A PON includes the OLT, ONU, and Optical Distribution Network (ODN). |
| NETCONF  | IETF XML-based management protocol that is realized on top of a RPC layer using an XML encoding and provide a basic set of operations to edit and query configuration on a network device |
| VNF      | VNF is used to refer to a network function that is virtualized. In this guide, a VNF can be a virtualized function that resides within a virtual environment or in a containerized environment (CNF). For XGS-PON, a typical VNF would be the BNG and labelled as vBNG. |
| YANG     | A data modelling language using in the management of network resources. |

---

