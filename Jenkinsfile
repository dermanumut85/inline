pipeline {
    agent any

    environment {
        
        PASSWORD= credentials ("github_password")
        AWS_ACCESS_KEY_ID=credentials("aws_access_key")
        AWS_SECRET_ACCESS_KEY=credentials("aws_secret_key")
    }

     
    stages{
        
        stage ("Create Infrustructure") {
            steps{
                echo "Creating Infrastructure"
               
                sh 'cd /var/jenkins_home/workspace/$JOB_NAME/'
                sh 'cd ./terraform-data'
                sh 'terraform init'
                sh 'terraform show'
                
                EC2_IP = sh (script:"terraform output server-public-ip"  returnStdout: true).trim()
                
                
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
                echo "Deploying to server"
                sshagent(credentials:['amazon-key.pem']){

                    sh 'ssh -l ec2-user@$EC2_IP '

                    sh   'docker run --name my-nginx -dp 90:80 umutderman/my-web-ste:latest'
                }
            }
        }

        
        
    }
    
}
