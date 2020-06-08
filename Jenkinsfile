node{
    def mavenHome=tool name: "maven3"
    def buildnumber = env.BUILD_NUMBER
    stage('git checkout')
    {
        git credentialsId: 'git', url: 'https://github.com/lucky0524/JenkinsWar.git'
    }
    stage('Build Artifacts')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('Code Quality Report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
 
    /*stage('Deploy To Tomcat')
    {
        sshagent(credentials:['sshauth']) 
        {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.8.246:/opt/apache-tomcat-9.0.35/webapps/'
        }
    }*/
    
    stage('Build Docker Image')
    {
        sh "docker build -t lucky0524/jenkinswar:${buildnumber} ."
    }
    stage('Push Docker Image')
    {
        withCredentials([string(credentialsId: 'lucky0524', variable: 'docker')])
        {
            sh "docker login -u lucky0524 -p ${docker}"
        }
        sh "docker push lucky0524/jenkinswar:${buildnumber}"
    }
    stage('Run Docker Image In Dev Server')
    {
        sshagent(['dockerserver'])
        {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.136 docker stop jenkinswaercontainer || true'
            sh 'ssh  ubuntu@172.31.12.136 docker rm jenkinswaercontainer || true'
            sh 'ssh  ubuntu@172.31.12.136 docker rmi -f  $(docker images -q) || true'
            sh "ssh  ubuntu@172.31.12.136 docker run  -d -p 8080:8080 --name jenkinswarcontainer lucky0524/jenkinswar:${buildnumber}"
        }
    }  
}
