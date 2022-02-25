# CSIT6000O Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 23:59, Feb 25, Friday

---

### Name: Long Jiaqi
### Student Id: 20791801

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose any open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Tool name: Sysbench  
    
    > Configuration for CPU   
    > Number of threads: 4,
    > Prime numbers limit: 10000
    > 
    > The reason of setting parameter:   
    > For the number of threads: I find that the number of virtual CPU is 2 (t3.medium, m5.large and c5d.large). And in the document "sysbench-manual", it said "This test mode was written to benchmark scheduler performance, more specifically the cases when a scheduler has a large number of threads competing for some set of mutexes." So I just make the number of threads more than 2.  
    > For Prime numbers: This parameter means "each request consists in calculation of prime numbers up to a value specified by the --cpu-max-primes option". I think the number of prime number under 10000 is enough.
    > 
    > The values of measurement:  
    > I choose the "CPU speed" to measure the EC2 CPU performance, it means how many operations the system is capable of performing within a given time (events/sec).
    
    > Configuration for Memory  
    > Number of threads: 4, block size: 1KiB, total size: 10240MiB, operation: write, scope: global  
    > 
    > The reason of setting parameter:   
    > For the number of threads: the same as the reason is CPU.  
    > For the block size: default parameter
    > For the total size: make it do not last too long time.  
    > For the operation: not sure whether there exists file to read, so I choose write mode.  
    > For the scope: I want to make each thread use a globally allocated memory block.
    > 
    > The values of measurement:  
    > I choose the "transfer rate" to measure the EC2 Memory performance (MB/s).

2. (1 mark) Run your measurement tool on general purpose `t3.medium`, `m5.large`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size      | CPU performance | Memory performance |
    |-----------|-----------------|--------------------|
    | `t3.medium` | 1583.92 events/s|5366.20 MiB/sec|
    | `m5.large`  | 1658.47 events/s|6571.92 MiB/sec|
    | `c5d.large` | 1817.04 events/s|7298.48 MiB/sec|

    > Region: US East (N. Virginia). Use `Ubuntu Server 18.04 LTS (HVM)` as AMI.

    > Answer:  
    > Yes, as the results shows in the table, the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type          | TCP b/w (Mbps) | RTT (ms) |
    |---------------|----------------|----------|
    | `t3.medium` - `t3.medium` |4650|0.243|
    | `m5.large` - `m5.large`  |4132|0.307|
    | `c5n.large` - `c5n.large` |4964|0.124|
    | `t3.medium` - `c5n.large`   |4722|0.170|
    | `m5.large` - `c5n.large`  |4692|0.156|
    | `m5.large` - `t3.medium` |4680|0.158|

    > Region: US East (N. Virginia). Use `Ubuntu Server 18.04 LTS (HVM)` as AMI.

    > Answer:  
    > For instances of the same type, the TCP bandwidth is: c5c.large > t3.medium > m5.large; the RRT is c5.large < t3.medium < m5.large. So the best network performance is c5n.large, the worst is m5.large.
    
    > For instances of the different type, the network performance is very close.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection | TCP b/w (Mbps)  | RTT (ms) |
    |------------|-----------------|--------------------|
    | N. Virginia - Oregon |22.6|66.913|
    | N. Virginia - N. Virginia |4968|0.136|
    | Oregon - Oregon |4967|0.151|
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 18.04 LTS (HVM)` as AMI. All instances are `c5.large`.

    > Answer:  
    > The network performance for instances deployed in different regions is very bad, comparing with the performance in same region.
