# Multi AZ

## EBS Multi-AZ

- EBS is tied to a single AZ
- How can we make EBS Multi-AZ?
  - Create ASG with 1 of min/max/desired attribute
  - Create Lifecycle Hooks for Terminate: make a snapshot of the EBS Volume
  - Create Lifecycle Hooks for Start: copy the snapshot, create an EBS, attach to instance
- For PIOPS volume (io1), to get max performance after snapshot, read the entire volume once (pre-warming of IO blocks)
