# AWS Capstone Project University Solution

## Scenario
With admission season around the corner XYZ University’s website lacks the proper availability to host it’s thousands of users. This results in a web application that is slow and utterly frustrating to use. Students and facility rely on this application for daily use and access to important information which applies urgency for a quick implementation of a solution in order to mitigate the effects on the University.
## Solution
In order to solve this problem they have decided to migrate to the AWS Cloud with a goal to mitigate user frustration and distribute a highly accessible and agile application to all users. Using my background in AWS I have created a solution within the AWS console and created an arcitechture for this hypethetical univeristy to implement to boost their availability for their users.
## Implementation
There are many aspects at play when it comes to and AWS architecutre which is why I will go thrpouh my process step by step in order to make it easier to digetst.
### Step 1 - Create a Virtual Private Cloud (VPC)
This first step in almost any situation is to create a Virtual Private Cloud (VPC) for a company so that all resources are easier to track and are in a centralized area for managment. A VPC is very similar to an on premise network but it is vitrualized through AWS and made easier to manage as you don't need to worry about as much physical hardware and over provisioning. To create the VPC in the AWS console follow the steps below.
+ Navagate to VPC in the services menu
+ Select Create VPC
+ Select requirements for personal scenario

For the University specifically, it is only necessary to create 2 public subnets, which are similar to LANs, in different availablility zones for backup incase of failure on the AWS side and auto scaling later. This helps to make the website which will be hosted on and EC2 instance on the subnet more available and accessable to users. For the sake of this scenario NAT gateways are not necessary as there are no private subnets required as the RDS has been simplified within the application itself. With this groundwork in place the University is setup to have much improvement to their resoureces at a cheap price.
### Step 2 - Create an EC2 Instance
In order for XYZ University's application to be accessable to the public for users to interact with an EC2 instance needs to be created to act as a Web Server and host the application on the public subnet. In order to craete and EC2 Instance follow the steps bellow.
+ Navigate to EC2 in the services menu within the AWS console
+ Click on instances
+ Select launch instance
+ Provision to case specific requirements
+ Launch the Instance

With regards to the Univeristy's case the instance requirments are more taxing than a personal use case as there are thousands of users relying on the availabiliy of the application.
