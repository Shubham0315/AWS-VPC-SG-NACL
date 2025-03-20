# AWS Route53

- Route 53 Provides DNS as a service on AWS (Domain Name System)

- We all use DNS on day to day basis
- When we use any app, we dont use IP addresses in real world (amazon, flipkart). We use domain names
  - e.g:- amazon.com
- In VPC, from LB user request goes to the app. These requests get assigned to IP address of app but we never use IPs in real world public app, instead we use Domain Names

- But these domain names has to be converted to IP address, which is done by DNS Service. It maps domain name with IP address of application. In real world DNS resolves domain name to the LB IP address only.
- When we create LB inside public subnet of VPC, AWS gives IP address to the LB. If user wants to access app inside private subnetthrough LB, we cannot gives him IP address of LB for 2 reasons 
  - As this address is difficult to remember and domain names are easy to remember
  - Also IP address can change anytime (we haven't assigned static IP address to LB and it gets restarted, just like public IP of EC2)
- So apps use domain names instead of IP address

- DNS keeps lot of records which maps domain names to IP address. So AWS provides this DNS as a service

- If we have app developed on personal laptop and needs to expose to world using domain name, go to GoDaddy like site which is domain name seller and buy domain name which has to be unique.  After buying domain, we also need hosting solution. Then there has to be DNS records to be maintained all the time
  - What AWS does here is, if you are hosting your app inside VPC, AWS provides us Route53
  - If user sends request to LB it has to go from Route53 before reaching LB. Route53 intercepts request, checks in its DNS records, it tries to resolve DNS name to IP address of LB and then routes to private subnet apps
 
- To configure route53
  - We need to register domain (we can get this domain name from AWS as well instead of GoDaddy)
  - Then we need to create hosted zones in which we create DNS records
  - Request is intercepted by route53 and it checks if the domain is registered, then request goes to hosted zones(public or private) where route53 looks for DNS records. If the info is met in DNS records, route53 will resolve our DNS name to IP address and then request goes ahead

- Route53 is used to:-
  - Supports health checks on web apps/web servers.
  - Domain registration can be done.
  - We can update DNS records using hosted zones
