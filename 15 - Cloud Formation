1. Create one script yml file with correct ami id, and with ssh-key already present in device.
2. cloud formation >> create stack >> choose existing template >> upload template file >> choose yml file >> next >> next >> next
3. Check stack created and running successfully >> one ec2 instance will be automatically created
4. To jump on that EC2 instance we have to create an jump-pad instance using ssh-key in system
     - vim key-name.pem
     - copy ssh-key
     - chmod 400 key-name.pem
     - ssh -i "key-name.pem" ec2-user@private-ip-addr

  Successfully cinnected to the cloud formation instance!
