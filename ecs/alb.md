# ECS Service Load Balancer

Allow access ECS Service from the internet with a feature called dynamic port forwarding which basically will route the traffic to the random port.
Dynamic port forwarding help to run multiple same task on the EC2 instance.

By updating Task using dynamic port forwarding, create new revision of Task Definition -> Container -> Port mapping -> just map the Container port, leave the Host port empty.

> Load Balancer can only be set on service creation, so if we want to add load balancer for a service, we have to recreate it with a load balancer

> To allow dynamic port forwarding on EC2 Instance, we need update the Security Group to allow All Traffic from the SG of Load Balancer
