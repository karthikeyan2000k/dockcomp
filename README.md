

# Deploying Project in docker

1. Create a new EC2 instance and Login in MobaXterm

2. Install Docker

        sudo yum update -y
        sudo yum install docker -y
        sudo systemctl start docker

        #Give ec2-user permission for docker
  
              sudo usermod -a -G docker ec2-user

        sudo systemctl enable docker
        sudo systemctl status docker

3. Install git

        sudo yum install git 

        git clone https://github.com/Padma-U/CreditCardBillPaymentApp.git

4. Build an Image for our project

        cd <project_name>

        sudo vi Dockerfile

        # Dockerfile

                From openjdk:8

                WORKDIR /app

                ADD build/libs/CreditCardPayment-0.0.1-SNAPSHOT.jar .

                EXPOSE 8086

                ENTRYPOINT ["java","-jar","CreditCardPayment-0.0.1-SNAPSHOT.jar"]

        sudo docker build -t app .

5. Install docker-compose 
                
        wget https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64

        ls -lrt

        sudo chmod +x docker-compose-linux-x86_64

        sudo mv docker-compose-linux-x86_64 docker-compose

        sudo mv docker-compose /usr/local/bin/docker-compose

        docker compose --version

6. Bulid docker-compose.yml file
 
        #run docker compose 

        docker-compose up -d

        docker-compose ps

        In chrome use this url:

        <Ec2Ip Address>:8080/swagger-ui.html

7. Go inside container to check the database

        docker container exec -it <contIdOfPostgres> bash

        psql -h localhost -p 5432 -U scott -W

        password: Password

        \c postgres (to connect to postgres database)

        \dt (now we got all relations like account, statement etc)
  
# Deploying project in kubernets
  
  1. create one EC2 instance, t2.medium, 30GB, allport, Launch

  2. Login to mobaxterm

  3. Install docker
  
         sudo yum update -y
         sudo yum install docker -y
         sudo systemctl start docker
  
         #Give ec2-user permission for docker
                sudo usermod -a -G docker ec2-user
  
         sudo systemctl enable docker
         sudo systemctl status docker

  4. Install kubectl
  
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  
    sudo chmod +x kubectl
  
    sudo mv kubectl /usr/bin
  
    sudo su -
  
    kubectl



  5. Download Minikube

    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
  
    yum install conntrack -y
  
    minikube start  --vm-driver=none
  
  6. Install git and clone repository
     
    yum install git -y

    git clone <copy the link from github under code>

    Go inside project directory

  7. Deploy all configurations
    
    cd config

    kubectl apply -f db_deployment.yml

    kubectl apply -f service_db.yml

    kubectl get svc

    copy the cluster Ip of postgresservice

    vi app_deploy.yml

    paste in databasesource url before :5432.
  
    kubectl apply -f app_deployment.yml
  
    kubectl apply -f service_app.yml

    kubectl get all

    copy the port of myapp i.e., 30005 (8080:30005/TCP)
    
    # In the chrome use this url
        <IpofEc2Instance>:<myAppPort>/swagger-ui.html
 
 8. To check the database
      
        kubectl get all

        Copy the name of the pod -> Eg: creditcarddb-6db985574f-x6tfj 

        kubectl exec -it <Paste the name of Pod>

        psql -U <user_name> -d <databaseName>

        psql -U scott -d postgres

        \dt;

