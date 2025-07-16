## 1. Cloud Computing, Service Models, and Deployment Models

### 1.1 What is Cloud Computing?

Cloud computing is the on-demand delivery of IT resources via the internet, with pay-as-you-go pricing. Instead of buying, owning, and maintaining physical data centers and servers, organizations can access technology services such as compute power, storage, and databases from a cloud provider.

### Characteristics of Cloud Computing:

* On-demand self-service
* Broad network access
* Resource pooling
* Rapid elasticity
* Measured service

---

### 1.2 Cloud Service Models

| Model                              | Description                                                                                           | Examples                              |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------- |
| Infrastructure as a Service (IaaS) | Offers basic computing resources like VMs, storage, and networking. Users manage OS and applications. | Amazon EC2, Amazon VPC, EBS           |
| Platform as a Service (PaaS)       | Provides a platform to develop, run, and manage applications without infrastructure management.       | AWS Elastic Beanstalk, AWS App Runner |
| Software as a Service (SaaS)       | Delivers software applications over the internet on a subscription basis.                             | Salesforce, Microsoft 365, Gmail      |

**Use Case Examples**:

* IaaS: Web hosting using Amazon EC2.
* PaaS: Deploying a Java app without provisioning servers using Elastic Beanstalk.
* SaaS: Managing email through Gmail or Office365.

---

### 1.3 Cloud Deployment Models

| Deployment Model | Description                                                                 | Use Case                                          |
| ---------------- | --------------------------------------------------------------------------- | ------------------------------------------------- |
| Public Cloud     | Resources are owned by a cloud provider and shared across multiple tenants. | Hosting websites, startups                        |
| Private Cloud    | Used exclusively by one organization, can be on-premise or hosted.          | Finance, healthcare systems                       |
| Hybrid Cloud     | Combines public and private cloud for flexibility.                          | Enterprises with compliance and scalability needs |
| Community Cloud  | Shared between organizations with similar requirements.                     | Government or regulatory bodies                   |

---

## 2. AWS Global Infrastructure

AWS provides a secure and scalable cloud computing platform with a globally distributed infrastructure. It is designed to deliver performance, fault tolerance, and low-latency services across the world.

### 2.1 AWS Regions

* A **Region** is a physical location in the world where AWS has multiple **Availability Zones**.
* Each region is isolated and designed for fault tolerance.
* Example: `us-east-1` (North Virginia), `ap-south-1` (Mumbai)

**Use Case**: For users in India, using `ap-south-1` provides lower latency.

### 2.2 Availability Zones (AZs)

* An **Availability Zone** consists of one or more data centers with independent power, networking, and connectivity.
* AZs within a region are connected via low-latency links.
* Designed to isolate failure points for high availability.

**Use Case**: Deploy redundant EC2 instances across multiple AZs to ensure availability in case one AZ fails.

### 2.3 Edge Locations

* Edge Locations are data centers used by **Amazon CloudFront** and **AWS Global Accelerator** to deliver content close to the users.
* Ideal for caching static content, DNS resolution, or routing traffic.

**Use Case**: A static website hosted on S3 + CloudFront serves content faster through nearby edge locations.

### 2.4 AWS Local Zones

* AWS Local Zones bring AWS services closer to large population centers to support latency-sensitive applications.
* Useful for applications like gaming, video editing, and real-time analytics.

**Use Case**: A game development company in Los Angeles uses AWS Local Zone (LA) for low-latency game processing.

---

## Reference Links (AWS Official Documentation)

* [What is Cloud Computing? - AWS](https://aws.amazon.com/what-is-cloud-computing/)
* [Types of Cloud Computing - AWS](https://aws.amazon.com/types-of-cloud-computing/)
* [AWS Cloud Computing Models](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/cloud-computing-models.html)
* [AWS Global Infrastructure Overview](https://aws.amazon.com/about-aws/global-infrastructure/)
* [AWS Edge Locations](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html#Locations)
* [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)
* [Regions and Availability Zones - AWS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)
