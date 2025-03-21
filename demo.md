# VPC Demo

1. VPC Creation
- First create custom VPC.
- Security of VPC is a shared responsibility. So as we create VPC and provide IP address range, AWS will create internet gateway, NACL with default configurations, route table for us. Additionally we'll create EC2 and attach SG to it
  - After doing this we'll play with configurations of SG and NACL and see how traffic flows into the apps inside our EC2. (Allow/block in NACL/SG)
 
<img width="674" alt="image" src="https://github.com/user-attachments/assets/85eed347-f555-47f5-9d76-a16dc08b6bc4" />

- Now go to VPC service in AWS.

![image](https://github.com/user-attachments/assets/5b5abe7f-8e76-43df-ace2-90e0a823fcc6)

- Create VPC. There're 2 options :- VPC only, VPC and more.
  - Go for VPC and more as AWS creates default resources for us.
  - Here choosing this option AWS will create public subnet and private subnet for us in both the availability zones of our region.So we dont need to create configuration ourselves
  - AWS also creates route tables for us
  - It also create "Internet Gateway" and "VPC endpoints".
 
![image](https://github.com/user-attachments/assets/1d0440c7-1cd1-44b9-b8fd-2dd6dbb56ce5)

- Provide name to VPC. Below it we can see CIDR range for IPs.

<img width="965" alt="image" src="https://github.com/user-attachments/assets/d69996b8-3de3-4c19-9ab2-359967319920" />

- Set the no. of availability zones. Set public subnets number as we're doing everything with it only.

![image](https://github.com/user-attachments/assets/7c716e45-9ae7-409e-86ac-0d27a6268fdf)

- Create VPC. VPC will be created.
  - First VPC will be created
  - Configures DNS
  - Subnet created (public and private)
  - Internet gateway will be created
  - Created route tables. Associated route tables with public and private subnets.
 
![image](https://github.com/user-attachments/assets/1a06e76c-85b8-496f-9a02-940dee22ce15)

- We can now go inside our VPC to check how it looks like

![image](https://github.com/user-attachments/assets/8042ac05-b174-4855-9b26-284191304cc4)

2. EC2 Creation
- Now we've to place EC2 in public subnet of VPC and demonstrate SG and NACL
- Inside network settings, select VPC created (not default one). AWS allocates private subnet for us as by default our EC2 and apps should be in private subnet. If we've to change the subnet we can do that as well

![image](https://github.com/user-attachments/assets/9a70adfd-18ce-4c6c-ac13-fc2c1098cb9e)

- Assign public IP - Enable.
- Create SG

![image](https://github.com/user-attachments/assets/5ea18af3-9275-47f7-a98b-cb0b28ba5019)

- Now launch instance

3. Install Application inside EC2
- Install python app inside EC2 and run it on port 8000. Here when we try to access the app, SG will block the request as default SG created by AWS will not allow traffic directly. So we need to enable the traffic

- Go inside instance, get public IP address and connect to the instance. Login and update packages of python
  - Command :- **sudo apt update**
  - To check python installed :- **python3**
  - To run http server on python :- python3 -m http.server 8000
  - So now simple http server is running on port 8000.
  - Now when we check application is accessible through browser, it is not :- **http://publicIP:8000**
  - This is as we havent specified traffic
  - In the default SG, we've only allowed port 22. This is as AWS by default only allows SSH traffic to login to EC2 instance
 
![image](https://github.com/user-attachments/assets/04d8687e-efed-4980-aeab-619697583e32)

  - Check NACL attached to our VPC and go to inbound rules as well (coming to our app)
  - It says all traffic is allowed. Deny condition will be triggered only if allow condition is not met.
  - We can also set allow only custom TCP traffic for port 8000 (not here)

![image](https://github.com/user-attachments/assets/10ec64a6-26bc-41cd-9757-93e8ad94945f)

- So now through internet gateway we got entry into subnet (pictorial diagram below). As NACL is allowing all traffic so internet gateway can forward request to route table as NACL accepts all traffic. Route table will forward request to EC2
  - But here request doesnt go to EC2 as SG is blocking it
  - First layer NACL is cleared but 2nd layer which is SG blocks request
 
![image](https://github.com/user-attachments/assets/7f08fe49-eeef-440c-a9f7-40fd9264317a)

  - To unblock go to SG - Edit Inbound Rules - Add rule - Allow port 8000 from anywhere - Save rule

![image](https://github.com/user-attachments/assets/d031653b-f744-41bf-83e5-0df72d866305)
![image](https://github.com/user-attachments/assets/7e2ae4ed-36e5-475b-8977-60b87961adcd)

- Now if we try to access website we can access it

![image](https://github.com/user-attachments/assets/1ebe40c6-fc08-4927-ac0b-50388e315591)


- If we've enabled port 8000 but its against the organization policy. We can control this at NACL level itself.
- As we cannot monitor each EC2, we can say for my VPC we'll block port 8000 at NACL
- NACL - Edit inbound rule - Custom TCP port 8000 - Deny
- If we do this, our request wont reach the application and application is not accessible
- As we've blocked it at NACL so it will be blocked at entire subnet (refer diagram)

![image](https://github.com/user-attachments/assets/83296b10-5e4e-4b7c-b4be-752b7b4c6a6b)

- Now lets add new rule keeping existing ones where deny traffic at NACL
- Still app is accessible as NACL goes in specific order of rules. First 100 will execute then 200 will.
- If first rule is met AWS will forward request to SG and there also if we've allowed port 8000, the request will reach our application

![image](https://github.com/user-attachments/assets/52b9247f-56a7-4324-ae35-6b7adcea698d)

- To change order we can re-number the rules like below so which we want to get executed first, it will go up the order
  - Now request wont reach to our EC2 as we've blocked specific port.

![image](https://github.com/user-attachments/assets/f4e41d48-4eec-4a32-a59e-5c8845a85871)
![image](https://github.com/user-attachments/assets/8efe7e58-7e4f-45cb-88d1-254266e0e8f8)


** Even if we allow configurations in SG, NCAL acts as first layer of defense. DevOps engineers/network engineers who have access to NACL can block configurations at subnet level
-
