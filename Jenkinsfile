pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-21' // Maven + JDK 21
            args '-v /root/.m2:/root/.m2' // cache Maven dependencies
        }
    }
    
    environment {
        BUILD_FILE_NAME = 'laptop.txt'
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'building'
                sh '''
                java -version
                mvn -v
                mkdir -p ${WORKSPACE}/build
                echo $BUILD_FILE_NAME
                touch ${WORKSPACE}/build/computer.txt
                echo "Mainboard" >> ${WORKSPACE}/build/computer.txt
                cat ${WORKSPACE}/build/computer.txt
                '''
            }
        }

        stage('Run Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'unit tests'
                        sh '''
                        test -f build/computer.txt && echo "File exists." || echo "File does not exist."
                        grep Mainboard build/computer.txt
                        '''
                    }
                }
                stage('Integration Tests') {
                    steps {
                        echo 'integration tests'
                        sh '''
                        echo "Running integration tests."
                        '''
                    }
                }
                stage('Functional Tests') {
                    steps {
                        echo 'functional tests'
                        sh '''
                        echo "Running functional tests."
                        '''
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}:${IMAGE_TAG}")'
                script {
                    env.IMAGE_NAME = 'myapp'
                    env.DATE = sh(script: 'date +%Y%m%d%H%M%S', returnStdout: true).trim()
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'docker push ${DOCKER_REGISTRY}/${env.IMAGE_NAME}:${IMAGE_TAG}'
                echo 'DATE is ${env.DATE}'
            }
        }

        stage('Packaging') {
            steps {
                echo 'packaging'
            }
        }

        stage('Approval') {
            steps {
                echo 'approval' 
                timeout(time: 1, unit: 'HOURS') { // Timeout for 1 hour
                    input message: 'approve deployment?', ok: 'deploy'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploy'
            }
        }
    }
    
    post {
        success{
            archiveArtifacts artifacts: "build/**"
            echo "Deployment complete!"
        }
        failure {
            echo "Build or deployment failed!"
        }
        always {
            echo "post"
        }
    }
}