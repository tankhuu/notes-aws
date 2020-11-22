# ECS Task Definition

Metadata in JSON Form to tell ECS how to run a Docker Container.

- ImageName
- Port Binding for Container and Host
  - Container Port: port run inside Docker Container
  - Host Port: port which traffic from internet will come in on Host or Fargate
- Memory & CPU required
- Environment Variables
- Networking information

If we want to run multiple instances of a Container just increase task number in CES Service Create/ Update.
