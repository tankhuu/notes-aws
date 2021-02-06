# Notes for AWS VPC Peering

`Requester` is VPC that you own and `Accepter` VPC with which to create the connection.
Accepter can belong to another AWS Account and can be in a different Region to the requester VPC.
The requester VPC and accepter VPC cannot have overlapping CIDR blocks.
