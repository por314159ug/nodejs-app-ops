pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'nodejs-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/por314159ug/nodejs-app.git'
                    ]]
                ])
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }
        
        stage('Cleanup Existing Container') {
            steps {
                script {
                    // Stop and remove any existing container using port 3000
                    powershell '''
                        $containers = docker ps -q --filter "publish=3000"
                        if ($containers) {
                            docker stop $containers
                            docker rm $containers
                        }
                    '''
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE).run('-p 3000:3000')
                }
            }
        }
    }
} 