pipeline {
    agent any

    environment {
        
        PASSWORD= credentials ("github_password")
        AWS_ACCESS_KEY_ID=credentials("aws_access_key")
        AWS_SECRET_ACCESS_KEY=credentials("aws_secret_key")
        
    }
    stages{

            stage ("build Image") {
                    steps{
                        echo "Creating Image"
                        sh 'docker build . -t umutderman/my-web-ste:latest'
                    }
                }

            stage ("Push to Docker Hub") {
                    steps{
                        echo "Login to Docker hub"
                        sh 'docker login -u umutderman -p ${PASSWORD}'
                        sh 'docker push umutderman/my-web-ste:latest'
                        
                    }
                }
     
   
            stage ("Create Infrastructure") {
                    steps{

                        echo "Creating Infrastructure"

                        sh """ 
                        #!/bin/bash
                        cd /var/jenkins_home/workspace/$JOB_NAME/terraform-data
                        pwd
                        terraform init
                        terraform apply -auto-approve
                        """ 
                    }
             }               
    }

}
        
        

