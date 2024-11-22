# Set Up a Virtualized Environment Using Docker

This project illustrates the process of setting up an Nginx web server within a Docker container, featuring a personalized `index.html` file. The container displays a simple page that reads "Welcome to Nginx Container." 

## Setup Instructions
Deploying a Dockerized Environment on AWS EC2
1.  # Set Up an EC2 Instance
    Log in to your AWS Management Console.
    Navigate to EC2 Dashboard → Launch Instances.
    Configure your instance:
                          AMI: Choose an Amazon Linux 2 AMI (or Ubuntu).
                          Instance Type: Choose a t2.micro instance (free tier eligible).
                          Key Pair: Create or select a key pair for SSH access.
                          Security Group: Configure inbound rules:
                                          Allow SSH (Port 22) from your IP address.
                                          Allow HTTP (Port 80) from anywhere (0.0.0.0/0).
                         Launch the instance.
2. # Connect to the Instance
      Use your key pair to SSH into the instance by below bash command;
     --- ssh -i your-key.pem ec2-user@<public-ip 0f the ec2 instance>
3. # Install Docker on the EC2 Instance
     Update the package manager and install dependencies:For Amazon Linux 2:use below bash commands:
   ---  sudo yum update -y
   --- sudo yum install docker -y             # install the docker
   --- sudo systemctl start docker            # start the docker service
   --- sudo systemctl status docker           # check the status of the docker
   ---  sudo usermod -aG docker ec2-user      # add ec2-user to docker group . Adding ec2-user to the docker group gives it the necessary permissions without requiring sudo.
   ---  newgrp docker                         # activates the changes otherwise exit and login again
4. # Deploy the Dockerized Nginx Environment
    On your EC2 instance, create the Dockerfile and index.html in a directory:
   --- mkdir nginx  # creat a directory named nginx
   ---  cd nginx    # change the directory
     create the dockerfile inside the directory:
           --- nano dockerfile #  create a file named dockerfile using the Nano text editor.
     # add the below contents:
         FROM nginx:latest                        ## Use the official Nginx image as a base
         COPY index.html /usr/share/nginx/html/   ## Copy custom index.html to the Nginx default web directory
         EXPOSE 80                                ## Expose port 80 for the container to allow traffic to the web server
         CMD ["nginx", "-g", "daemon off;"]       ## Command to run in foreground when the container starts
   then pres ctrl + o enter ctr + X               ## Save and quit from nano editor

   create an index.html in nanoeditor :
   --- nano index.html
   # add the below content
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to Nginx on AWS EC2</title>
</head>
<body>
    <h1>Welcome to Nginx on an AWS EC2 Container!</h1>
</body>
</html>

5. # Build and Run the Docker Container
   --- docker build -t nginx-container .                   # Build the docker image named nginx-container from dockerfile
   --- docker run -d -p 3006:80 nginx-container            # Runs a docker container , mapping port 3006 on your host machine to port 80 inside the container
   --- docker ps                                          # Verify that the container is running


6. # Configuring Security Group for Host Port 3006
      Log in to your AWS account and go to the EC2 Dashboard and  and select the ec2-instance which you have created and  navigate to Security Groups under Security section.
      Find and select the Security Group attached to your EC2 instance and Click the Inbound rules tab and then click Edit inbound rules.
      In the Inbound rules section, add a new rule:
                                 -- Type: Custom TCP
                                 -- Port Range: 3006
                                 -- Source:
                                         Select My IP to allow access only from your current IP address.
   Save the rule
7. # Access the Service
       Retrieve the Public IP
           In the AWS EC2 Dashboard, locate your instance and Copy its Public IPv4 Address.
8. #  Test the Service
     Open a browser and visit
        <public-ip>:3006
   You should see the "Welcome to Nginx Container!"

   
    
     


   
   
