# AWS_APAC
AWS APAC Solutions Architecture virtual experience program on Forage - December 2024    
- Designed and simple and scalable hosting architecture based on Elastic Beanstalk for a client experiencing significant growth and slow response times.
- Described my proposed architecture in plain language ensuring my client understood how it works and how costs will be calculated for it.
  
## REQUEST :- 
<p align="justify">
My name is Lilly Sawyer and I am the co-founder and CTO of Fastier, the online tool that makes organizing things fast. You may have seen that we have been in the news recently and have been experiencing some big growth as a result. Unfortunately, this means we’ve been having some difficulties with our website. The response times can be slow during peak periods and sometimes we even have server crashes when it runs out of memory. We also experience some downtime during deployments and don’t have a disaster recovery plan. Based on our current trajectory, we predict we’re going to see consistent linear growth for a while. Our application is a SPA React Application backed by Python and Flask, with PostgreSQL as a database. It all runs off a single AWS EC2 instance currently (t3.medium, 4GB of RAM). Can you design an architecture for us that will alleviate the problems we’re experiencing? We are not on a strict budget right now, but at the same time, it’s not unlimited.
</p>

## SOLUTION : 
<img src="https://github.com/mvaibhav5874/AWS_APAC/blob/main/Architecture%20Diagram.png" alt="Architecure_image" width="300" height="200">

### Tools used :

**Code Pipeline -**    
This will build code and can deploy it to various AWS services. It is charged per pipeline. Can be used with Elastic Beanstalk for blue/green no-downtime deployments.

**Elastic Load Balancing -**    
Distributes traffic across application servers, such as EC2, Lambda or Fargate. Can use health checks to know which servers should service requests. Charges are based on the number of hours the load balancer runs and (at a high level) the amount of traffic it services. The actual charging rules can be quite complex.

**RDS -**    
Relational database hosting platform. Charged in the same way as EC2 (i.e. a virtual machine with a set amount of resources). Servers can be resized but must be restarted to do so.

**Route 53 -**    
AWS' domain and DNS management.

**S3 -**    
Store objects in the cloud. Charged based on the amount of data being stored, how it’s stored, and for retrieval.


**Application Executors -**    
AWS provides a few ways of executing applications, from virtual machines, to container executors to function executors.

**EC2 -**    
Scalable virtual servers. Charged based on resources of the virtual servers (RAM, CPU, storage), per hour. Servers can change their resource allocation but must be restarted to apply them.

**Fargate -**    
Run containerized applications (e.g., Docker images). Charges are based on the resources assigned to the container (RAM, CPU, etc.) and how long it runs for. Very scalable but can require manual orchestration.

**Lambda -**    
An application is uploaded to Lambda and only executed when triggered, for example, on HTTP request or via S3. For example, a Lambda instance would run to just service a single HTTP request. Charging is based on the amount of memory the Lambda uses and how long it runs for, plus the number of requests. Sometimes data transfers can also be charged depending on which region(s) a Lambda is fetching data from. Highly scalable up and down. Not suited for serving static content.

**Lightsail -**    
Virtual machines, like EC2 but simpler to set up. Charges are based on the resources of the Lightsail virtual machine. Virtual machines' resources can’t be changed, instead the machines must be cloned to a new instance and restarted. 

**Elastic Beanstalk -**    
Ties together EC2, RDS and Elastic Load Balancing with simple configuration and deployment. Its primary advantage is how it can facilitate the autoscaling of EC2 instances. Billing is based on a combination of the EC2, RDS and ELB that you use. Deployment is made easy with CLI tools, and we can use rolling deployments so there is no downtime. It has support for several languages including Python, NodeJS, Java and Go.

**Availability Zones -**    
For extra redundancy, services can be deployed across multiple availability zones (AZ). This means that requests can fall over from one AZ to another in the case of infrastructure failure. In general, the cost of a service is multiplied by the number of availability zones it’s in.
You should be able to tell the customer why you have chosen Elastic Beanstalk and what components it contains, and the purpose of these. The AWS resource pages for each service contain descriptions of how they are charged.

The concrete pricing differs in each region, however, the method of price calculation is the same. For example, an EC2 instance in Region A may cost more than the same instance in Region B, however, both are billed at an hourly rate. The AWS pricing calculator at https://calculator.aws/ can also give some guidance.

### Reply  To Email :

**Subject**: AWS Services for Your Web Application Hosting

*Dear Lilly.sawyer@fastier.com*,

Greetings from Amazon Web Services (AWS).
We received your email regarding your company's interest in migrating to AWS. We're thrilled to assist you!
This email outlines the proposed architecture for your web application hosting on AWS. We'll describe the services involved and how they'll work together to ensure a reliable and scalable solution.

### The Services We'll Use:

1. Route 53: Your Digital Compass

    * DNS Service: Route 53 acts as your domain name system, directing internet traffic to your application.
    * Cost: Route 53 pricing is based on the number of hosted zones, query volume, and data transfer. For most small-to-medium-sized websites, the costs are minimal.

2. Elastic Load Balancing: Traffic Conductor

    * Load Distribution: The Elastic Load Balancer distributes incoming traffic across multiple instances, optimizing resource utilization and ensuring high availability.
    * Cost: ELB pricing is based on the number of load balancers, the amount of data transferred, and the number of hours the load balancer is active.

3. CodePipeline: Automated Deployment

    * CI/CD Pipeline: CodePipeline automates the build, test, and deployment processes, streamlining the software delivery lifecycle.
    * Cost: CodePipeline pricing is based on the number of pipeline executions and the amount of compute time used.

4. EC2: The Powerhouse

    * Virtual Machines: EC2 provides the virtual machines (VMs) that run your application. You can choose from a variety of instance types to meet your specific performance and cost requirements.
    * Cost: EC2 pricing is based on the type of instance, the operating system, and the number of hours the instance is running.

5. AWS CloudWatch: Your Application's Guardian

    * Monitoring and Logging: CloudWatch monitors your application's performance and logs, providing valuable insights into its behavior.
    * Cost: CloudWatch pricing is based on the number of metrics and alarms you use.

6. PostgreSQL Server: Data's Safe Haven

    * Database Service: PostgreSQL provides a robust and scalable database solution for storing your application's data.
    * Cost: PostgreSQL pricing is based on the instance type, storage size, and the amount of data processed.

7. Multiple Availability Zones: Fault Tolerance

    * Geographic Redundancy: By deploying your application across multiple Availability Zones, you can significantly reduce the risk of downtime due to hardware failures or regional outages.
    * Cost: The cost of deploying in multiple Availability Zones will increase, as you'll need to pay for additional instances and network traffic.

8. S3: Scalable Storage

    * Object Storage: S3 offers a durable and scalable storage solution for your application's data and assets.
    * Cost: S3 pricing is based on the amount of data stored and the number of requests made to the service.

### How It Works:
<p align="justify">
Users access your application through the internet, reaching Route 53, the DNS service.Route 53 directs users to the Elastic Load Balancer.The Elastic Load Balancer distributes incoming traffic across your application instances running in multiple Availability Zones.Each Availability Zone contains EC2 instances running your application code.The EC2 instances connect to the PostgreSQL database for data storage and retrieval.The application can utilize Amazon S3 for storing additional data or assets.CodePipeline automates the deployment process, ensuring smooth and efficient updates.Multiple Availability Zones provide redundancy and disaster recovery capabilities.
</p>

- Cost Considerations:
    The final cost depends on your specific application's resource needs and usage patterns. We estimate a monthly range of $250-$350, but this can vary. AWS offers pay-as-you-go pricing, so you only pay for the resources you use.  
- Next Steps:   
    If you have any questions or require further clarification, please don't hesitate to contact us. We're happy to assist you further in migrating your web application to a robust and scalable AWS environment.

Thank you for your interest in AWS!  
Sincerely,    
The AWS Team 

>[!NOTE]
>The certificate for this is added on links of this reposotry
