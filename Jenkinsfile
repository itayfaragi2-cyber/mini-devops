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
                sh '''
                    python -m venv /tmp/jenkins-venv
                    /tmp/jenkins-venv/bin/python -m pip install -r requirements-dev.txt
                    /tmp/jenkins-venv/bin/python -m pytest -v
                '''
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t mini-devops:ci .'
            }    
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        set +x
                        export DOCKER_CONFIG=$(mktemp -d)
                        trap 'rm -rf "$DOCKER_CONFIG"' EXIT

                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        
                        docker tag mini-devops:ci amnontt/mini-devops:$BUILD_NUMBER

                        docker push amnontt/mini-devops:$BUILD_NUMBER
                    '''
                }
            }
        }
    }
}