pipeline {
  agent any

  tools {
    maven 'Maven 3.8.7'
    jdk 'JAVA 11'
  }

  environment {
    JAVA_HOME = "${tool 'Java 11'}"
    PATH = "${JAVA_HOME}/bin:${env.PATH}"
  }

  stages {
    stage('Checkout & Build WAR') {
      steps {
        git url: 'https://github.com/maazbinhasan/Java-app-Tomcat.git', branch: 'main'
        sh 'mvn clean package'
      }
    }

    stage('Deploy to Tomcat') {
      steps {
        sshagent(['tomcat-deploy-key']) {
          sh '''
            scp target/*.war ubuntu@localhost:/opt/tomcat/webapps/
            ssh ubuntu@localhost "/opt/tomcat/bin/shutdown.sh || true && sleep 5 && /opt/tomcat/bin/startup.sh"
          '''
        }
      }
    }
  }

  post {
    success {
      echo '✅ Build and Deployment Successful!'
    }
    failure {
      echo '❌ Build or Deployment Failed!'
    }
  }
}
