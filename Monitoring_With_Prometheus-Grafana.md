# Monitoring.
# Index

1. [What is Monitoring?](#What-is-Monitoring)
2. [Why is Monitoring Important?](#Why-is-Monitoring)
3. [Various Monitoring Aspects](#various-monitoring-aspects)
4. [What is Prometheus?](#what-is-prometheus)
5. [Where and Why is Prometheus Used?](#where-and-why-is-prometheus-used)
6. [Specific Use Cases for Using Prometheus Monitoring](#specific-use-cases-for-using-prometheus-monitoring)
7. [How does Prometheus Work?](#how-does-prometheus-work)
8. [Prometheus Architecture Explained](#prometheus-architecture-explained)
9. [Prometheus Server](#prometheus-server)
10. [What are Targets and Metrics?](#what-are-targets-and-metrics)
11. [How does Prometheus Collect Metrics from Targets?](#how-does-prometheus-collect-metrics-from-targets)
12. [What are Target Endpoints and Exporters?](#what-are-target-endpoints-and-exporters)
13. [How to Monitor Your Own Application?](#how-to-monitor-your-own-application)
14. [Pull Mechanism - Unique Advantage of Prometheus](#pull-mechanism---unique-advantage-of-prometheus)
15. [Pushgateway for Short-lived Jobs](#pushgateway-for-short-lived-jobs)
16. [Configuring Prometheus - Example YAML Configuration](#configuring-prometheus---example-yaml-configuration)
17. [Alert Manager - Triggering Alerts](#alert-manager---triggering-alerts)
18. [Prometheus Data Storage - Where Does Prometheus Store the Data?](#prometheus-data-storage---where-does-prometheus-store-the-data)
19. [PromQL Query Language](#promql-query-language)
20. [Key Characteristics - Advantages and Disadvantages of Prometheus](#key-characteristics---advantages-and-disadvantages-of-prometheus)
21. [Prometheus Monitoring with Docker and Kubernetes](#prometheus-monitoring-with-docker-and-kubernetes)
22. [Grafana ](#Grafana)

# What-is-Monitoring

Monitoring is the process of **collecting** and **analyzing** data about a system in order to identify and troubleshoot problems. It is an essential part of IT operations, as it allows organizations to ensure that their systems are running **smoothly** and **efficiently**.


# Why is Monitoring.

**Identifying and troubleshooting problems:** Monitoring can help organizations to identify and troubleshoot problems quickly. This can help to prevent outages and to improve the overall performance of the system.

**Ensuring system availability:** Monitoring can help organizations to ensure that their systems are available when they are needed. This is important for businesses that rely on their systems to operate.

**Protecting system security:** Monitoring can help organizations to protect their systems from unauthorized access. This is important for businesses that handle sensitive data.

**Make informed decisions:** Monitoring can provide organizations with data that can be used to make informed decisions about their systems. For example, monitoring can be used to identify trends in system performance, which can help organizations to make decisions about how to improve the performance of their systems.

Overall, monitoring is an essential part of IT operations. It allows organizations to ensure that their systems are running smoothly and efficiently, and it helps to identify and troubleshoot problems quickly.

##  **Here are some of the benefits of monitoring:**

**Prevention:** Monitoring can help to prevent problems from occurring by identifying potential issues before they cause an outage or other disruption.

**Resolution:** Monitoring can help to resolve problems quickly by providing information about the source of the issue and how to fix it.

**Improvement:** Monitoring can help to improve the performance of systems by identifying areas where they can be optimized.

**Cost savings:** Monitoring can help to save money by preventing outages and other disruptions that can lead to lost revenue.

## The challenges of monitoring include:

**Data collection:** Collecting data can be a challenge, especially for large systems.

**Data analysis:** Analyzing data can be a challenge, especially for complex systems.

**Alerting:** Alerting users of problems can be a challenge, especially if the problems are not obvious.

**Cost:** Monitoring can be expensive, especially for large systems.

Overall, monitoring is an essential part of IT operations. It allows organizations to ensure that their systems are running smoothly and efficiently, and it helps to identify and troubleshoot problems quickly.

## Various Monitoring Aspects

1. **Infrastructure Monitoring:**
   - Servers (CPU, memory, disk usage, etc.)
   - Network devices (routers, switches, firewalls)
   - Storage systems (SAN/NAS)
   Infrastructure Monitoring:
   - Nagios
   - Zabbix
   - Prometheus
   - Grafana
   - SolarWinds Server & Application Monitor (SAM)

2. **Application Monitoring:**
   - Application response times
   - Error rates and exceptions
   - Throughput and request rates
   **Application Monitoring:**
   - New Relic
   - Datadog
   - AppDynamics
   - Dynatrace
   - Stackify Retrace

3. **End-User Monitoring (EUM):**
   - End-user experience and satisfaction
   - Page load times
   - Transaction success rates
   **End-User Monitoring (EUM):**
   - Google Analytics
   - Pingdom
   - Dynatrace Real User Monitoring (RUM)
   - New Relic Browser

4. **Log Monitoring:**
   - System logs (syslog, Windows Event Logs)
   - Application logs (web server, database, application-specific)
   **Log Monitoring:**
   - ELK Stack (Elasticsearch, Logstash, Kibana)
   - Splunk
   - Graylog
   - Sumo Logic

5. **Performance Monitoring:**
   - Application performance metrics
   - Server performance metrics
   - Network performance metrics
   **Performance Monitoring:**
   - Apache JMeter
   - Gatling
   - LoadRunner
   - Locust

6. **Security Monitoring:**
   - Security events and alerts
   - Intrusion detection
   - Logins and access attempts
   **Security Monitoring:**
   - ArcSight
   - Splunk Enterprise Security
   - LogRhythm
   - IBM QRadar

7. **Network Monitoring:**
   - Network bandwidth utilization
   - Latency and packet loss
   - Network device health (status, availability)
   **Network Monitoring:**
   - PRTG Network Monitor
   - SolarWinds Network Performance Monitor (NPM)
   - Nagios Network Analyzer
   - ManageEngine OpManager

8. **Cloud Monitoring:**
   - Cloud resource utilization (compute, storage, networking)
   - Auto-scaling events
   - Cloud service availability
   **Cloud Monitoring:**
   - Amazon CloudWatch
   - Google Cloud Monitoring (formerly Stackdriver)
   - Azure Monitor
   - Datadog Cloud Monitoring

9. **Database Monitoring:**
   - Database query performance
   - Connection pool usage
   - Database locks and deadlocks
   **Database Monitoring:**
   - Oracle Enterprise Manager (OEM)
   - SQL Server Management Studio (SSMS)
   - Prometheus with database exporters (e.g., mysqld_exporter)

10. **Synthetic Monitoring:**
    - Simulated user interactions and transactions
    - Global performance variations
    - Regional performance monitoring
    **Synthetic Monitoring:**
    - Pingdom
    - Uptrends
    - Site24x7
    - Dynatrace Synthetic Monitoring

11. **Container Monitoring:**
    - Resource utilization in containerized environments (e.g., Docker, Kubernetes)
    - Container health and status
    **Container Monitoring:**
    - Docker Monitoring with cAdvisor
    - Kubernetes Monitoring with Prometheus Operator
    - Sysdig Monitor
    - Datadog Container Monitoring

12. **Virtualization Monitoring:**
    - Virtual machine performance metrics
    - Hypervisor performance and resource usage
    **Virtualization Monitoring:**
    - VMware vRealize Operations
    - SolarWinds Virtualization Manager
    - Microsoft System Center Virtual Machine Manager (SCVMM)

13. **Service-Level Agreement (SLA) Monitoring:**
    - Monitoring adherence to SLAs and uptime guarantees
    - Service availability and response times
    **Service-Level Agreement (SLA) Monitoring:**
    - Freshping
    - UptimeRobot
    - Pingdom
    - StatusCake

14. **Alerting and Notification:**
    - Setting up and managing alert thresholds
    - Notifying relevant teams or stakeholders of critical events
    **Alerting and Notification:**
    - PagerDuty
    - OpsGenie
    - VictorOps
    - Slack (integration with monitoring tools for alerting)

15. **Dashboarding and Visualization:**
    - Creating and customizing real-time dashboards
    - Data visualization for easier analysis and reporting
    **Dashboarding and Visualization:**
    - Grafana
    - Kibana (part of ELK Stack)
    - Datadog Dashboard
    - New Relic Insights

Each of these monitoring aspects provides unique insights into the health and performance of different components of the IT infrastructure and applications. By monitoring these aspects, organizations can ensure optimal performance, detect and resolve issues proactively, and provide a seamless experience to end-users.

# **Prometheus: Overview and Architecture**

## What is Prometheus?

Prometheus is an open-source **monitoring** and **alerting** system that is widely used in the DevOps and cloud-native ecosystem. It was developed by **SoundCloud** and later donated to the **Cloud Native Computing Foundation (CNCF)**.

Prometheus was created to **monitor highly dynamic container environments** like Kubernetes, Docker Swarm, etc. However, 
it can also be used in a traditional non-container infrastructure where you have just bare servers with applications deployed 
directly on it. 
So over the past years, Prometheus has become the mainstream monitoring tool of choice in the container and 
microservice world.
