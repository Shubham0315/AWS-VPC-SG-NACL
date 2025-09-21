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

----------------------------------------------------------------------------------------------

Difference between a default VPC and a custom VPC.
-
- Default VPC is auto created in every AWS region while custom needs to be created manually
- Default has 1 public subnet per AZ while in custom user defines subnets
- Default comes with IGW attached, in custom we need to create and attach IGW
- All things same like above

----------------------------------------------------------------------------------------------

Difference between NAT Gateway and NAT Instance and when to use each.
-
- NAT gateway fully manages by AWS, highly available within AZ, auto scales to handle traffic with low latency
- NAT instance is user managed EC2 instance, manual scaling is required

- Use NAT gateway in production for private subnets to allow internet access (e.g., updates, API calls) with minimal management.
- Use NAT instance for small-scale or specialized needs where custom configuration (firewall rules, monitoring) is required.

----------------------------------------------------------------------------------------------

How do you route traffic between subnets, VPCs, and on-prem networks?
-
- Route tables to route traffic between subnets in same VPC
- Use VPC peering to establish peering connection between 2 VPCs
- Use transit gateway for multiple VPCs
- Ensure SG and NACL allow traffic

----------------------------------------------------------------------------------------------

How do Elastic IPs work in VPC, and when are they required?
-
- Elastic IP is static, public IPV4 address that we can aloocate to AWS account and associate with EC2 in VPC
- Assign IP to EC2 in public subnet. Internet traffic directed to that EIP reaches associated resource
- They're required when NAT gateway in private subnet needs EIP to access internet

- An Elastic IP is a static public IPv4 address that stays consistent across instance restarts and can be reassigned, required when persistent internet access or failover capability is needed for EC2 instances or NAT Gateways.”

----------------------------------------------------------------------------------------------

How do you configure VPC endpoints for S3 or DynamoDB to avoid using the internet?
-
- VPC endpoint allows private connectivity between VPC and AWS services without using public internet
- VPC - Endpoints - Create - Select service S3 and DynamoDB - Select VPC and route tables to update

- I configure a VPC Gateway Endpoint for S3 or DynamoDB by creating the endpoint in the VPC, associating it with route tables, and optionally applying an endpoint policy, enabling private connectivity without using the internet.”

----------------------------------------------------------------------------------------------

A public subnet EC2 instance cannot reach the internet. How would you troubleshoot?
-
- Check subnet route table, ensure it has route
- Verify IGW is attached to VPC
- Check public IP assignment
- Verify SG, NACL, DNS settings

----------------------------------------------------------------------------------------------

How do you handle overlapping CIDR blocks in VPC peering?
-
- VPC peering doesnt allow overlapping CIDR blocks. If IP ranges of 2 VPC overlap, AWS will reject peering connection
- Resize or redesign CIDR block of one VPC to non overlapping range
- Use NAT or transit gateway to translate IPs, allowing communication despite overlap

----------------------------------------------------------------------------------------------

How do you secure VPC resources from unauthorized access while allowing CI/CD pipelines to deploy?
-
- Use IAM roles for CICD pipelines with least privilege
- Deploy resources in private subnets
- Use SG and NACL
- Store secrets in Secrets manager

- I secure VPC resources by using private subnets, IAM roles for CI/CD pipelines, VPC endpoints, restricted security groups, and secrets management, while monitoring all access with CloudTrail and VPC flow log

----------------------------------------------------------------------------------------------

How do you monitor and audit network traffic in a VPC for compliance?
-
- Enable VPC flow logs to capture incoming and outgoing traffic in VPC. Can be sent to cloudwatch logs or S3 for storage
- Monitor and audit VPC network traffic using VPC Flow Logs for traffic capture, CloudTrail for API activity, CloudWatch for metrics and alarms, and AWS Config rules to enforce compliance policies
