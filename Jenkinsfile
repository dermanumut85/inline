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
               script{
                sh 'cd /var/jenkins_home/workspace/${JOB_NAME}/terraform-data'
                sh 'pwd'
                sh 'terraform init'
                sh 'terraform apply -auto-approve'
                
                sh "terraform output server-public-ip > ./ip.txt"  

                sh "cat ./ip.txt"
               }
                
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
                
                    sh 'export IP=$(cat /var/jenkins_home/workspace/$JOB_NAME/ip.txt )'
                    sh 'ssh -i $KEY ec2-user@$IP -y '

                    sh   'docker run --name my-nginx -dp 90:80 umutderman/my-web-ste:latest'
                
            }
        }

        
        
    }
    
}
