pipeline{
    agent any
    
    stages{
        stage('Hello'){
            steps{
                echo 'Hello from Jenkins!'
            }
        }
        stage('Python Tests'){
            agent {
                docker {
                    image 'python:3.12-slim'
                    reuseNode true
                }
            }
            
            steps{
                sh 'python -m pip install -r requirements-dev.txt'
                sh 'python -m pytest -v'
            }
        }
    }
}