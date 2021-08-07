pipeline {
    agent any

    environment {
        
        PASSWORD= credentials ("github_password")
        AWS_ACCESS_KEY_ID=credentials("aws_access_key")
        AWS_SECRET_ACCESS_KEY=credentials("aws_secret_key")
        KEY=credentials("amazon-key.pem")
    }

     
    stages{
        
          stage ("Create Infrastructure") {
            steps{
                echo "Creating Infrastructure"

                sh """ 
                #!/bin/bash
                cd /var/jenkins_home/workspace/${JOB_NAME}/terraform-data
                pwd
                terraform init
                terraform apply -auto-approve
                terraform output server-public-ip > ./ip.txt  
                cat ./ip.txt
                
                """ 
              
               }
               
             }

        stage ("build Image") {
            steps{
                echo "Creating Image"
                sh 'docker build . -t umutderman/my-web-ste:latest'
            }
        }



        stage ("test") {
            steps{
                echo "test completed"
                
            }
        }
        stage ("Push to Docker Hub") {
            steps{
                echo "Login to Docker hub"
                sh 'docker login -u umutderman -p ${PASSWORD}'
                sh 'docker push umutderman/my-web-ste:latest'
                
            }
        }



        stage ("Deploy") {
            steps{
                echo 'Deploying to server'
                
                sh '''
                #!/bin/bash
                export IP=$(cat /var/jenkins_home/workspace/$JOB_NAME/ip.txt )
                ssh -i $KEY ec2-user@$IP -y 
                docker run --name my-nginx -dp 90:80 umutderman/my-web-ste:latest
                '''
                
            }
        }

        
        
    }
    
}
