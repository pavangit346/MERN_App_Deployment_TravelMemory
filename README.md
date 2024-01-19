# MERN_App_Deployment_TravelMemory
Deploy a MERN Stack App on AWS EC2 With SSL, NGINX, Subdomain and Loa balancing 

Graded Project on Travel Memory Application Deployment

////////////////////////////////////////////////////////////////////////////////
Introduction:

The TravelMemory application has been developed using the MERN stack. Your challenge is to deploy this application on an Amazon EC2 instance. This will provide you with hands-on experience in deploying full-stack applications, working with cloud platforms, and ensuring scalable architecture.

Project Repository:

Access the complete codebase of the TravelMemory application from the provided GitHub link: https://github.com/UnpredictablePrashant/TravelMemory

Objective:

- Set up the backend running on Node.js.

- Configure the front end designed with React.

- Ensure efficient communication between the front end and back end.

- Deploy the full application on an EC2 instance.

- Facilitate load balancing by creating multiple instances of the application.

- Connect a custom domain through Cloudflare.

 Tasks:

 1. Backend Configuration:

   - Clone the repository and navigate to the backend directory.

   - The backend runs on port 3000. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.

   - Update the .env file to incorporate database connection details and port information.

 2. Frontend and Backend Connection:

   - Navigate to the `urls.js` in the frontend directory.

   - Update the file to ensure the frontend communicates effectively with the backend.

 3. Scaling the Application:

   - Create multiple instances of both the frontend and backend servers.

   - Add these instances to a load balancer to ensure efficient distribution of incoming traffic.

 4. Domain Setup with Cloudflare:

   - Connect your custom domain to the application using Cloudflare.

   - Create a CNAME record pointing to the load balancer endpoint.

   - Set up an A record with the IP address of the EC2 instance hosting the frontend.

 5. Documentation:

   - Prepare comprehensive documentation detailing each step of the deployment process. Include relevant screenshots to make the process clear and reproducible.

   - Design a deployment architecture diagram using [draw.io](https://www.draw.io/) to visualize the flow and connections.

Expected Deliverables:

1. A fully functional TravelMemory application hosted on an EC2 instance, accessible via a custom domain.

2. Detailed documentation of the deployment process with appropriate screenshots.

3. A deployment architecture diagram.

Evaluation Criteria:

- Accuracy and effectiveness of the deployment.

- Clarity and comprehensiveness of the documentation.

- Adherence to best practices in terms of security, scalability, and resilience.

Hints:

- While setting up with Cloudflare, remember that a CNAME record is essential for linking the load balancer endpoint. An A record, on the other hand, connects the EC2 instance via its IP address.

- ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
- ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

- 

Solution  ---- Executed through the Assignment >>


- Travel Memory
.env file to work with the backend:

MONGO_URI='ENTER_YOUR_URL'
PORT=3000

Solution:

Create an account and deploy a cluster on MongoDB Once deployed create a login credentials of read and write permission

Netwrok Access was added as 0.0.0.0/0, so that it an be access from anywhere

Use MongoDBCompass to test the login URL

Create two Ec2 instance - preferred Ubuntu - named - Satya_TM_FE, and Satya_TM_BE

Update and Upgrade OS

sudo apt update && sudo apt upgrade -y
install git on both the instances

sudo apt-get install git -y
clone the git repo

git clone https://github.com/CharismaticOwl/TravelMemory.git
Install nodejs and npm on both the instances

sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
version 18.x or later

NODE_MAJOR=18
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
Steps divided in two parts part A for backend and part B for frontend

PART - A, Deploy backend

cd into backend folder

cd TravelMemory/Backend
create .env file to work with the backend and copy the end point of db and also the port

vim .env
enter:

MONGO_URI='mongodb+srv://pavannaga:tmdbpwd@cluster0.gnvcdk7.mongodb.net/tm-db-pavan'
PORT=3000

initialize npm, install to get dependencies and then start the server

npm init -y

npm install

node index.js
Checked webbrowser, for http://localhost:3000

output should be - Cannot GET /

Which verifies that connection to DB is good to go

Installing Nginx and doing reverse proxy to http://localhost:3000

sudo apt install nginx -y

sudo systemctl start nginx

sudo systemctl enable nginx
Change the sites-available/default file

sudo vim /etc/nginx/sites-available/default
#add below in the file

location / {
    proxy_pass http://localhost:3000;
}
restart nginx server

sudo systemctl nginx

Now test public ip for EC2

http://publi_ip:80

output should be - Cannot GET /

Which verifies that load balancer is set correctly.

PART - B, Deploy frontend

cd into FrontEnd folder

cd TravelMemory/Frontend/src
change the url.js file with the new private ip where backend is deployed

vim url.js

# export const baseUrl = "http://privateIP:80"
change directory to frontend

cd ..
initialize npm, install to get dependencies and then start the server

npm init -y

npm install

npm start
Checked webbrowser, for http://localhost:3000

Installing Nginx and doing reverse proxy to http://localhost:3000

sudo apt install nginx -y

sudo systemctl start nginx

sudo systemctl enable nginx
Change the sites-available/default file

sudo vim /etc/nginx/sites-available/default
#add below in the file

location / {
    proxy_pass http://localhost:3000;
}
restart nginx server

sudo systemctl nginx

Now test public ip for EC2

http://publi_ip:80

Created multiple instances for both frontend and backend application

#1 Set VM's
(i-007430b282ebbf64c ) -- > pavan-tm-be-jan24  ( backend ) #1 
( i-0db432ff5984b6ff6 ) --> pavan-tm-fe-jan24  ( frontend ) #1 

#2 Set VM's 

( i-01b5ed4aee483f068 ) --> pavan-tm-be2-jan24 (backend ) #2
( i-060cb8d47dbf0c8f3 ) --> pavan-tm-fe2-jan24 ( frontend ) #2



BE - #1
PublicIPs: 35.170.56.66    PrivateIPs: 10.2.4.40

BE - #2
PublicIPs: 3.235.230.5    PrivateIPs: 10.2.13.73
Tested and verified that the instances

# Load Balancing - 

Below target groups attach to security groups which has open to 80/443/22/8080/3000
FETG-TMFELB-Pavan
BETG-TMBELB-Pavan

load balacer Front End( register - TG : FETG-TMFELB-Pavan) 
FELB-TM-Pavan
DNS : FELB-TM-Pavan-70310177.us-east-1.elb.amazonaws.com

Load balancer Back End ( register - TG : BETG-TMBELB-Pavan) 
BELB-TM-Pavan
DNS : BELB-TM-Pavan-1192894435.us-east-1.elb.amazonaws.com

Both backend and frontend are deployed , tested and working on the Loadbalacners.

# Renamed the Frontend App at Instance #1 as React App - 1 and renamed the App as React App - 2 from Instance #2
started NPM.
Tested the the Frontend loadbalancer, for Load balance between React App - 1 and React App- 2
tested and verified that working fine.

