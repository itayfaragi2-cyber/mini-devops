pipeline{
    agent any
    stages{
        stage('Hello'){
            steps{
                echo 'Hello from Jenkins!'
            }
        }
        stage('Verify'){
            steps{
                sh 'pwd'
                sh 'ls -la'
            }
        }
    }
}