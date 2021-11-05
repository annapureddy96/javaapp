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
   stage('push artifact to s3') {
      steps {
        sh 'aws s3 cp webapp/target/webapp.war s3://vaishu-s'
      }
    }
     stage('Deploy to tomcat') {
      steps {
        sh 'ansible-playbook deploy_new.yml'
      }
    }
 //     stage('building docker image from docker file by tagging') {
//       steps {
//         sh 'docker build -t phanirudra9/phani9-devops:$BUILD_NUMBER .'
//       }   
//     }
//     stage('logging into docker hub') {
//       steps {
//         sh 'docker login --username="phanirudra9" --password="9eb876d4@A"'
//       }   
//     }
//     stage('pushing docker image to the docker hub with build number') {
//       steps {
//         sh 'docker push phanirudra9/phani9-devops:$BUILD_NUMBER'
//       }   
//     }
//     stage('deploying the docker image into EC2 instance and run the container') {
//       steps {
//         sh 'ansible-playbook deploy.yml --extra-vars="buildNumber=$BUILD_NUMBER"'
//       }   
//     }  
}
post {
     always {
       emailext to: 'annapureddymahendra@gmail.com',
       attachLog: true, body: "Dear team pipeline is ${currentBuild.result} please check ${BUILD_URL} or PFA build log", compressLog: false,
       subject: "Jenkins Build Notification: ${JOB_NAME}-Build# ${BUILD_NUMBER} ${currentBuild.result}"
    }
}
}
