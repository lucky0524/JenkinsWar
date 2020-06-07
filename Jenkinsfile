node{

   def tomcatWeb = '/opt/apache-tomcat-9.0.35/webapps'
   stage('SCM Checkout'){
      git 'https://github.com/lucky0524/JenkinsWar.git'
   }
   stage('Compile-Package-create-war-file'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
   }
   stage('Deploy to Tomcat'){
      sshagent(['1a738931-d4cf-4f9d-98be-b0e7f8edc195']{ 
      sh "scp -o StrictHostKeyChecking=no target/JenkinsWar*.war ec2-user@172.31.8.246:${tomcatWeb}"
      }
   }
}
