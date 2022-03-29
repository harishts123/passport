pipeline{
   agent {
    docker {
      image 'maven:3.6.3-jdk-11'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
      stage("Maven Build"){
          steps{
             script{
                  flagemail=env.STAGE_NAME
            }
              sh 'mvn -B -DskipTests clean package'
          }
      }
      
      stage('Maven Test'){
            steps{
               script{
                  flagemail=env.STAGE_NAME
            }
                sh 'mvn test'
            }
            post{
            always{
                junit 'target/surefire-reports/*.xml'
            }
        }
        }
     stage("Build & SonarQube analysis") {
            agent any
            steps {
                script{
                  flagemail=env.STAGE_NAME
            }
              withSonarQubeEnv('sonar-pass') {
                sh 'java -version'
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
   

      stage('Deploy to artifactory'){
        steps{
        rtUpload(
         serverId : 'jfrog-server',
         spec :'''{
           "files" :[
           {
           "pattern":"target/*.jar",
           "target":"pass-2.1.1/Folder1/"
           }
           ]
         }''',
         
      )
      }
     }
    
  }
        post {  
         always {  
             echo 'This will always run'  
         }  
         success {   
            echo "========Deploying executed successfully========"
            emailext attachLog: true, body: "<b>Example</b><br>Project: ${env.JOB_NAME}", from: 'harish.dummymail@gmail.com', mimeType: 'text/html', replyTo: '', subject: "Deploy Successfully!! Project name -> ${env.JOB_NAME}", to: "harishts123@gmail.com; harish.dummymail@gmail.com";
         }  
         failure {  
             mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> Stage Name: $flagemail <br> URL de build: ${env.BUILD_URL}", cc: 'sanjayselvam5@gmail.com', charset: 'UTF-8', from: 'harish.dummymail@gmail.com', mimeType: 'text/html', replyTo: '', subject: "ERROR Deploy Failed: Project name -> ${env.JOB_NAME}", to: "harishts123@gmail.com";  
         }  
         unstable {  
             echo 'This will run only if the run was marked as unstable'  
         }  
         changed {  
             echo 'This will run only if the state of the Pipeline has changed'  
             echo 'For example, if the Pipeline was previously failing but is now successful'  
         }  
     }
     
  
        
     
    
  }

           
