[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)

# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: WANG, Shibo

### Student Id: 21143036

### Email: swanggi@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > <u>**Measurement Tool**: Phoronix Test Suite

    > **Configuration**: I configured Phoronix Test Suite by running two specific tests using the following commands:
    > 1. `phoronix-test-suite run pts/compress-7zip`: measures CPU performance by assessing the number of instructions processed per second during file compression using the 7-Zip algorithm.
    > 2. `phoronix-test-suite run pts/ramspeed`: assesses memory performance by benchmarking memory bandwidth and speed. Additionally, I choose the AVERAGE and INTEGER Option to evaluate the average performance when processing Interger operation in memory.
    
    > **Default Parameters**: I utilized the default settings for both tests to ensure standardized and reliable benchmarking results. This approach avoids introducing variables that could affect the comparability of the measurements.

    > **Explanation of Measurement Values**:
    > 1. For CPU performance measurement, I choose MIPS, which means the numbers of million instructions per second processed by CPU when processing the task. It can evaluate the performance of CPU.
    > 2. For memory performance measurement, I choose the speed to evaluate the performance of memory. The speeder, the performance higher.</u>

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |     3153 MIPS    |       10371.80 MB/s             |
    | `t2.medium`  |  100052 MIPS   |       19108.55 MB/s            |
    | `c5d.large` |   7526 MIPS     |       13918.95 MB/s             |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

     <u>According to the the experience result, We can find t2.medium's performance is the best while t2.micro's is the worst cause the configuration of t2.micro is worst. So we can draw a preliminary conclusion: the performance of EC2 instances generally increases with the addtion of more VCPUs and memory. However, the performance of c5d.large is lower than t2.medium although they have same number of VCPU and memory. Why?

    We can find a table in AWS document:

    | Size        | VCPU | Memory(GiB) | Cores | Threads per core |
    | ----------- | --------------- | ------------------ | -- | --|
    | `t2.micro` |    1   |       1             |1 |1|
    | `t2.medium`  |  2 |        4         | 2|1|
    | `c5d.large` |  2  |       4          | 1|2|

    So, one possible reason is the core number of t2.medium is double to it of c5d.large. It means even if the EC2 instance A has more VCPUS and memory than B, it's possible that A'performance is lower than B cause there are other important factors such as thread. So we can generate the final conclusion: **Only when other FACTORs is unchanging, the performance of EC2 instances generally increases with the addtion of more VCPUs and memory** </u>

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |   4120              |   0.364       |
    | `m5.large` - `m5.large`   |   4840              |   0.217       |  
    | `c5n.large` - `c5n.large` |   4960              |   0.164       |
    | `t3.medium` - `c5n.large` |   2370              |   0.605       |
    | `m5.large` - `c5n.large`  |   2430              |   0.599       |
    | `m5.large` - `t3.medium`  |   4240              |   0.230       |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    <u>In the same region, the netwwork performance between instances of the same type is better than it of the different type. For example, `m5.larg`e` connects to `c5n.large` is slowly while `m5.large` connects to another `m5.large` is quickly</u>

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |   33.0             |  60.752        |
    | N. Virginia - N. Virginia |   4650             |   0.182       |
    | Oregon - Oregon           |   4780             |   0.175       |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    <u>The connection speed of the EC2 instance in different regions is an order of magnitude slower than the connection speed in the same region. It means we must deploy the instances, databases and other resources in the same region to achieve better performance.</u>