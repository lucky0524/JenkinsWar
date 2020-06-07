node
{
	def mavenHome=tool name: "maven3"
	def tomcatWeb = '/opt/apache-tomcat-9.0.35/webapps'
	stage('git checkout')
	{
		git credentialsId: 'git', url: 'https://github.com/lucky0524/JenkinsWar.git'
	}
	stage('Build Artifacts')
	{
		sh "${mavenHome}/bin/mvn clean package"
	}
	stage('Deploy To Tomcat')
	{
	    sshagent(['1a738931-d4cf-4f9d-98be-b0e7f8edc195']
	    {
	        sh 'scp -o StrictHostKeyChecking=no target/JenkinsWar*.war ec2-user@172.31.8.246:${tomcatWeb}/JenkinsWar.war'
	    }
	}
}
