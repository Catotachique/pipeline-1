pipeline {
    agent {
        docker {
            image 'maven:3.9.9-eclipse-temurin-21' // Maven + JDK 21
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

        stage('Test') {
            steps {
                echo 'test'
                sh '''
                test -f build/computer.txt && echo "File exists." || echo "File does not exist."
                grep Mainboard build/computer.txt
                '''
            }
        }

        stage('Packaging') {
            steps {
                echo 'packaging'
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