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
    email: henry.yu1@huawei.com
 -
    fullname: Xiao Li
    organization: Huawei
    email: henry.yu1@huawei.com

normative:

  ITU-T_G.709:
    title: Interfaces for the optical transport network
    author:
      org: International Telecommunication Union
    date: 2020-06
    seriesinfo:
      ITU-T: G.709/Y.1331 (2020)
    target: https://www.itu.int/rec/T-REC-G.709

informative:


--- abstract

This document defines a YANG module for SLA assurance management in
optical transport networks. The module provides a standard way to
define, detect, and report issues that may impact service and network
availability. It enables consistent modeling of assurance intent,
impairment detection, and risk reporting across optical transport
domains. The YANG model is designed to support closed-loop operations,
allowing automated monitoring, analysis, and remediation workflows to
maintain high service reliability and SLA compliance {{ITU-T_G.709}}.


--- middle

# Introduction

Service Level Agreement (SLA) assurance management in optical transport
networks concerns the continuous monitoring, analysis, and management of
network conditions that may impact the performance, availability, and
reliability of services. Its objective is to detect potential issues
proactively, diagnose their root causes, and initiate corrective actions
to prevent SLA violations. Several standards already exist in this area.
SAIN {{!RFC9417}} and {{!RFC9418}} defines an architecture and a YANG model
for network service assurance, respectively.
{{!I-D.ietf-nmop-network-incident-yang}} defines a YANG model for the
network incident lifecycle management. It aims to provide a standard way
to report, diagnose, and help resolve network incidents which may cause
SLA violations.

Figure 1 illustrates the SLA assurance issue management architecture. The main component
for issue management is the Issue Server, which provides capabilities for issue identification
and classification, issue reporting, and issue querying. The Issue Server can be deployed on
controllers as defined in {{?RFC8969}} within each network domain and interfaces with the OSS. A typical workflow is as follows:

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
      <td rowspan="2">Delay</td>
      <td>Operating latency exceeds allowed limit</td>
    </tr>
    <tr>
      <td>Protection switching latency exceeds allowed limit</td>
    </tr>
  </tbody>
</table>

# Operational Considerations

TBC per {{?I-D.opsarea-rfc5706bis}}.

# SLA Assurance Data Model Design

# Overview

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

# SLA Assurance YANG module

~~~~ yang
{::include yang/ietf-optical-sla-assurance.yang}
~~~~
{: sourcecode-markers="true" sourcecode-name="ietf-optical-sla-assurance@2026-07-02.yang"}

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

