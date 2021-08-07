pipeline {
    agent any

    environment {
        version = '4'
        PASSWORD= credentials ("github_password")
        ACCESS_KEY=credentials("aws_access_key")
        SECRET_KEY=credentials("aws_secret_key")
    }

     
    stages{
        
        stage ("Create Infrustructure") {
            steps{
                echo "Creating Infrastructure"
                sh 'export TF_VAR_access_key=$ACCESS_KEY'
                sh 'export TF_VAR_secret_key=$SECRET_KEY'
                sh 'cd ./terraform_data'
                sh 'terraform init'
                sh'terraform apply -auto-approve'
                
            }
        }

        stage ("build Image") {
            steps{
                echo "Creating Image"
                sh 'docker build . -t umutderman/my-web-ste:$version'
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
                sh 'docker login -u umutderman -p $PASSWORD'
                sh 'docker push umutderman/my-web-ste:$version'
                
            }
        }

        
        
    }
    
}
