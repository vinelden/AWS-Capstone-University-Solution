# AWS Capstone Project University Solution

## Scenario
With admission season around the corner, XYZ University’s website lacks the proper availability to host its thousands of users. This results in a web application that is slow and utterly frustrating to use. Students and facilities rely on this application for daily use and access to important information which applies urgency for a quick implementation of a solution in order to mitigate the effects on the University.

![XYZ](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/6b0c4de6-950f-461d-85ae-671801ab41ac)

## Solution
### Architecture
In order to solve this problem they have decided to migrate to the AWS Cloud with a goal to mitigate user frustration and distribute a highly accessible and agile application to all users. Using my background in AWS I have created a solution within the AWS console and created an 
architecture for this hypothetical university to implement to boost their availability for their users.

![AWS Capstone Project Topology (1)](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/fa795571-0b84-44d5-87c1-95d2ff42aca2)

My solution creates a VPC for the University with two public subnets. Within these subnets, there are EC2 instances that host the web application. My solution utilizes an Application Load Balancer that routes the traffic to an Auto Scaling Group that will help to handle peak traffic at thousands of connections and requests. My solution also stays within an $80 budget for a month's worth of uptime and maintenance. This is the main reason that Amazon Web Services is a great tool for this university as it provides a cheap alternative to on-premises networking services that are harder to provision and come at a much higher price.

This second diagram, created with LucidChart, shows my original ideas for the architecure but as the project progressed I had to change and simplify it as the circumstances required.

![AWS Capstone Project Topology](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/7badc538-fd4b-46fb-bcd9-fcf60a174850)

### Cost Analysis

![pt11](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/5e32f200-a9b1-4d29-a595-710b59e50e19)

![pt2](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/babb1d80-837a-4fab-9653-356d482456fd)

$38.71 can be reducted from the total monthly cost as RDS was simplified from this project and is incorporated within the application itself.

![pt3](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/0ca80703-9a52-45e4-9eda-f6198080e7e8)

## Implementation
There are many aspects at play when it comes to AWS architecture which is why I will go through my process step by step in order to make it easier to digest.

### Step 1 - Create a Virtual Private Cloud (VPC)
This first step in almost any situation is to create a Virtual Private Cloud (VPC) for a company so that all resources are easier to track and are in a centralized area for management. A VPC is very similar to an on-premise network but it is virtualized through AWS and made easier to manage as you don't need to worry about as much physical hardware and over-provisioning. To create the VPC in the AWS console I followed the steps below.
+ Navigate to VPC in the services menu
+ Select Create VPC
+ Select requirements for the personal scenario

![Screenshot 2023-05-31 141515](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/2092dc98-ecd5-4a38-8c75-671ef08ea384)

For the University specifically, it is only necessary to create 2 public subnets, similar to LANs, in different availability zones for backup in case of failure on the AWS side and auto-scaling later. This helps to make the website which will be hosted on an EC2 instance on the subnet more available and accessible to users. I also created a security group for the VPC that allows HTTP and HTTPS traffic from IPv4 anywhere to receive HTTP requests from users. For the sake of this scenario, NAT gateways are not necessary as there are no private subnets required as the RDS has been simplified within the application itself. With this groundwork in place, the University is set up to have much improvement to its resources at a cheap price.

### Step 2 - Create an EC2 Instance
In order for XYZ University's application to be accessible to the public for users to interact with an EC2 instance needs to be created to act as a Web Server and host the application on the public subnet. In order to create an EC2 Instance I followed the steps below.
+ Navigate to EC2 in the services menu within the AWS console
+ Click on instances
+ Select launch instance
+ Provision to case-specific requirements
+ Launch the Instance

![Screenshot 2023-05-31 141425](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/17c56f2b-5dc9-425e-804b-3421afd625a5)

With regards to the University's case the instance requirements are more taxing than a personal use case as there are thousands of users relying on the availability of the application. I chose to host this instance on the university-subnet-public1 using the same security group that I created in Step 1. This allowed it to then be replicated later with an AMI and a target group to distribute the load across two availability zones for better fault tolerance and access. For the actual instance, I chose Ubuntu as the operating system which was compatible with the application and t2.micros which are able to handle the requests from thousands of users at the same time. I also inserted the provided application code into the user data which was how the application was hosted on the instance. After launching the instance and passing the two status checks I entered its public IP address into a browser and found that the website was accessible and ready for minimal use.

### Step 3 - Create an Amazon Machine Image (AMI)
With the website up and running, available to users the next step was to increase the availability so that more clients could access the application. While at the same time creating an environment that could handle peak traffic. To create an AMI I followed the steps below.
+ Select Web Server 1 (The instance I created to host the application)
+ Select Actions
+ Select Image and templates and then Create Image

![Screenshot 2023-06-01 141721](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/a1d48898-6bd5-4c76-bc37-77e1c6bd5838)

This is the first step that is crucial for creating a highly available application. Creating this AMI allowed me to make an Auto Scaling Group and an application Load Balancer so that new instances could be reproduced from this image and populate both availability zones within both university-subnet-public1 and university-subnet-public2.

### Step 4 - Create a Load Balancer
The next step after creating an instance to host the web application and an AMI for it to be replicated easily was to create a Load Balancer, but before this, I also needed to create a Target Group. In order to create the Target Group I followed the steps below.
+ Select Target Group
+ Target Type: Instances
+ Select the University VPC
+ Register targets
+ Create the target group

![targetGroup](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/4398689d-6bf1-451a-ab8d-d78ce20c275c)

Then I created an Application Load Balancer following the steps below.
+ Navigate to Load Balancers
+ Select Create Load Balancer
+ Choose and Application Load Balancer
+ Select Company VPC and Subnets it applies to
+ Select the VPC Web Security Group
+ Create Load Balancer

![loadbalancer](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/6349295b-d238-431e-820d-3c789ce6dc32)

For this specific scenario it was necessary to create an Application Load Balancer which deals specifically with HTTP and HTTPS requests. This is to help handle the load from incoming requests at peak traffic times and balance the load across multiple instances in different subnets. I selected the university-vpc which I created in Step 1 as well as the security group that allows HTTP traffic created in the same step. For mapping, I selected both subnets in my VPC, university-subnet-public, and university-subnet-public2. This incorporates both subnets into the balancing of the requests in order to increase fault tolerance and availability of the application. This step is crucial for the solution as it creates an environment where all requests can be handled and distributed across the VPC in order to improve user experience.

### Step 5 - Create Launch Configuration and Auto Scaling Group
After creating the VPC, Web Server, AMI, and the Load Balancer the last step was to put all of these aspects together with automation. I chose to use an Auto Scaling Group to aid the availability of the application and the scaling of the solution. However, before I could create an Auto Scaling Group it was important to create a Launch Configuration first. To create a Launch Configuration I followed the steps below.
+ Navigate to Launch Configurations
+ Select Create launch configuration
+ Select the AMI created in Step 3
+ Configure Instance type to match the AMI
+ Enable Monitoring within CloudWatch
+ Select the security group created in Step 1
+ Create launch configuration

![launch configuration](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/53269c23-7dd3-44fb-b3ff-ec9d49597913)

I then created the Auto Scaling Group that followed this Launch Configuration template following the steps below.
+ Select the Launch Configuration
+ Select Actions and then Create Auto Scaling group
+ Select the university VPC as well as the subnets supported in the Load Balancer
+ Attach the Load Balancer created in Step 4
+ Enable metrics within CloudWatch
+ Configure Group Size and Capacity
+ Create a Scaling Policy with a target CPU utilization value
+ Create Auto Scaling group

![autoscalinggroups](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/cd48b2ec-8af1-4213-8f79-d83289478724)

For my Auto Scaling group it was important for my solution to be able to handle thousands of requests and connections at the same time to make the application more available during peak traffic times. For this, I found that my desired capacity and minimum capacity were ideal at 1, and my maximum capacity was ideally at 6 for the stress tests that I performed. This however could be increased at any time to handle more users as there is still plenty of space in the budget. I also enabled CloudWatch monitoring to make the maintenance on this aspect of the solution easier to manage. The alarms that I was able to set within CloudWatch were very helpful and could really help to avoid over-usage charges.

## Testing
While finding the correct stress testing application for my circumstances was a bit difficult I found that the Cloud9 service within AWS was ideal for a simple test based on requests and connections to my cloud architecture. I created a Cloud9 instance which was able to make the stress test compatible with my Web Server 1 and the University Instances that were being created by the Auto Scaling Group. After creating the Cloud9 instance it was time to try the stress test where I applied 5,000 requests and 2,000 connections which would be an accurate representation of peak traffic for XYZ University. 

After the stress test started the load on the resources started to increase dramatically, setting off the alarm within the CloudWatch monitoring and notified me that the Auto Scaling Group was going to create 2 new instances to handle the increased load. 

![Screenshot 2023-05-31 135729](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/eb6f47af-7bba-414a-9e1c-3beb54f76657)

![Screenshot 2023-05-31 135737](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/ea449e13-10a5-41ba-8a02-9dbc284245de)

![Screenshot 2023-05-31 135925](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/220311be-68f7-488f-be26-3a54b60458a5)

After a continuous load on the web server the Auto Scaling Group eventually deployed all 6 instances from my desired maximum capacity and the load was spread out across the VPC. 
![Screenshot 2023-05-31 141148](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/52c78573-475f-4b80-a801-ed282cdd52f5)
Additionally the average CPU utilization per instance was reduced to a comfortable threshold that kept the application available to all of the fake users that were requesting to access the application.

![Screenshot 2023-05-31 141322](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/7466da7e-7015-4070-97f2-7fa98fbdd75a)

![Screenshot 2023-05-31 141842](https://github.com/vinelden/AWS-Capstone-University-Solution/assets/133430016/122cca4d-2584-4db7-85fd-81a98035db5b)

## Conclusion
With the aid of Amazon Web Services, XYZ University was able to find a solution to a problem that was deterring potential students and opportunities. All coming at a cheap price, this solution was able to be implemented and tested under large stress as proof that the architecture was compatible and highly available to the users.
