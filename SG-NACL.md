# Security Groups and NACL

- Inside VPC, there can be multiple projects. For each project we can define private subnets. Private subnets will not have access to internet. So when user wants to access application, request has to go through public subnet with which devops engineer places LB. LB talks to private subnet and user has access to LB.
- Once LB forwards request to private subnet, using AWS we can add additional security at the subnet level.

- Here we can add securrity at subnet level as well as EC2 instance level where application is actually deployed
  - If we add security at EC2 instance level then it is called as Security Groups
  - If we add security at subnet level then we call it as NACL (Network Access Control Lists)
 
- For every organization if they want to move to public cloud, they will see if public cloud is secured or not as they put their apps, user details, data on the cloud.
- In AWS, security is shared responsibility of VPC, Security groups, NACL, DevOps/AWS admins whether we want to add LB, API gateways, configure SG/NACL.
- Security groups and NACL are the last point of our security aspect and also the most critical as if we dont configure them in right way, our application can be easily hacked as its point where request reaches the application.

---------------------------------------------------------------------------------------------------

Difference between SG and NACL
-
- SG serves at instance level.
- When we create AWS account, AWS gives default VPC without which AWS wont deal with anything. We can also create custom VPCs. 
  - Here we create EC2 and try to deploy an application. By default we cannot access the application as AWS doesnt allow any traffic into our instance directly. So we need to configure security groups. Here to access, we can allow port or allow all traffic. We as devops engineers only allow specified traffic into EC2 which is done by SG in AWS
- Using Security groups, we can add more security to EC2 instance level whereas Using NACL, we can add more security at subnet level

- Security group is only for allowing traffic(doesnt have deny) where as NACL will deny traffic and add only specific traffic to EC2

---------------------------------------------------------------------------------------------------

Security Groups
-
- In SG, there are 2 things Inbound traffic and outbound traffic.
  - When we deploy app into EC2 instance, there are 2 activities. User tries to access application and app tries to access internet.
  - Traffic flowing into application is called inbound traffic
  - Traffic flowing outside of our application is called outbound traffic
 
- If we try to access amazon as user it is Inbound traffic. For payment if amazon tries to access google pay it is outbound traffic.
- As part of SG we have to manage both traffic, traffic incoming to EC2 and traffic outgoing from EC2

- When we create EC2, AWS assigns default SG as AWS takes care of security.
  - By default AWS will allow all outbound traffic except port 25 and denies every inbound traffic by default. Here we can restrict or do specific configurations in outbound traffic
  - AWS doesnt allow the outbound traffic default on port 25 as port 25 is mailing service to avoid spamming activities/record IPs of EC2. So it blocks port 25.

---------------------------------------------------------------------------------------------------

NACL (Network Access Control Lists)
-
- It goes a level beyond EC2, applied at subnet level. (SG applied at EC2 level)
- Suppose, we gave EC2 instance to dev team1. Deployed the application and to make process quicker they allowed all the traffic to their EC2 from all IPs instead of just exposing specific port for the application.
- But this team ignored the security aspect of AWS as SGs are meant to open only specific ports and allow only specific traffic to EC2
- As devops engineer we have one more level of security at subnet level like what kind of traffic we want to DENY. So even if we try to access it wont be accessible
- NACL is used by devops/AWS admins to define their organisational network traffic. So if anything is applied to subnet level by default it is applied to all instances within subnet
- Lets say we have 50 EC2 in subent and we said allow all traffic. But if we applied NACL and by default deny all traffic and allow only specific as per NACL, AWS will deny all traffic flowing in and only allow configuration defined in NACL
- SO NACL will get added as additional layer of security

** Instead of applying configuration for each EC2 instance we can use NACL for automation.
- Instead of adding SGs to every EC2, if we apply NACL to subnet, the configuration will get applied to all EC2 instances inside subnet(50k as well). We can reduce manual activity of assigning rules to each EC2
- Its onto us if we want to use SG or NACL or combination of SG+NACL

** Security group is only for allowing traffic(doesnt have deny) where as NACL will deny traffic and add only specific traffic to EC2
-
---------------------------------------------------------------------------------------------------
