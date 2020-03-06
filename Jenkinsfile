node
{
     def mavenHome = tool name: "maven 3.6.3"
     properties([pipelineTriggers([pollSCM(ignorePostCommitHooks: true, scmpoll_spec: '* * * * *')])])
     
     
     
  stage('CheckOutCode') 
  {
   git branch: 'development', credentialsId: 'c9f01e50-c559-45ad-bb7b-9e463c1c197c', url: 'https://github.com/divya27112/maven-web-application.git'
  }
   
  stage('Build')
   {
   sh "${mavenHome}/bin/mvn clean package"
   }
   
   stage('ExecuteSonarQubeReport')
    {
     sh "${mavenHome}/bin/mvn sonar:sonar"
    }
	
    stage('UploadArtifactIntoNexus')
     {
      sh "${mavenHome}/bin/mvn deploy"
     }
   
   stage('DeployAppIntoTomcat')
   {
     sshagent(['3012a020-836c-4244-91a0-2a89cb1bd300'])
   {
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.128.242:/opt/apache-tomcat-9.0.31/webapps/" 
   }
   }
  
    stage('SendEmailNotification')
   {
   
   emailext body: '''hi 
   this''', subject: 'masala', to: 'devopstrainfree@gmail.com'
   }
   
}
