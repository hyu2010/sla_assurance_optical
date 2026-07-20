---
title: "A YANG Data Model for Service Level Agreement (SLA) Assurance Management in Optical Transport Networks"
abbrev: "SLA Assurance YANG"
category: std

docname: draft-yu-ccamp-sla-assurance-optical-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "Common Control and Measurement Plane"
keyword:

- next generation
- unicorn
- sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "hyu2010/sla_assurance_optical"
  latest: "https://hyu2010.github.io/sla_assurance_optical/draft-yu-ccamp-sla-assurance-optical-yang.html"

author:

-
    fullname: Henry Yu
    organization: Huawei
    email: hyu2010b@gmail.com
-
    fullname: Xiao Li
    organization: Huawei
    email: lixiao33@huawei.com
-
    fullname: Yanxia Tan
    organization: China Unicom
    email: tanyx11@chinaunicom.cn
-
    fullname: Xing Zhao
    organization: CAICT
    email: zhaoxing@caict.ac.cn

normative:

  ITU-T_G.709:
    title: Interfaces for the optical transport network
    author:
      org: International Telecommunication Union
    date: 2020-06
    seriesinfo:
      ITU-T: G.709/Y.1331 (2020)
    target: <https://www.itu.int/rec/T-REC-G.709>

informative:

--- abstract

This document defines a YANG module for SLA assurance management in
optical transport networks. The module provides a standard way to
define, detect, and report issues that may impact service and network
availability. It enables consistent modeling of assurance intent,
impairment detection, and risk reporting across optical transport
domains. The YANG model is designed to support closed-loop operations,
allowing automated monitoring, analysis, and remediation workflows to
maintain high service reliability and SLA compliance

--- middle

# Introduction

Service Level Agreement (SLA) assurance management in optical transport networks concerns the continuous monitoring, analysis, and management of network conditions that may impact the performance, availability, and reliability of services. Its objective is to detect potential issues proactively, diagnose their root causes, and initiate corrective actions to prevent SLA violations. Several standards already exist in this area. SAIN {{?RFC9417}} and {{!RFC9418}} defines an architecture and a YANG model for network service assurance, respectively. {{!I-D.ietf-nmop-network-incident-yang}} defines a YANG model for the network incident lifecycle management. It aims to provide a standard way to report, diagnose, and help resolve network incidents which may cause SLA violations.

However, several gaps remain in the existing standards and their associated YANG models with respect to supporting SLA assurance management in optical transport networks. First, the SAIN framework focuses on the detection and resolution of symptoms, that is, conditions indicating that a service instance or subservice is not operating in a fully healthy state. In optical transport networks, however, undesirable network conditions may exist that increase the risk of SLA violations while remaining asymptomatic. For example, multiple service paths may traverse fiber links deployed within the same trench, thereby introducing a common point of failure. Such conditions do not necessarily result in observable service degradation and, therefore, may not be identified through symptom-based monitoring mechanisms.

In addition, SLA assurance monitoring, in some cases, needs to detect and report events defined by SLA policies, such as bandwidth utilization exceeding a specified threshold. The threshold values are defined by the user, reflecting user’s perception of desired network conditions. The occurrence of such events does not necessarily indicate the presence of an incident affecting service health. Instead, event reporting may serve as a precautionary mechanism requested by the user. A mechanism and a YANG model are therefore needed to support the detection and reporting of such events.

Thirdly, some optical network conditions, such as fiber attenuation, may degrade silently and gradually before resulting in observable incidents. These conditions may persist for an extended period of time. For SLA assurance management in optical networks, it is preferable to define, detect, and resolve such issues proactively rather than waiting for them to escalate into incidents.

To address these optical network specific challenges, an issue-centric SLA assurance solution is specified. An issue is an optical network condition that, if unaddressed, may lead to SLA violations and incidents.

This document defines a YANG data model for SLA assurance management in optical networks. The model defines optical network SLA assurance issues and their corresponding resolutions.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are defined in {{!RFC8632}}, {{!RFC9543}}, {{!I-D.ietf-nmop-terminology}} and are not redefined here:

- Event
- Incident
- Anomaly
- Cause
- Characteristic
- SLA (Service Level Agreement)
- SLO (Service Level Objective)

The following terms are defined in this document:

SLA assurance issue:
: A network condition that, if not addressed in time, may lead to SLA violations of services and incidents. An issue may either be symptomatic or asymptomatic. Symptomatic issues cause visible impacts to the health of services. Asymptomatic issues, in contrast, do not impact services alone. They lead to SLA violations in conjunction with other conditions or events, such as a service reroute.

SLA assurance issue type:
: A class used for the classification of SLA assurance issues. Every issue belongs to a certain type. There is a limited number of the types of assurance issues in optical networks.

SLA assurance capability:
: The ability to detect SLA assurance issues and initiate preemptive actions to prevent SLA violations. This ability is measured against how many different types of issues the SLA assurance system can handle.

SLA assurance issue classification:
: The process to identify the type of an issue when it is raised. It is an SLA assurance capability.

Issue Server:
: An entity that is responsible for detecting, classifying, and reporting SLA assurance issues, and performing issue solutions to prevent SLA violations.

Fiber same cable:
: A pair of optical fibers traverse the same physical cable sheath (i.e., both fibers are contained within the same fiber cable).

Fiber same trench:
: A pair of optical fibers are deployed within the same trench/duct route (i.e., both fibers share the same underground pipeline or trench corridor).

# Sample Use Cases

## Proactive resilience assurance for high-availability business services

Modern optical transport networks underpin mission-critical enterprise and carrier-grade services that require continuous availability and strict SLA compliance. This use case focuses on proactively assuring network resilience to prevent service degradation and minimize the impact of potential failures.

In this scenario, the optical transport network continuously monitors key performance indicators such as signal quality, optical power levels, latency, and error rates across wavelengths and optical links, as well as ODUk connections. Advanced analytics and telemetry are used to detect early signs of degradation, including fiber attenuation, amplifier drift, or emerging congestion patterns.

When potential risk conditions are identified, the system proactively evaluates network topology and service dependencies to assess impact on high-availability business services. If a risk of SLA violation is detected, automated or semi-automated remediation actions are triggered. These may include traffic rerouting via pre-provisioned protection paths, wavelength re-optimization, or activation of restoration mechanisms in the optical layer.

The objective is to ensure near-zero interrupted service delivery (surpassing the five-nines availability) for critical applications, such as financial trading, cloud connectivity, and real-time enterprise communications. By shifting from reactive fault management to proactive resilience assurance, operators can significantly reduce downtime, improve SLA adherence, and enhance overall customer experience in optical transport networks.

## Deterministic Latency Assurance for Latency-Sensitive Services

Deterministic SLA assurance in optical transport networks is a use case that aims to ensure stable and predictable service performance for latency-sensitive applications, such as AI distributed training, telemedicine, industrial automation, and immersive XR services. These applications require stringent guarantees on end-to-end latency, jitter, packet loss, bandwidth availability, and service continuity.

In this use case, an optical transport service is provisioned with explicit SLA objectives that define the required deterministic performance characteristics. During service operation, the network continuously monitors both the service state and the underlying optical transport infrastructure through real-time telemetry collection. The monitored information may include end-to-end path latency, optical link quality, bandwidth utilization, wavelength continuity, congestion conditions, and other transport performance indicators relevant to SLA fulfillment.

As network conditions dynamically evolve, the system identifies conditions that may threaten the SLA commitments of active services before visible service degradation occurs. Such conditions may include increasing congestion, abnormal latency variation, optical signal degradation, or predicted failures affecting the active transport path. The network then evaluates alternative optical transport paths and service optimization options capable of preserving the required SLA characteristics.

The objective of this use case is to maintain continuous SLA compliance throughout the service lifecycle by enabling adaptive transport behavior in response to changing network conditions. This improves service reliability, minimizes performance instability and disruption, and enhances the capability of optical transport networks to support mission-critical and performance-sensitive applications.

## Dynamic Bandwidth Allocation for Cloud-Network Synergy

Cloud–Network Synergy-Based Dynamic Bandwidth Allocation is a use case in optical transport networks that enables adaptive allocation of transport resources based on coordinated demands from cloud applications and network conditions. It targets service scenarios such as AI training workloads, distributed computing, cloud bursting, and large-scale data synchronization, where bandwidth demand is highly dynamic and time-varying.

In this use case, cloud applications continuously generate workload signals that reflect their resource requirements, including bandwidth demand, traffic intensity, latency sensitivity, and service priority. These requirements are communicated to the optical transport network through a cloud–network interface. At the same time, the optical transport network maintains real-time visibility of available capacity, including wavelength availability, link utilization, path constraints, and operational state across the optical infrastructure.

Based on the correlation between cloud-side demand signals and network-side resource state, the system determines when and where bandwidth adjustments are required. When increased demand is detected, the network evaluates feasible optical path and wavelength allocation options that satisfy service constraints. Conversely, when demand decreases, allocated resources may be released or re-optimized to improve overall network efficiency.

The objective of this use case is to achieve efficient and elastic utilization of optical transport resources while ensuring that cloud application performance requirements are consistently met. By enabling coordinated decision-making between cloud workloads and optical network control, the system improves resource utilization, reduces congestion by dynamic bandwidth control of hard-pipes, and enhances the responsiveness of the transport network to dynamic application demands.

# SLA assurance issues

As shown in {{tab-1}}, SLA assurance issues in optical networks can be classified, based on their impact, into three categories: (1) those affecting service bandwidth, (2) those affecting network and service availability, and (3) those introducing traffic delay.

<table anchor="tab-1">
    <name>SLA Assurance Issues</name>
    <thead>
      <tr>
        <td>Issue category</td>
        <td>SLA assurance issues</td>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td rowspan="3">Bandwidth</td>
        <td>Traffic spike (sudden increase in traffic)</td>
      </tr>
      <tr>
        <td>Utilization exceeds threshold defined in the SLA</td>
      </tr>
      <tr>
        <td>Network bit error rate / packet loss rate higher than allowed threshold</td>
      </tr>
      <tr>
        <td rowspan="8">Network and service availability</td>
        <td>Service outage</td>
      </tr>
      <tr>
        <td>Transient service interruption</td>
      </tr>
      <tr>
        <td>Failover protection failure</td>
      </tr>
      <tr>
        <td>Protection on the same route</td>
      </tr>
      <tr>
        <td>Protection on the same route</td>
      </tr>
      <tr>
        <td>Risk of traffic rerouting (during failover/reconvergence)</td>
      </tr>
      <tr>
        <td>Service performance degradation</td>
      </tr>
      <tr>
        <td>customer premises equipment (CPE) access vulnerability/hazard</td>
      </tr>
      <tr>
        <td rowspan="2">Delay</td>
        <td>Operating latency exceeds allowed limit</td>
      </tr>
      <tr>
        <td>Protection switching latency exceeds allowed limit</td>
      </tr>
    </tbody>
  </table>

Most SLA assurance issues, if left unaddressed, are expected to eventually result in SLA violations and incidents. However, exceptions exist. For example, an issue indicating that bandwidth utilization has exceeded an SLA-defined threshold does not necessarily imply the presence of a service health problem or any network equipment failures, particularly when the configured threshold provides a substantial operational margin. In such cases, the impact analysis included in the issue report SHOULD indicate that no immediate service impact is expected. The report MAY also recommend that the user review and adjust the threshold value to better align with the intended monitoring objectives.

Issues that could eventually lead to SLA violations may be either symptomatic or asymptomatic. For symptomatic issues, the associated symptoms are observable at the service layer and indicate degradation in the health of one or more services. In contrast, asymptomatic issues may not manifest as observable symptoms in any affected service. Despite this distinction, the underlying causes of both types of issues may originate from abnormal conditions in the underlay optical transport network that provides connectivity for the services. More specifically, there are three major sources of possible root causes spanning in the three layers of transport networks: (1) abnormal conditions in optical fibers, (2) error conditions in optical WDM services, and (3) bit errors in OTN payloads. SLA assurance analysis must provide accurate diagnosis of these issues and identify their underlying causes.

It should also be noted that certain SLA terms permit temporary service outages or transient service interruptions. This is common, for example, for best-effort services or services without protection mechanisms. In such cases, the distinction between issues and incidents may become less clear, and the two concepts may overlap. Accordingly, issue management MAY defer the handling of such cases to incident management processes.

Each issue is described in more detail in the following subsections.

## Bandwidth Issues

Bandwidth issues are service assurance conditions that impact the capacity, throughput, or traffic-carrying efficiency of optical transport networks.

### Traffic spike (sudden increase in traffic)

This issue refers to the network condition that the customer-side interface receives a large volume of burst traffic within an extremely short period of time (on the order of milliseconds). When the instantaneous rate of microburst traffic exceeds the device’s forwarding capacity, the device buffers the burst traffic for later transmission. If the device does not have sufficient buffer space, the excess traffic is dropped, resulting in congestion and packet loss.

### Bandwidth utilization exceeds threshold defined in the SLA

This issue refers to the network condition that the customer-side service traffic exceeds the user-defined threshold, posing a risk of bandwidth overutilization. This issue can be quantified as the percentage of cumulative time during a specified period in which the service bandwidth utilization exceeds the SLA-defined threshold.

### Network bit error rate (or packet loss rate) higher than allowed threshold

This issue refers to the problem that the packet loss on the customer side causes the available bandwidth to fall short of the user’s requirements.

Possible cause at the OTN layer: Bit errors are being generated at the electrical-layer service level, specifically within digital layer channels (such as ODUk and OTUk), indicating that the transmission at the digital wrapper/virtual container layer is experiencing signal corruption. This may result from impairments in the physical transmission path or intermediate equipment, and can lead to degraded service quality or potential data integrity issues at the service layer.

Possible cause at the DWDM layer: Corrected bit errors are occurring at the optical-layer (OCh) service level, indicating that the optical channel is experiencing transmission impairments that require error correction mechanisms to recover affected data. While forward error correction (FEC) is able to detect and correct these errors, their presence suggests degradation in optical signal quality, which may be caused by factors such as optical attenuation, noise, dispersion, or aging/impairment of the transmission path, and could eventually lead to uncorrectable errors if conditions worsen.

Possible cause at the fiber layer: The logical fiber path traversed by the service is experiencing degradation in terms of attenuation, indicating increased optical loss along the transmission route. This deterioration in signal strength may be caused by fiber aging, connector or splice losses, bending losses, or other impairments in the optical path, and can negatively impact signal quality and overall service performance.

## Network and service availability issues

Network and service availability issues are service assurance conditions that impact the ability of optical transport networks to maintain continuous and reliable service delivery.

### Service outage

The service experiences either a network-side interruption or a customer-side interruption, resulting in a loss of connectivity or service availability.

Possible cause at the OTN layer: The service experiences an interruption at the electrical-layer service level (ODUk/OTUk/VC), indicating a loss of continuity in the digital wrapper/virtual container transport layer. This type of failure typically results in a complete break in the ODUk/OTUk signal path, causing service disruption and potential traffic loss across the affected connection, and may be triggered by transport defects, equipment failures, or upstream/downstream signal degradation.

Possible cause at the DWMD layer: The service experiences an interruption at the optical (OCh/OMS/OTS) service level, indicating a loss of continuity in the optical channel transport path. This results in a complete failure of the optical (OCh/OMS/OTS) signal transmission, leading to service outage and traffic disruption. Such an interruption may be caused by severe optical impairments, fiber cuts, transmitter/receiver failures, or failures in optical switching elements along the transmission path.

Possible cause at the fiber medium layer: The logical fiber path traversed by the service experiences an interruption, indicating a complete loss of continuity along the virtualized or logical optical transport route. This results in a service outage due to the inability of the end-to-end path to maintain signal transmission. Such an interruption may be caused by failures in underlying physical fiber links, intermediate network elements, or logical path provisioning issues within the optical transport network.

### Transient service interruption

Service experiences momentary interruptions. For instance, optical power fluctuations in the fiber may cause short bursts of bit errors or packet loss at higher layers, also leading to transient service interruptions.

Possible cause at the OTN layer: The service’s electrical-layer (ODU/OSU) working path has switched to a protection path, resulting in a brief service interruption (service hit or transient outage). This switchover indicates that a fault or degradation was detected on the original working route, triggering protection mechanisms to reroute traffic; however, the switching process introduces a short-lived traffic disruption during path convergence.

Possible cause at the DWDM layer: The OCh service’s optical-layer working path has switched to a protection path, resulting in a brief service interruption (transient outage or service hit). This indicates that degradation or a failure was detected on the original optical channel working route, triggering protection mechanisms to reroute traffic. During the switching and path convergence process, a short-lived disruption may occur, leading to a momentary service blip before normal transmission resumes on the protection path.

Possible cause at the fiber layer: Abnormal optical power fluctuations occur on the fiber path traversed by the service, causing transient instability in the received signal level. These rapid variations in optical power can lead to momentary loss of signal integrity, triggering a brief service interruption (service hit or flash outage). Such behavior is typically associated with physical-layer impairments such as connector issues, micro-bending, fiber movement, or unstable optical components along the transmission path.

### Failover protection failure

This issue may rise when the protection route of the service experiences an interruption, resulting in protection failure. This indicates that the backup (protection) path is unavailable or has lost continuity, preventing the protection mechanism from successfully taking over in the event of a working path fault. As a result, the service is left without effective redundancy, increasing the risk of service disruption or prolonged outage if a failure occurs on the active path.

Possible cause at the OTN layer: The protection route at the electrical-layer (ODU/OTU/VC) level experiences an interruption, indicating a loss of continuity in the backup ODU/OUT/VC path. As a result, the protection path becomes unavailable, preventing it from serving as a valid failover for the working route in the event of a failure. This condition reduces service resilience and increases the risk of service disruption if the primary electrical-layer path degrades or fails.

Possible cause at the DWDM layer: The protection route at the optical-layer (OCh) experiences an interruption, meaning the backup OCh (or OCh protection path) has lost continuity or become unavailable. As a result, the optical protection mechanism cannot successfully take over traffic in the event of a failure on the working channel, thereby reducing service resilience and increasing the risk of service outage under fault conditions.

Possible cause at the fiber layer: The protection fiber traversed by the service—referring to the logical fiber path not currently used by the working route—experiences an interruption, indicating a loss of continuity on the backup transport path. Since this logical fiber represents the protection route, its failure renders the protection mechanism unavailable for failover, thereby reducing overall service resilience and increasing the risk of service disruption if the working path subsequently fails.

### Protection on the same route

The working and protection paths of the service partially overlap, such as sharing common nodes or fiber links within the same Shared Risk Link Group (SRLG), which may compromise the effectiveness of the protection mechanism in case of a failure.

Possible cause at the OTN layer: The service’s electrical-layer (ODU/VC) working and protection paths share a common SRLG (Shared Risk Link Group), as identified through protection segment analysis. This indicates that the supposedly redundant primary and backup routes are not fully disjoint and may be simultaneously affected by a single underlying failure domain (e.g., shared fiber segment, duct, node, or equipment). As a result, the effectiveness of protection is compromised, increasing the risk of concurrent working and protection path failure and potential service outage under fault conditions. Similarly, the issue may also rise when the service’s electrical-layer working and protection paths traverse the same network element, indicating that both primary and backup routes are not node-disjoint. This introduces a shared risk at the equipment level, meaning that a single failure or maintenance event on that network element could simultaneously impact both working and protection paths.

Possible cause at the DWDM layer: The service’s optical-layer (OCh) working and protection paths share a common SRLG, indicating that the primary and backup routes are not fully physically diverse. This means both paths may traverse a shared risk element—such as a fiber segment, conduit, node, or optical equipment—creating a single point of correlated failure. As a result, the protection scheme’s effectiveness is reduced, increasing the risk that a single failure could simultaneously impact both working and protection OCh paths, potentially leading to service outage. The issue may also rise if the OCh working and protection paths pass through the same network element (NE). This creates a shared risk at the equipment level, where a single device failure, software fault, or maintenance activity on that network element could simultaneously impact both working and protection optical channels.

Possible cause at the fiber layer: Fiber same cable and fiber same trench scenarios may also lead to a “protection on the same route” issue, where the working and protection paths are no longer truly physically diverse. In such cases, although logical routing appears to provide redundancy, both fiber paths share the same underlying physical infrastructure—either within the same cable or the same trench/duct corridor. This creates a shared risk domain, meaning that a single physical failure event (such as cable cut, trench excavation damage, or environmental impact) can simultaneously affect both the working and protection fibers. As a result, the protection mechanism may fail to provide effective redundancy, significantly increasing the risk of concurrent path failure and resulting service interruption.

### Risk of traffic rerouting (during failover/reconvergence)

A service may be exposed to an increased risk of SLA violations during network failover or routing reconvergence events. Although protection switching and restoration mechanisms are designed to maintain service continuity, the newly selected path may exhibit different characteristics from the original path, such as increased latency, reduced available bandwidth, additional optical impairments, or a higher detour factor. These changes may cause one or more SLA performance objectives to be approached or exceeded, even if the service remains operational. Accordingly, the Issue Server SHOULD identify services that are susceptible to such rerouting events, assess the potential impact on SLA compliance, and report the associated risk before or immediately after the rerouting occurs, thereby enabling proactive operational actions to mitigate potential SLA violations.

Possible cause at the OTN layer: TBD

Possible cause at the DWDM layer: TBD

Possible cause at the fiber layer: TBD

### Service performance degradation

The service experiences performance degradation, indicating a decline in end-to-end service quality compared to normal operating conditions. One example could be the degradation in Optical Channel (OCH) performance - performance degradation at the optical layer, such as post-FEC errors, may cause impairments at the service level (e.g., bit errors and packet loss).

Possible cause at the OTN layer: The service experiences performance degradation at the electrical-layer (ODU/VC) service level, typically manifested as error-related performance counters or alarms. This indicates that the ODU/VC transport layer is suffering from signal quality deterioration, leading to increased bit errors, errored seconds, or other degradation indicators. Such conditions are often associated with impairments in the underlying transmission path, equipment issues, or signal interference, and may degrade service quality even if the connection remains up.

Possible cause at the DWDM layer: Performance degradation occurs at the optical-layer (OCh) service level, typically manifested as pre-FEC bit error rate (BER) threshold violations or related performance degradations. This indicates that the optical channel is experiencing signal quality deterioration before forward error correction is applied, suggesting impairments such as optical noise, attenuation, dispersion, or aging of optical components. Although FEC may still be able to recover the transmitted data, persistent or worsening pre-FEC error conditions can reduce transmission margins and may eventually lead to uncorrectable errors or service interruption.

Possible cause at the fiber layer: The logical fiber path experiences performance degradation, typically indicated by fiber attenuation exceeding the defined threshold (for example, surpassing the designed End-of-Life (EOL) attenuation limit). This suggests that the optical transmission path is suffering from excessive signal loss, which may be caused by fiber aging, connector or splice degradation, bending losses, contamination, or other physical impairments. Excessive attenuation reduces optical power margins and can negatively impact signal quality, increasing the risk of transmission errors, instability, or eventual service interruption.

### CPE access vulnerability/hazard

This issue refers to the problem that the protection configuration of the service access segment does not match the required availability grade, or the working and protection paths within the access segment share the same SRLG. This indicates that the access-side protection design does not provide the level of resilience required by the service SLA, either because the configured protection mechanism is insufficient or because the primary and backup access paths are not fully risk-disjoint. As a result, a single physical or logical failure event may simultaneously affect both working and protection paths, reducing service availability and increasing the risk of service interruption.

Possible cause at the OTN layer: The protection configuration of the access segment does not match the configured service availability grade. For example, the access segment is deployed with a single uplink architecture, while the service availability requirement is configured for dual-uplink protection. This mismatch indicates that the actual redundancy capability of the access network is insufficient to meet the intended SLA or resilience target. As a result, the service may not achieve the expected availability level, and a single uplink failure could directly lead to service interruption.

Possible cause at the DWDM layer: TBD

Possible cause at the fiber layer: There is a shared SRLG (Shared Risk Link Group) risk between the working and protection paths of the access segment, indicating that the primary and backup access routes are not fully physically or logically diverse. Both paths may rely on the same underlying risk domain—such as shared fiber infrastructure, conduit, node, or access equipment—meaning that a single failure event could simultaneously impact both working and protection connections. As a result, the effectiveness of the access protection mechanism is reduced, increasing the risk of service interruption and lowering overall service resilience.

## Latency issues

Latency issues are service assurance conditions that affect the delay experienced by traffic traversing optical transport networks. These issues may be introduced by suboptimal routing, increased path length due to protection switching, congestion in optical or OTN layers, or processing delays within network elements.

### Operating latency exceeds allowed limit

The service’s current working latency exceeds the defined threshold, indicating that the end-to-end transmission delay is out of the acceptable range. This may be due to latency threshold violations or an excessive detour factor (route inefficiency), where traffic is forced onto a longer-than-optimal path.

Possible causes at the OTN layer: The service may experience a latency-related SLA issue due to changes in the routing characteristics of the working electrical-layer (ODU) service path. For example, an electrical-layer path switch may result in the newly selected working path exhibiting a higher end-to-end latency than the original route, causing the measured latency to exceed the configured threshold. In addition, the new routing may introduce a detour factor violation. The detour factor is defined as the ratio of the measured or estimated latency to the ideal latency, where the ideal latency is calculated as the shortest-path distance in the network topology multiplied by the fiber latency coefficient. If the actual routing deviates significantly from the optimal shortest path, the resulting increase in path length and transmission delay may cause the detour factor to exceed the acceptable limit. Both conditions indicate that the working ODU service path no longer satisfies the latency-related constraints specified by the SLA.

Possible causes at the DWDM layer: The service may experience a latency-related SLA issue due to changes in the routing characteristics of the working optical path. For example, an optical-layer path switch may result in the newly selected working path exhibiting a higher end-to-end latency than the original route, causing the measured latency to exceed the configured threshold. In addition, the newly established route may lead to a detour factor violation on the working logical fiber. The detour factor is defined as the ratio of the measured or estimated latency to the ideal latency, where the ideal latency is calculated as the shortest-path distance between the two endpoints in the network topology multiplied by the fiber latency coefficient. If the routing of the working logical fiber deviates significantly from the optimal shortest path, the resulting increase in transmission distance and delay may cause the detour factor to exceed the acceptable limit. Both conditions indicate that the current optical-layer routing no longer satisfies the latency-related constraints specified by the SLA.

Possible causes at the fiber layer: The service may experience a latency-related SLA issue following a fiber cut along the optical path that triggers a routing switch. As part of the failure recovery process, the rerouted working path may exhibit higher end-to-end latency than the original route, causing the measured latency to exceed the configured threshold. In addition, the rerouted working logical fiber may exhibit a detour factor violation. The detour factor is defined as the ratio of the measured or estimated latency to the ideal latency, where the ideal latency is calculated as the shortest-path distance in the network topology multiplied by the fiber latency coefficient. If the rerouted logical fiber deviates significantly from the optimal shortest path, the resulting increase in transmission distance and delay may cause the detour factor to exceed the acceptable limit. Both conditions indicate that the post-recovery routing no longer satisfies the latency-related constraints specified by the SLA.

### Protection switching latency exceeds allowed limit

The service’s current protection latency exceeds the defined threshold, indicating that the end-to-end delay on the protection path is out of the acceptable range.

Possible cause at the OTN layer: The service experiences a detour factor violation on the protection electrical-layer (ODU) path. The detour factor is defined as the ratio of measured or estimated latency to ideal latency, where ideal latency is calculated as the shortest network path distance between two points multiplied by the fiber latency coefficient. In this case, the protection ODU routing deviates significantly from the optimal shortest path, resulting in excessive transmission delay and causing the detour ratio to exceed the acceptable threshold.

Possible cause at the DWDM layer: The service experiences a detour factor violation on the protection optical-layer (OCh) path. The detour factor is defined as the ratio of measured or estimated latency to ideal latency, where the ideal latency is calculated as the shortest network path distance between two points multiplied by the fiber latency coefficient. In this case, the protection OCh routing significantly deviates from the optimal shortest path, resulting in increased transmission delay and causing the detour ratio to exceed the acceptable threshold.

Possible cause at the fiber layer: The service experiences a detour factor violation on the protection logical fiber path. The detour factor is defined as the ratio of measured or estimated latency to ideal latency, where the ideal latency is calculated as the shortest path distance between two points in the network topology multiplied by the fiber latency coefficient. In this case, the protection logical fiber route significantly deviates from the optimal shortest path, resulting in increased transmission delay and causing the detour ratio to exceed the acceptable threshold.

# SLA Assurance Issue Management Architecture and SLA Assurance Capability

~~~~ ascii-art
+-------------------------------------------------------------+
|                             OSS                             |
+-------------------------------------------------------------+
       ^                                             |
       | issue report                                | issue query
       |                                             v
+-------------------------------------------------------------+
|                         Issue Server                        |
+-------------------------------------------------------------+
       ^                ^                ^                ^
       |                |                |                |
    alarm/     Network performance  Network Diagnosis    Log
    event       Metrics/Telemetry    using OAM Test,
                                           OTDR
       |                |                |                |
+-------------------------------------------------------------+
|                      Transport Network                      |
+-------------------------------------------------------------+
~~~~
{:#fig-arch title="SLA Assurance Issue Management Architecture"}

{{fig-arch}} illustrates the SLA assurance issue management architecture. The main component for issue management is the Issue Server, which provides capabilities for issue identification and classification, issue reporting, and issue querying. The Issue Server can be deployed on controllers as defined in {{?RFC8969}} within each network domain and interfaces with the OSS. A typical workflow is as follows:

- The Issue Server derives monitoring policies from the SLA, including the events, performance metrics, and alarms that need to be monitored, and initiates the corresponding monitoring activities.

- Upon detecting an issue, the Issue Server performs issue classification, impact analysis, and resolution generation, and reports the issue to the OSS.

- The OSS can query the Issue Server to retrieve information about identified issues, including their status, impact assessment, and recommended resolutions.

- The OSS can invoke RPC operations exposed by the Issue Server to initiate issue resolution.

It should be noted that the SLA assurance capabilities supported by Issue Servers may vary across implementations and products. Therefore, an Issue Server SHOULD provide interfaces that expose its supported capabilities and enable external systems to query such capability information.

To support optical networks SLA assurance management, a unified data model is required for standardized representation. The model should support classification (issue-sla-type) and hierarchical layering (layer), where the layering follows the network architecture defined in {{ITU-T_G.709}}.

Specifically, the optical layer includes OTS, OMS, and OCh, while the electrical layer includes OTUk and ODUk (including both higher-order and lower-order ODUk). The model should also incorporate severity labeling (severity), explicit identification of affected objects (source-objects), impacted services or objects (impacted-objects), and temporal tracking attributes such as occurrence time (occur-time) and clearance time (clear-time). In addition, each impairment instance should include a detailed description (description) and corresponding remediation or handling recommendations (suggestion), enabling consistent lifecycle tracking, analysis, and operational decision-making.

## Interworking with SAIN

~~~~ ascii-art
    +-------------------------+
    | Assurance Issue Handler |
    +-------------------------+
                 ^
                 | issue
                 |
    +-------------------------+
    | Assurance Issue Process |
    +-------------------------+
      ^                     ^
      |                     | symptoms
      | Asymptomatic        |
      | network      +------+--+
      | metrics      |  SAIN   |
      |              +------+--+
      |                     ^
      |                     | Service
      |                     | metrics
    +-+---------------------+--+
    |    Network in the        |
    |   Autonomous Domain      |
    +--------------------------+
~~~~
{:#fig-sain title="Interworking with SAIN"}

SAIN {{?RFC9417}} defines an architecture for network service assurance and specifies mechanisms for detecting symptoms associated with service degradation. For symptomatic issues, issue lifecycle management can leverage the capabilities provided by SAIN to support issue detection, analysis, and handling. However, SAIN does not address asymptomatic issues, since such issues do not manifest as observable service-level symptoms. Therefore, the management of asymptomatic issues requires issue lifecycle management functions to interact directly with the underlying physical network resources and associated monitoring mechanisms.

## Relationship with Incident

The objective of SLA assurance issue management is to prevent incidents, or reduce their occurrence, by identifying and resolving issues before they escalate into incidents. However, incidents cannot be fully prevented by issue management for several reasons. First, certain SLA terms permit temporary service outages or transient service interruptions, in which case issue handling is typically delegated to incident management. Second, an Issue Server may have limited SLA assurance capabilities, which may prevent it from detecting certain classes of issues. Therefore, issue management and incident management are complementary functions.

## Interworking with ACTN framework

The issue lifecycle management framework can interwork with the Abstraction and Control of Traffic Engineered Networks (ACTN) framework to support end-to-end service assurance across multi-domain transport networks. In particular, ACTN provides a hierarchical control architecture that enables coordination between customer-facing service requests and underlying network resources, while issue lifecycle management provides mechanisms for detecting, classifying, and handling SLA assurance issues.

In an integrated deployment, the Issue Server may be instantiated within or alongside ACTN controllers in each domain, enabling correlation of service-level issues with underlying network conditions across multiple technology layers and administrative domains. ACTN’s provision of abstracted network views can be leveraged by issue management functions to localize the impact of detected issues and to facilitate cross-domain impact analysis.

Furthermore, issue-related information, including detected issues, their classifications, and resolution recommendations, may be exposed through ACTN interfaces to support coordinated remediation actions. Conversely, ACTN control and orchestration functions may provide topology, connectivity, and service intent information that enhances issue detection accuracy and improves root cause localization.

# SLA Assurance Data Model Design

## Overview

The YANG module "ietf-optical-sla-assurance" defines a data model for SLA assurance management in optical transport networks. The module provides a standard way to define, detect, and report issues that may impact service and network availability. It enables consistent modeling of assurance intent, impairment detection, and risk reporting across optical transport domains. The information reported in the SLA assurance issue includes issue identification, classification, severity, impacted objects, and remediation suggestions.

At the top of "ietf-optical-sla-assurance " module is the SLA Assurance container. The SLA assurance issues are represented as a list and indexed by "csn" (Customer Serial Number). Each SLA assurance issue is associated with issue metadata such as issue name, type, category, layer, severity, and timing information. In addition, the module supports relationships between issues through related-issues list and identifies source objects where the issue occurs. The module also defines impacted objects (services and tunnels) that are affected by each issue.

~~~~ ascii-art
   module: ietf-optical-sla-assurance
   +--rw sla
      +--rw issues* [csn]
         +--rw csn                 uint64
         +--rw issue-id?           int64
         +--rw issue-name?         string
         +--rw issue-sla-type?     enumeration
         +--rw layer?              enumeration
         +--rw issue-category?     enumeration
         +--rw occur-time?         yang:date-and-time
         +--rw clear-time?         yang:date-and-time
         +--rw description?        string
         +--rw suggestion?         string
         +--rw severity?           enumeration
         +--rw related-issues* [csn]
         |  +--rw csn    uint64
         +--rw source-objects* [object-id]
         |  +--rw object-name?   string
         |  +--rw object-id      yang:uuid
         |  +--rw object-type?   string
         +--rw issue-type?         enumeration
         +--rw impacted-objects* [service-name tunnel-name]
            +--rw service-name    yang:uuid
            +--rw tunnel-name     yang:uuid
~~~~

The issue-sla-type enumeration classifies issues into three categories based on their impact: bandwidth, availability, and delay. The layer enumeration identifies the transport layer where the issue originates, including service-layer, electrical-layer (OTN/ODU), optical-layer (DWDM/OCh), and fiber-layer. The issue-category distinguishes between fault conditions (actual failures) and risk conditions (potential failures that may lead to SLA violations).

The severity enumeration provides four levels: critical, major, minor, and info, allowing operators to prioritize issue resolution. The related-issues list establishes relationships between issues, enabling correlation analysis and root cause identification. The source-objects list identifies the specific network resources where the issue manifests, with each object identified by UUID and described by name and type.

The module also defines an RPC operation "query-history-issue-statistics-by-service" that allows retrieval of historical SLA issue statistics for specific services or tunnels within a specified time range. The RPC accepts service identifiers (UUIDs), time range parameters, and an optional SFTP URL for exporting results. The output provides comprehensive statistics including interruption count, total interruption duration, availability percentage, and a detailed interrupt history with begin/end times and durations.

~~~~ ascii-art
rpcs:
   +---x query-history-issue-statistics-by-service
      +---w input
      |  +---w service-name?   yang:uuid
      |  +---w tunnel-name?    yang:uuid
      |  +---w begin-time?     date-and-time-s
      |  +---w end-time?       date-and-time-s
      |  +---w sftp-url?       string
      +--ro output
         +--ro sla-issue-statistic* [service-name tunnel-name]
            +--ro service-name    yang:uuid
            +--ro tunnel-name     yang:uuid
            +--ro service-pm
               +--ro interruptions?        uint64
               +--ro interrupt-duration?   uint64
               +--ro availability?         decimal64
               +--ro interrupt-history* [interrupt-begin-time interrupt-end-time]
                  +--ro interrupt-begin-time    yang:date-and-time
                  +--ro interrupt-end-time      yang:date-and-time
                  +--ro interrupt-duration?     uint64
~~~~

The model is designed to support closed-loop operations for SLA assurance in optical transport networks. It enables automated monitoring systems to detect issues, classify them according to SLA impact, and initiate appropriate remediation actions. The separation between symptomatic issues (actual service degradation) and asymptomatic issues (risk conditions that may lead to future SLA violations) allows for proactive assurance management, helping to prevent incidents before they occur.

The module follows YANG 1.1 specification and imports standard YANG types from ietf-yang-types. It defines custom groupings for reusable issue attributes (issue-base) and complete issue definitions (issue), promoting modularity and consistency across different implementations. The use of UUID for service and tunnel identifiers ensures global uniqueness and facilitates integration with other management systems.

This data model is intended to be used by network management systems, orchestration platforms, and assurance analytics engines in optical transport network environments. It provides a standardized representation of SLA assurance issues that can be exchanged between different vendor equipment and management systems, enabling multi-vendor interoperability and consistent SLA management across heterogeneous optical transport networks.

## SLA assurance issue identification and observability

SLA assurance issue identification is the process of detecting and categorizing network conditions that may impact service performance, availability, or reliability. The YANG model provides a structured framework for representing identified issues with comprehensive observability attributes that enable effective monitoring, analysis, and remediation.

Each SLA assurance issue is uniquely identified by a Customer Serial Number (CSN), which serves as the primary key in the issues list. The CSN is a 64-bit unsigned integer that provides a globally unique identifier for each issue within a management domain. In addition to the CSN, issues may optionally include an issue-id (64-bit signed integer) for legacy compatibility or cross-system referencing, and a human-readable issue-name (string up to 1000 characters) for operator identification and documentation.

Observability of issues is enhanced through temporal and descriptive attributes. The occur-time attribute records when the issue was first detected or identified, using the yang:date-and-time type for precise timestamping. The clear-time attribute records when the issue was resolved or cleared, enabling calculation of issue duration and trend analysis. For issues that represent persistent conditions rather than events, the clear-time may be unset until resolution.

## SLA assurance issue resolution and capability list

SLA assurance issue resolution encompasses the processes and capabilities for addressing identified issues, ranging from immediate remediation actions to long-term capacity planning and network optimization. The YANG model provides the data foundation for representing issues throughout their lifecycle and enables the definition of standardized resolution capabilities and procedures.

Issue resolution typically follows a structured workflow: detection, classification, prioritization, diagnosis, remediation, and verification. The YANG model supports this workflow through comprehensive issue representation and temporal tracking. The occur-time and clear-time attributes enable measurement of issue duration and resolution time, while the severity attribute drives prioritization. The description and suggestion attributes provide contextual information and remediation guidance for both human operators and automated systems.

The SLA assurance capability list represents the set of issue types that a management system can detect, analyze, and resolve. This capability is measured by the comprehensiveness of the issue catalog and the effectiveness of the resolution mechanisms. The YANG model's issue-sla-type and issue-category enumerations provide the framework for organizing this capability list, which can be extended with specific issue instances through the issue-name and description attributes.

# SLA Assurance YANG module

~~~~ yang
{::include yang/ietf-optical-sla-assurance.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-optical-sla-assurance@2026-07-02.yang"}

# Security Considerations

The YANG module specified in this document defines a data model for SLA assurance management in optical transport networks. The module is designed to be accessed via YANG-based management protocols, such as NETCONF {{!RFC6241}} and RESTCONF {{!RFC8040}}. These YANG-based management protocols (1) have to use a secure transport layer (e.g., SSH {{!RFC4252}}, TLS {{!RFC8446}}, and QUIC {{!RFC9000}}) and (2) have to use mutual authentication.

The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

Some of the readable data nodes in this YANG module may be considered sensitive or vulnerable in some network environments. It is thus important to control read access (e.g., via get, get-config, or notification) to these data nodes. These are the subtrees and data nodes and their sensitivity/vulnerability:

'/sla/issues': This list specifies the SLA assurance issue entries for optical transport networks. Unauthorized read access of this list can allow intruders to access SLA assurance information and potentially get a picture of the health and resilience status of the optical transport network. Intruders may exploit the vulnerabilities revealed in the issue data, including source-objects and impacted-objects, to lead to further negative impact on the network. Care must be taken to ensure that this list is accessed only by authorized users.

Some of the RPC operations in this YANG module may be considered sensitive or vulnerable in some network environments. It is thus important to control access to these operations. These are the operations and their sensitivity/vulnerability:

"query-history-issue-statistics-by-service": This RPC operation retrieves historical SLA issue statistics and service performance metrics. If a malicious or buggy client performs an unexpectedly large number of this operation, the result might be an excessive use of system resources on the server side as well as network resources. Servers MUST ensure they have sufficient resources to fulfill this request; otherwise, they MUST reject the request using appropriate RPC error responses. Additionally, the sftp-url parameter provided in this operation MUST be validated against authorized destinations to prevent unauthorized data exfiltration.

# IANA Considerations

## The "IETF XML" Registry

IANA is requested to register the following URI in the "ns" registry within the "IETF XML Registry" group {{!RFC3688}}:

URI: urn:ietf:params:xml:ns:yang:hnts

Registrant Contact: The IESG.

XML: N/A, the requested URIs are XML namespaces.

## The "YANG Module Names" Registry

IANA is requested to register the following YANG module in the "YANG Module Names" registry {{!RFC6020}} within the "YANG Parameters" registry group.

Name: ietf-optical-sla-assurance

Maintained by IANA?  N

Namespace: urn:ietf:params:xml:ns:yang:ietf-optical-sla-assurance

Prefix: ietf-optical-sla-assurance

Reference:  RFC XXXX

// RFC Ed.: replace XXXX and remove this comment

--- back

# Acknowledgments
{:numbered="false"}

The authors would like to thank Yanlei Zheng, Italo Busi for their valuable comments and great input to this work.
