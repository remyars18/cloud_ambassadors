# Set Up a Virtualized Environment Using Docker

This project illustrates the process of setting up an Nginx web server within a Docker container, featuring a personalized `index.html` file. The container displays a simple page that reads "Welcome to Nginx Container." 

## Setup Instructions
Deploying a Dockerized Environment on AWS EC2
1. Set Up an EC2 Instance
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

2. Connect to the Instance
      Use your key pair to SSH into the instance by below bash command;
   ```bash
      ssh -i your-key.pem ec2-user@<public-ip of the ec2 instance>
3. Install Docker on the EC2 Instance
     Update the package manager and install dependencies:For Amazon Linux 2:use below bash commands:
   ```bash
   sudo yum update -y
   sudo yum install docker -y             # install the docker
   sudo systemctl start docker            # start the docker service
   sudo systemctl status docker           # check the status of the docker
   sudo usermod -aG docker ec2-user      # add ec2-user to docker group . Adding ec2-user to the docker group gives it the necessary permissions without requiring sudo.
   newgrp docker                         # activates the changes otherwise exit and login again6. 
   

4. On your EC2 instance, create a directory for the project:
    ```bash
    mkdir nginx   # Create a directory named nginx
    cd nginx      # Change to the nginx directory
    ```

5. create and edit the dockerfile inside the directory`:
    ```bash
    nano Dockerfile   # Create a file named Dockerfile using the Nano text editor
    ```

    Add the following content to the `Dockerfile`:
    ```dockerfile
    FROM nginx:latest                        # Use the official Nginx image as a base
    COPY index.html /usr/share/nginx/html/   # Copy custom index.html to the Nginx default web directory
    EXPOSE 80                                  # Expose port 80 for the container to allow traffic to the web server
    CMD ["nginx", "-g", "daemon off;"]        # Command to run in foreground when the container starts
    ```

6. Save and exit the editor (`Ctrl + O`, Enter, `Ctrl + X`).

7. Create and edit the `index.html` file:
    ```bash
    nano index.html  # Create a file named index.html using the Nano text editor
    ```

    Add the following content to the `index.html` file:
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Welcome to Nginx!</title>
    </head>
    <body>
        <h1>Success! Your Nginx server is running.</h1>
    </body>
    </html>
    ```

8. Save and exit the editor (`Ctrl + O`, Enter, `Ctrl + X`).


9. Build and Run the Docker Container:
    ```bash
    docker build -t nginx-container .                   # Build the docker image named nginx-container from dockerfile
    docker run -d -p 3006:80 nginx-container            # Runs a docker container , mapping port 3006 on your host machine to port 80 inside the container
    docker ps
     ```                                        # Verify that the container is running


10.  Configuring Security Group for Host Port 3006:
      Log in to your AWS account and go to the EC2 Dashboard and  and select the ec2-instance which you have created and  navigate to Security Groups under Security section.
      Find and select the Security Group attached to your EC2 instance and Click the Inbound rules tab and then click Edit inbound rules.
      In the Inbound rules section, add a new rule:
                                 -- Type: Custom TCP
                                 -- Port Range: 3006
                                 -- Source:
                                         Select your IP to allow access only from your current IP address.
   Save the rule
11.   Access the Service:
           In the AWS EC2 Dashboard, locate your instance and Copy its **Public IPv4 Address**.
12.  Test the Service:
   open a browser and visit the following URL:
   public-ip:3006  
Replace `public-ip` with your actual EC2 public IP address.
 
  You should see the "Welcome to Nginx Container!"

   
    
     


   
   
