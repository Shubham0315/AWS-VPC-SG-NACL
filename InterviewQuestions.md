What is an AWS VPC and why is it important in cloud networking?
-
- VPC is logically isolated section of AWS cloud where we can launch our AWS resources in virtual private network
- We control IP ranges, subnets, routes, network gateways creating private network in cloud

----------------------------------------------------------------------------------------------

What’s the difference between a public and private subnet?
-
- Public Subnet
  - Can access and be accessed from the internet
  - Has a route IGW
  - Used for frontend, LBs
  - Usually assigned for instances
 
- Private Subnet
  - No direct access to and from internet
  - No route to IGW
  - Used for backend apps, DBS

----------------------------------------------------------------------------------------------

How do you connect a VPC to the internet?
-
- Create or use public subnet in desired AZ.
- Assign proper CIDR block
- Attach IGW which is horizontally scaled component which allows internet traffic in and out of our VPC
- Update route table for public subnet
- Assign public IPs to EC2 instances
- Configure SG and NACL

----------------------------------------------------------------------------------------------

What is VPC Peering?
-
- It is network connection between 2 VPCs (within same or different AWS accounts) which allows them to communicate privately using their private IP addresses
- Its like creating private bridge between 2 VPCs

- Traffic stays within AWS, never goes over public internet
- Used to connect dev/test and prod env securely

- To setup VPC peering
  - Create VPC peering connection from one VPC to another
  - Accept VPC peering request in target VPC
  - Update route tables in both VPCs to route traffic to each other via peering connection
  - Modify NACLs and SGs to allow traffic

----------------------------------------------------------------------------------------------

How is Transit Gateway different from VPC Peering?
-
- Use transit gateway when we need to connect more than 10 VPCs. if you want centralized routing and security amd multi-region, multi-account network is required
- Peering is not scalable but transit is.

----------------------------------------------------------------------------------------------

Can a subnet span multiple AZs?
-
- No
- Each subnet in VPC is strictly tied to single AZ. This is to ensure HA and fault isolation.
- If we want resources in different AZs, we need to create multiple subnets, one per AZ

----------------------------------------------------------------------------------------------

How do you set up a highly available architecture in a VPC?
-
- Use multiple AZs, at least 2 for data redundancy. Create one subnet per AZ
- Deploy LBs across AZ. Use ALB or NLB. ALB auto routes traffic across healthy targets in multiple AZs
- Launch EC2 in multiple AZs via ASG
- Use NAT gateways for each AZ

----------------------------------------------------------------------------------------------

How does VPC Flow Logs help in security?
-
- VPC flow logs capture info about IP traffic going to and from network interfaces in our VPC
- This act as Network activity log
- It logs details like :- IP, ports, protocoals, action, traffic

- It helps detect unauthorized access
- Monitor inbound and outbound traffic
- Verifies SG and NACL rules
