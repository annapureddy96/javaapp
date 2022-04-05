pipeline {
  agent any
  tools {
    maven 'maven'
  }
  stages {
   stage ('Initialize') {
            steps {
                sh '''
                    M2_HOME=/opt/maven
                    M2=/opt/maven/bin
                    PATH=$PATH:$HOME/bin/:$JAVA_HOME:$M2:$M2_HOME
                    export PATH
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    whoami
                '''
            }
        }
    stage('Build maven') {
      steps {
        sh 'mvn clean install package'
      }
    }
   
  stage('Push artifact to s3') {
   steps {
     sh 'aws s3 cp webapp/target/webapp.war s3://mahi-app'
      }
    }
//   stage('Deploy to tomcat') {
//     steps {
//        sh 'ansible-playbook deploy_new.yml'
//      }
//    }  
 
post {
     always {
       emailext to: 'annapureddymahendra@gmail.com',
       attachLog: true, body: "Dear team pipeline is ${currentBuild.result} please check ${BUILD_URL} or PFA build log", compressLog: false,
       subject: "Jenkins Build Notification: ${JOB_NAME}-Build# ${BUILD_NUMBER} ${currentBuild.result}"
    }
}
}
