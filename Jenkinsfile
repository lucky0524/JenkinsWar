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
    state('Store Docker Image')
    {
        withCredentials([string(credentialsId: 'lucky0524', variable: 'docker_hub_password')])
        {
            sh "docker login -u lucky0524 -p ${docker_hub_password}'
        }
        sh 'docker push lucky0524/jenkinswar:${buildnumber}'
    }
    
}
