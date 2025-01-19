pipeline {
    agent any

    tools {
        // Specify the Maven version configured in Jenkins
        maven "M398"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/WiemBoujelben/spring-boot-hello-world.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                // Run the application in the background
                sh 'nohup mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8081 &'

                // Add sleep to allow the application to start
                sleep(time: 30, unit: 'SECONDS') // Adjust time if necessary

                // Test the deployed application
                sh "curl --fail --request GET 'http://192.168.192.131:8081/hello'"
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
