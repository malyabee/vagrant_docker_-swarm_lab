# vagrant_docker_-swarm_lab
A vagrant file to build Docker_swarm_lab using CentOS7




This Vagrant file will create three VMs and install docker on all VMs. 

We are going to use one of the VM as master node.

###Command to setup Swarm 

####On manager node
to setup node as master node we need to run  

     docker swarm init --advertise-addr 192.168.22.10
     
     
####On worker nodes
   need to run the out put of above command

We can create a service with single replica 

    docker service create -d --name=nginx_service -p 80:80 --replicas 1 nginx:latest

  
  
docker node promote node01.example.com
  
  
  
  
  
  
on manager node
    docker node ls

   docker service ls
  
  
   docker service ps nginx_service
   
 Commands to scale services   
   docker service scale nginx_service=3
   docker service ps nginx_service
   docker service scale nginx_service=5
   docker service ps nginx_service
   docker service scale nginx_service=7
   docker service ps nginx_service
   docker service scale nginx_service=8
   docker service ps nginx_service
   
   
   verification 
   curl 192.168.22.11:80
