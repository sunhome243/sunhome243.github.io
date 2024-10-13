---
title: "Load Testing Report: Pushing FitSync's Backend to its Limits"
categories:
  - projects
tags:
  - DevOps
  - Performance Testing
  - Cloud Infrastructure
  - Azure
sidebar:
  nav: "sidebar-category"
toc: true
toc_sticky: true
toc_label: "Load Testing Report"
header:
  teaser: "/assets/posting/performance-boost.png"
---

## 📌 Overview

I conducted load testing on our backend services to evaluate their capacity to handle high traffic volumes. This report summarizes my findings, focusing on the deployment's scalability and performance under heavy load. I used Apache Bench (ab) to generate the load and leveraged Prometheus and Grafana for monitoring and data collection.

## 🛠️ Load Testing Procedure

I personally designed and executed the load testing procedure:

### Load Testing Tool: Apache Bench (ab)

I chose Apache Bench for its simplicity and effectiveness. Here's the command I used:

```bash
ab -n 100000 -c 100 http://<External IP>.nip.io/user-service/health
```

- **Number of Requests**: 100,000
- **Concurrency Level**: 100

I specifically targeted the `/user-service/health` endpoint to simulate multiple concurrent users accessing a critical part of our system.

## 📊 Results Summary

After running the load test, I analyzed the following key performance metrics:

- **Total Requests**: 100,000
- **Time Taken for Tests**: 68.082 seconds
- **Requests per Second**: 1468.82 [#/sec] (mean)
- **Time per Request**: 68.082 ms (mean)
- **Transfer Rate**: 210.86 KB/sec received

### Detailed Connection Times

I broke down the connection times to understand our system's performance at different stages:

- **Connect**: Minimum 25 ms, Mean 29 ms, Maximum 1034 ms
- **Processing**: Minimum 26 ms, Mean 39 ms, Maximum 345 ms
- **Total Time**: Minimum 53 ms, Mean 68 ms, Maximum 1088 ms

### Percentile Breakdown

To get a clearer picture of our system's performance across all requests, I calculated the following percentiles:

- **50% of Requests**: Completed within 65 ms
- **90% of Requests**: Completed within 78 ms
- **99% of Requests**: Completed within 125 ms
- **Longest Request**: 1088 ms

## 🔍 Deployment Performance Analysis

### Scalability and Resilience

Based on the load test results, I concluded that our deployment of the FitSync backend services demonstrated excellent scalability and resilience:

- The system successfully handled up to 1468 requests per second.
- The mean response time of 68 ms indicates a highly responsive system even under heavy load.

I monitored the following metrics in Grafana during the load test:

#### CPU Usage

<img src="/assets/posting/cpu-usage-graph.png" alt="CPU Usage Graph" width="600"/>

I observed that CPU usage initially spiked when the load test began. However, the HorizontalPodAutoscaler (HPA) I configured responded quickly, increasing the number of pods. This action effectively distributed the load, bringing CPU usage back to manageable levels.

#### Pod Deployment and Scaling

<img src="/assets/posting/pod-scaling-graph.png" alt="Pod Scaling Graph" width="600"/>

The deployment graph showed that during peak load, the number of pods increased dynamically. I saw the HPA scale the pods from 1 to 5 replicas, which helped maintain stable CPU utilization and response times.

#### Network I/O

<img src="/assets/posting/network-io-graph.png" alt="Network I/O Graph" width="600"/>

I monitored network traffic closely and observed a significant increase as the load ramped up. However, the Kubernetes cluster's network capabilities that I had configured proved sufficient, with no significant signs of bottlenecks.

#### Memory Usage

<img src="/assets/posting/memory-usage-graph.png" alt="Memory Usage Graph" width="600"/>

I noted spikes in memory usage during pod scaling activities. Each additional pod consumed additional memory, as expected. The cluster's memory capacity, which I had provisioned, was sufficient to manage the increased demand.


## 💡 Key Findings

Based on my analysis of the load test results, I identified the following key findings:

1. **Scalable Deployment**: The Kubernetes deployment I configured effectively utilized the HorizontalPodAutoscaler to handle increased loads by scaling out pods.

2. **Fast Response Times**: Despite the high concurrency level, the average response time remained well within acceptable limits. 90% of requests completed within 78 ms, indicating that the system I designed can handle large numbers of concurrent users without significant performance degradation.

3. **No Failures**: There were no failed requests during the load test, demonstrating the robustness of the infrastructure I had set up.

## 🔮 Future Improvements

Based on these results, I've identified several areas for future improvement:

1. **Fine-tune Autoscaling**: Adjust HPA settings to potentially scale up more aggressively during sudden traffic spikes.

2. **Optimize Database Queries**: Analyze and optimize database queries to reduce processing time for data-intensive operations.

3. **Implement Caching**: Introduce a caching layer (e.g., Redis) to reduce database load for frequently accessed data.

4. **Enhance Monitoring**: Set up more detailed monitoring and alerting for specific service-level indicators (SLIs) and objectives (SLOs).

## 🎉 Conclusion

The load testing results provide a positive outlook for the scalability and reliability of the FitSync backend deployment that I architected and implemented. The system demonstrated excellent resilience and scalability, capable of handling high traffic volumes while maintaining performance.

As we move forward, I'll focus on implementing the identified improvements to further enhance our system's performance and reliability. These load testing insights will be crucial in supporting FitSync's growth and ensuring we can meet the demands of our expanding user base.