
# Full-Stack Secure Deployment on AWS
                                         
React + Django app deployed to AWS using Docker, ECS, ECR, and Application Load Balancer — with a security-first architecture.


#Overview -

This project demonstrates a production-grade deployment of a full-stack web application on AWS with:
- Dockerized React (frontend) and Django (backend)
- Private ECR repositories
- Secure ECS Fargate setup with Task Definitions
- Application Load Balancer with path-based routing
- Strong Security Group and IAM policy configurations


Deployment Steps (summarized)
List the steps:
1. Dockerize frontend and backend 
2. Push images to AWS ECR
3. create a ecs cluster for both frontend and backend
4. create a rds postgresql database with a security group attached to it
5. Create a blue print with ECS Task Definitions
6. Set up Security Groups
7. Configure ALB and Target Groups
8. create a namespace for swift api communication
9. make sure the backend is configured to communicate with the data base and the frontend is configured to communicate with the backend
10. create a service for the backend first deploy
11. create a service for the frontend then deploy 
12. test it by acccessing the frontend alb dns



here is a little diagram overview

[ Client (Browser) ]
         ↓ HTTPS
[ Application Load Balancer ]
       ↙           ↘
[React Frontend]   [Django Backend]
   (Fargate)          (Fargate)
     ↓                   ↓
[Public ECR]       [Private ECR]
                      ↓
               [PostgreSQL DB]
                    (AWS RDS)


                                            here are the security messures 

1. attached security groups to my rds data base -
   inbound (most important) -  only allows the backend to communicate it with
   outbound - it can only communicate with my ip address 

inbound screen-short -
<img width="938" alt="inbound" src="https://github.com/user-attachments/assets/d27f974c-d8e2-465c-b9cc-992cdf920667" />

outbound screen short - 
<img width="923" alt="outbound" src="https://github.com/user-attachments/assets/af9b0145-dec2-44ba-b5f3-e4d381b45d1a" />

2. attached security groups to my backend -
    inbound(most important) - it only allows frontend communicate with it
    <img width="926" alt="backend-inbound" src="https://github.com/user-attachments/assets/b4b27f7a-bc9b-48a7-996a-5337aa7e92d7" />

    outbound - this allows it to communicate with the internet and also postgresql for storing data
   <img width="935" alt="backend- outbound" src="https://github.com/user-attachments/assets/eb7ca9b0-9b31-4231-9092-65c8e6a99e81" />

  
3. Network Security
Backend (Django) is not exposed to the public internet

Configured Security Groups:
Frontend allowed only HTTP/HTTPS
Backend only accepts traffic from the Application Load Balancer
Used a VPC to isolate all services

4. IAM (Identity and Access Management)
Created least-privilege IAM roles for ECS task execution

Used scoped-down IAM policies for ECR, S3, CloudWatch, etc.

Avoided hardcoding credentials; used AWS IAM roles with automatic credential injection

5. Container & Image Security
Built Docker images with minimal base images (e.g., python:3-slim, node:alpine)
Stored images in private ECR repositories
Avoided exposing Docker secrets/environment variables in the image

here is an image- 
<img width="939" alt="private repostories for frontend and backend" src="https://github.com/user-attachments/assets/3298a039-289f-4733-ad0f-55b36a357e05" />


5. Load Balancer and Routing
only attached a load balancer to the frontend and deployed the backend locally
Used HTTP (SSL/TLS) via Application Load Balancer (ALB)
Configured path-based routing to isolate frontend (/) and backend (/api)
Enforced namespace isolation for services

here is an image -
<img width="958" alt="frontend balancer" src="https://github.com/user-attachments/assets/7271f3cf-cca2-41d1-aac6-9df8a6d028a4" />



7. Infrastructure & Deployment
Used ECS Fargate to avoid managing EC2 (reduces attack surface)
<img width="938" alt="fargate screenshot" src="https://github.com/user-attachments/assets/0f5a6fbf-7d0a-44ae-952d-7cefc637858c" />

No public SSH/RDP access to any compute resource
Separated frontend and backend tasks for better access control

7. Data Security
Connected backend securely to PostgreSQL (RDS) in a private subnet
Used parameterized queries or ORM in Django to prevent SQL injection

8. added namspace for my backend to enable swift apu call between my frontend
    <img width="955" alt="swift api call" src="https://github.com/user-attachments/assets/41098824-135f-4cdc-8353-4be70ac2aa96" />
  


8. Monitoring 
Enabled CloudWatch Logs for ECS tasks
<img width="935" alt="frontend usage" src="https://github.com/user-attachments/assets/567c3c7c-8de2-4e82-812b-0f6fe7ab6679" />

 Considered setting up GuardDuty or AWS Config for threat monitoring


here is an overall diagram of my project
  
  ![diagram](https://github.com/user-attachments/assets/95a2ae3c-212a-4675-aa6f-ec3c146b1013)


here is the pic of the website 

<img width="957" alt="hosted website" src="https://github.com/user-attachments/assets/bbe35d02-aa39-4689-8fc0-060a90129e34" />



I'm a Cloud Security Engineer interested in secure deployments, DevSecOps, and scalable cloud architecture. Connect with me on [LinkedIn](www.linkedin.com/in/uchime-victor-2b5b3131b).
