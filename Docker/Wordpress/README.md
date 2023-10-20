

# WordPress Container Setup with Docker Compose

> In this document, we will guide you through setting up a WordPress website using Docker Compose. This setup includes WordPress, a MariaDB database, and phpMyAdmin for managing your database. Make sure you have a local .env file with the following properties:

•	MYSQL_DATABASE: "dbname"

•	MYSQL_USER: "username"

•	MYSQL_PASSWORD: "password"

•	MYSQL_ROOT_PASSWORD: "password"


## Prerequisites
Before you start, ensure that you have Docker and Docker Compose installed on your system.

## Docker Compose File:

Below is the Docker Compose file that defines the services for your WordPress setup:
 Url: 
## Setting up WordPress
1.	Create a .env file in the project directory with the following contents and replace with your desired values:
 

•	MYSQL_DATABASE: "dbname"

•	MYSQL_USER: "username"

•	MYSQL_PASSWORD: "password"

•	MYSQL_ROOT_PASSWORD: "password"

2.	Save the Docker Compose file as docker-compose.yml in the same directory.

        https://github.com/tinkusaini13/DevOps-Projects/blob/main/Docker/Wordpress/docker-compose.yml


3.	Open a terminal and navigate to your project directory.


4.	Run the following command to start the containers:

        docker-compose up -d
       
This will start the WordPress, MariaDB, and phpMyAdmin containers in detached mode.


1.	Once the containers are running, you can access your WordPress site at http://localhost:8080 and phpMyAdmin at http://localhost:8081. 

You can use the database credentials specified in your .env file to configure WordPress during setup.

Access the phpMyAdmin interface at http://localhost:8081 using your web browser.

 image1

 image2


 
2.	You now have a local WordPress development environment up and running.


##  Accessing Containers

## To access the containers individually, you can use the following commands:
•	Access the WordPress container:
        
        docker exec -it <wordpress_container_name> bash                

•	Access the MariaDB container:

        docker exec -it <database_container_name> bash     


## Stopping the Containers.


To stop the containers, navigate to your project directory and run:

        docker-compose down          

 This will stop and remove the containers while preserving your WordPress data in the ./wp-content directory and the MariaDB data in the db-data volume.
You now have a local WordPress development environment set up using Docker Compose. You can start, stop, and manage your containers easily for website development and testing.




         
