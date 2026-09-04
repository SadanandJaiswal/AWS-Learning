# AWS Route 53
It is a scalable DNS service for domain registration, traffic routing and health checking capabilities.

We access our EC2 instance using its IP, but its hard to remember so we use domainname (a simple name mapped to your instance ip)

**note** : Not available in AWS free tier


### DNS (Domain Name System)
It is a internet service that translates human friendly domain names like wwww.example.com into machine readable ip addresses.

**Default port**: for DNS is **53**


### Practical
- Create EC2 instance > copy the public IP of instance
- Register a Domain (gets Hosted Zone) or Use already present zone (need to create hosted zone)
- Create Hosted Zone
    - Provide Domain Name (saddy-tech.com)
    - Type : Public Hosted or Private Hosted
    - Create Hosted zone (gets NS (name server))
    - Update the Nameserver for your domain (if domain is out of AWS)

User type saddy-tech.com in browser > nameserver > route 53

Now came to router 53, now whats next? need rule for next (records)

- Create records : Record name, type (Type-A : convert to IPv4)
- Routing Policy : Simple routing (get request send to the instance)


If have multiple instance (instance1-eu, instance2-mumbai, instance3-us)
- Select the hosted zone
- Select Record (saddy-tech.com) Type - A
- Edit Record
- Routing Policy (Latency, Geolocation, Failover, Weighted, etc) : Latency
- Select Region (Mumbai) and Record ID (any : Asia Record)
- save record

Create another Record for another region
- Create record > Record Type : A > paste IP (13.60.30.74) > Routing type : Latency > Region : Europe > create record
- Create record > Record Type : A > paste IP (13.60.20.74) > Routing type : Latency > Region : USA > create record


### HealthCheck
- Open route 53 > Sidebar > Health Check > Create Health Check

**note** : Its Region Specific

- Name, Endpoint, IP address (13.60.30.74), Request Interval (standard : 30sec), create health check
- Use the healthcheck in record for all the instance record of there specific region  


### Flow Summary
- Domain Name Creation : Register a domain and point it to AWS route 53
- Hosted Zone Creation : Create a hosted zone to manage DNS records
- DNS Records : Add records to route traffics to various endpoints
- Routing Policies : 
- Health Checks : Configure health checks to monitor ednpoints


### Types of Record supported by Route 53
- A : map domain name to ipv4 address
- AAAA : map domain name to ipv6 address
- CNAME : map a domain name to another domain name (alias)
- MX : Directs mail to an email servers
- TXT : 
- NS : 
- SRV : 


### Usecase
- Hosting Websites : Manage domain name and route traffic to web applications
- Load Balancing : Distribute traffic accross multiple endpoints using weighted or latency-based routing
- Disaster Recovery : Use health checks and failover routing for high availability
- Multi-Region Development : Route traffic to the closest region for low latency