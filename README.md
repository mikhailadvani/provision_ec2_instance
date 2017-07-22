provision_ec2_instance
-------

[![Build Status](https://travis-ci.org/mikhailadvani/provision_ec2_instance.svg?branch=master)](https://travis-ci.org/mikhailadvani/provision_ec2_instance) [![Galaxy](https://img.shields.io/badge/ansible--galaxy-mikhailadvani.provision_ec2_instance-blue.svg)](https://galaxy.ansible.com/mikhailadvani/provision_ec2_instance)


Ansible role to provision EC2 instances with security group(s) and elastic IPs

### Required variables

Setup the following vars and include them in the playbook before execution

    api_keys:
      ec2:
        aws_access_key_id: AWS_ACCESS_KEY_ID
        aws_secret_access_key: AWS_SECRET_ACCESS_KEY
    aws_region: us-east-1
    instance:
      tags: { "Name" : "Instance-1" }                                           #Dictionary of tags to be associated to the instance
      count: 1                                                                  #Number of instances with the particular tags to be created
      ami_id: ami-e6f48789                                                      #The AMI ID to be used while provisioning the instance
      instance_type: "t2.micro"                                                 #The type of the instance
      assign_public_ip: yes                                                     #Is public IP to be assigned
      key: "key-1"                                                              #The name of the SSH key pair
      subnet_name: "public-1"                                                   #The name of the subnet where the instance is to be launched
      elastic_ips:                                                              #List of static IPs to be assigned to created instances. Number of IPs pre-allocated should be equal to the instance count 
        - 1.2.3.4
      volumes:                                                                  
        - EBSSpecifications:                                                    
            volume_size: 8                                                      #The size of the volume
            volume_type: gp2                                                    #The type of the volume
            device_name: /dev/sda1                                              #The device id of the volume
            delete_on_termination: yes                                          #Whether the volume needs to be retained after instance termination
    
    security_groups:
      - name: security_group_1                                                  #Name of the security group
        description: Security group description                                 #Description of the security group
        rules:                                                                  #Inbound security group rules
          - {proto: tcp, from_port: 0, to_port: 65535, cidr_ip: "0.0.0.0/0" }
        rules_egress:                                                           #Outbound security group rules
          - {proto: tcp, from_port: 0, to_port: 65535, cidr_ip: "0.0.0.0/0" }
             
Additionally, a folder named `keys` with `id_rsa` and `id_rsa.pub` are needed for key pair creation          

### Example Playbook

    - hosts: localhost
      roles:
         - { role: mikhailadvani.provision_ec2_instance }

License
-------

Apache    
