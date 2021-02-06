# SSM Hybrid Activations

Centrally manage hybrid environment
Create an activatin to register on-premises servers and virtual machines (VMs), non-AWS Cloud Servers, and other devices with SSM.

- Create an Activation (prefix `i-` is for EC2, `mi-` is for hybrid instances.)
- Get Activation Code & Activation ID
- Register instance: `sudo amazon-ssm-agent -register -code "Activation_Code" -id "ActivationID" -region "region"`
- Start the SSM Agent: `sudo systemctl start amazon-ssm-agent`
