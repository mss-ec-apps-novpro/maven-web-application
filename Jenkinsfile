node
{
    def mavenHome = tool name: "maven3.8.4"
    stage('CheckoutCode') {
        git branch: 'development', credentialsId: '593bbd00-39cf-4df7-ad44-193c176a9c9f', url: 'https://github.com/mss-ec-apps-novpro/maven-web-application.git'
        }
            stage('Builds')
            {
                sh "${mavenHome}/bin/mvn clean package"
            }
            stage('ExecuteSonarQubeReport')
            {
                sh "${mavenHome}/bin/mvn clean sonar:sonar"
            }
            stage('UpdateArtifactIntoNexus')
            {
                sh "${mavenHome}/bin/mvn clean deploy"
            }
            sshagent(['d941e763-9101-468b-94b7-f44878dc261d']) {
            sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@ip-172-31-12-61:/opt/apache-tomcat-9.0.62/webapps"
             post {
  always {
           emailtext body: '''build over
           
           Regards
           Shashikumar''', Subject: 'Build over...', to: 'shashikumarsashi@gmail.com'
  }
  success {
          emailtext body: '''build over
           
           Regards
           Shashikumar''', Subject: 'Build over...Sucess', to: 'shashikumarsashi@gmail.com' 
  }
  failure {
    emailtext body: '''build over
           
           Regards
           Shashikumar''', Subject: 'Build over...failure', to: 'shashikumarsashi@gmail.com'
  }
}
}
}
