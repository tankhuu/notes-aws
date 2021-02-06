# Service Auto Scaling

Note that this auto scaling doesn't relate to the EC2 Instance Autoscaling
-> ECS SErvice will autoscale but the EC2 Instance to host those containers won't be autoscale (not include Fargate)
-> If we want EC2 Instance Autoscale too, we can use Elastic Beanstalk for Docker to apply autoscaling on both ECS Service & EC2 Instances (using Multi-Container Docker platform in Base Configuration)
