# Inband Telemetry

## INT(Inband Network Telemetry)
P4.org与2015年提出，Alibaba, Arista, CableLabs, Cisco Systems, Dell, Intel, Marvell, Netronome, VMware等公司均有贡献。最新版2.1标准2020-11-11发布。  
Barefoot的SPRINT(**S**mart、**P**rogrammable、**R**eal Time **INT**)解决方案：转发面与SDK软件结合的，针对白盒生态系统的解决方案；转发面使用P4实现可编程，SDK适配SAI的DTEL。


### 参考
1. [In-band Network Telemetry (INT) Dataplane Specification](https://p4.org/p4-spec/docs/INT_v2_1.pdf)
2. [In-band Network Telemetry (INT)](https://nkatta.github.io/papers/int-hula.pdf)
3. [Improving Network Monitoring and Management with Programmable Data Planes](https://opennetworking.org/news-and-events/blog/improving-network-monitoring-and-management-with-programmable-data-planes/)

## IFA(Inband Flow Analyzer)
2018年提出，Broadcom, Arista, Alibaba, Huawei, Fujian Ruijie等公司参与贡献。最新版至2022年。

实现原理类似INT：
- IFA delivers Inband Telemetry;
- Each hop adds its metadata to the user packet in the dataplane;
- Metadata is gathered from each node in a cumulative manner;
- Last network node removes the metadata and sends one consolidated report to the collector application;

### 参考
1. [IFA的IETF草案](https://datatracker.ietf.org/doc/draft-kumar-ippm-ifa/)
2. [Inband Flow Analyzer (IFA) 2.0 Probe for Real-Time Flow Monitoring](https://www.juniper.net/documentation/us/en/software/junos/flow-monitoring/topics/topic-map/ifa2.0-probe-for-real-time-performance-monitoring.html)
3. [Trident 4 IFA 2.0 w/ BroadView Instrumentation & AIOps](https://www.broadcom.com/video/eb1489a0ce8e428797dfb4342366184a)

## iOAM(In-Situ OAM)

### 参考

## IFIT(In-situ Flow Information Telemetry)

### 参考