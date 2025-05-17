pipeline {
    agent any
    
    environment {
        BUILD_FILE_NAME = 'laptop.txt'
    }
    
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                echo 'building'
                sh '''
                npm --version
                mkdir -p ${WORKSPACE}/build
                echo $BUILD_FILE_NAME
                touch ${WORKSPACE}/build/computer.txt
                echo "Mainboard" >> ${WORKSPACE}/build/computer.txt
                cat ${WORKSPACE}/build/computer.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'test'
                sh '''
                test -f build/computer.txt && echo "File exists." || echo "File does not exist."
                grep Mainboard build/computer.txt
                '''
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
        }
        always {
            echo "post"
        }
    }
}