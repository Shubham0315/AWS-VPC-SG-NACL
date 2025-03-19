# AWS-VPC
Virtual Private Cloud to solve the problem of security breach in the organization. 

- Virtual Private Cloud is One of the most complicated topic of AWS.
- This comes under networking part of AWS
- VPC solves problem of security breach. AWS resources are not accessed by unauthorized people in order to maintain security

Real Life Scenario
-
- Suppose there is a village with lots of lazy people who dont want to construct their houses as they dont want to maintain a lot of things related to house.
- There is a person who saw this as opportunity to make money so he acquired land in village and asked lazy people to send him a request with entire requirements like resources, capacity. Then he will give you house and you give him money. So he acquired land before talking to lazy people and constructed houses which he will maintain for them
- But here other people in town realized there is a privacy breach or security issue as these houses are very close and anyone can access house of anyone. This arose as the constructor built houses to save money.
- So these people now request the person (constructor) to build house for them without compromising on security. Now the constructor comes with secured land. So he constructs secured property for these people.
- Thus now people who has access to secured property will only access that property.
- He will construct a gate or gateway from where only secured people will access/enter property
- Now there is A B and C houses in secured property and X who is guest wants to meet A, he has to pass through gateway and then after entering property he need guide to reach A. After reaching A there will be another security gate to validate if he is authorized person to meet A.
- In this way we can solve problem of security breach

- Now suppose we are in a Region of AWS (Mumbai) where there are many companies who need to maintain their own data centers which was hectic for them.
- So AWS saw this as opportunity. AWS started to build their own data centers in region (Mumbai). AWS can host apps of those companies inside these data centers. So if the companies want resources, they provide money to AWS and get the required resources

- If company wants 10 EC2 instances, it went to data center in Mumbai region and created 10 EC2 instances. Inside the data centres as well there can be multiple servers. AWS creates say 2 instances under server1. Another company also requested same and have their 2 instances under server1
- If proper security is not there, hacker entered into server and tries to make false requests to app and hacks the company 2's server
  As both companies have their instances inside same server, hacker can easily come inside app of company 1 as well and he can hack entire server and instances of 2nd company as well. So all the servers are hacked

- To solve this problem, VPC comes into picture. VPC is built by DevOps/AWS engineers.

- Lets in TCS, for a project devops engineer requests for VPC to AWSin Mumbai region. But what should be size of it? IP Address Range?
- While creation, AWS asks for IP Address range. Lets say 172.16.0.0/16 = 65536 IP Addresses. So we can assign the IP addresses to 65536 apps/instances

------------------------------------------------------------------------------------------

Subnets
-
- Suppose there are 3-4 internal projects inside our VPC created by DevOps engineer and engineer will split the IP addresses between them. This concept is called "SUBNET" (SubNetworking).
- This is done to assign the IP address to different apps inside our VPC
- We're splitting our IP addresses for our sub projects. Depending upon project, we can deploy as many apps or instances inside our subnet

- Now for all subnets/sub projects, devops engineer will create gate for the VPC under which subnets are created (as this is secured we need gate/gateway)
- From this gate, if anyone wants to access app inside any VPC, he has to enter. Gateway is a pass to enter to VPC of subnets (public subnets)

- There are 2 types of subnets :- Public and Private

#1. Public Subnet
- User access inside a VPC at first point. It connects to user using internet gateway
- Through internet gateway, there is a public subnet from which user enters to VPC
- Once user enters, request goes to LB which is assigned with public subnet. From LB we create target group for application. From LB, we create Router (Router Gateway/Route table in AWS) which defines how should request go to application. 
- After reaching the subnet/app, there can be another security aspect who can block the request. So for EC2 instance, there can be security groups. It asks from which IP you need to connect the request. If it is from specific one, it allows us access
- So if someone from internet (outside world) wants to access app inside private subnet, request has to go through internet gateway. Then it goes to public subnet(common subnet across VPC). Then LB which is attached to public subnet and also has target. LB takes request to private subnet and app using route tables. From LB to reach app, route table helps define path. It can go to subnet but security groups can again block its path. After allowed by security group, our request reach app

- Lets say we've Laptop on which internet is there and from there we've to reach app on one of the subnets inside VPC
  - Firstly we have to pass through internet gateway and public subnet will be there. After passing through internet gateway we have ELB there. Our request from external world reaches ELB. Now this request has to go to private subnet, there has to be proper route which is defined by route table. Route table is a path through which our request has to flow. So in ELB we'll attach private subnet and target group (Firstly we have to create target group and assign the app/instance inside subnet to that group). At the same time subnet should have route table, so traffic is flowing there. Also outside private subnet, there is security group to access or block request.
 
- Components of VPC
  - Internet Gateway
  - Public subnet
  - Load balancer
  - Route tables
  - Security groups
 
- Within same subnet to define same security group to multiple apps/EC2, NACL is used which is automation to our security groups where instead of defining same thing again and again we can define it as NACL (Network Access Control Lists)

NAT Gateway 
-
- Network Access Translation
- Request has now reached our app inside private subnet. Now what if our app tries to access something from internet. Lets say it wants to download package from internet.
- If someone tries to access something from internet, if its in private subnet, it is bad practice to expose our app IP address to internet here. The internet site should not know IP of my server (subnet).
- Here we need to do masking of our IP addresses called as "NAT Gateways"
- Here NAT will download required package from internet changing IP address of our private subnet with public IP address of LB or router
- If done by LB called as SNAT, if done by router called as NAT

- NAT Gateway does 2 things
  - Helps to download resource from internet
  - Helps to mask IP address of subnet/apps
 
- This is done so that hacker in outside world cannot know the actual IP address of our app inside private subnet


