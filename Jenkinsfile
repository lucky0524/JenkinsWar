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
      sh "scp -o StrictHostKeyChecking=no target/JenkinsWar*.war ec2-user@172.31.8.246:${tomcatWeb}"
   }
}
