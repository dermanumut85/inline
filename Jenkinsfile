pipeline {
    agent any

    environment {
        version = '4'
        PASSWORD= credentials ("github_password")
    }

    stages{
        stage ("build Image") {
            steps{
                echo "Creatnig Image"
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
