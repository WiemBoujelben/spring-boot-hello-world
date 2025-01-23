pipeline {
  agent any
  stages {
    stage('Clone Repository') {
      steps {
        git(branch: 'main', url: 'https://github.com/WiemBoujelben/spring-boot-hello-world.git')
      }
    }

    stage('Build') {
      steps {
        sh 'mvn clean install'
        archiveArtifacts 'target/spring-boot-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }

    stage('Deploy') {
      steps {
        sh 'nohup mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8081 &'
        sleep(time: 30, unit: 'SECONDS')
        sh 'curl --fail --request GET \'http://192.168.192.131:8081/hello\''
      }
    }

  }
  tools {
    maven 'M398'
  }
}