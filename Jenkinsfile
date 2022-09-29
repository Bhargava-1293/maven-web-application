node{
    
    def mavenHome = tool name: "maven 3.8.1"
    
    stage('CheckOutcode')
    {
        git branch: 'development', credentialsId: '5aac4cab-4517-4f70-b376-591c61e24541', url: 'https://github.com/Bhargava-1293/maven-web-application.git'
    }

    stage('Build')
    {
    sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('SonarQube Report')
    {
    sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
    
    stage('UploadArtifactintoNexus')
    {
    sh "${mavenHome}/bin/mvn clean deploy"
    }
    
    stage('DeployAppIntoTomcatServer')
    {
        sshagent(['d68b946e-f616-4c8c-bbde-390e4287c134']) {
       sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@43.204.23.142:/opt/apache-tomcat-9.0.65/webapps/"
        }
    }
    
    stage('SendEmailNotification')
    emailext body: '''Build is over!

Regards,
Mithun Technologies..''', subject: 'Build over...', to: 'royalbhargava5@gmail.com'
}
