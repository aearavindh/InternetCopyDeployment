pipeline {
    agent any
    tools {
        maven "Maven"   
    }    
    stages {
        stage('Compile-Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Pushing to Nexus'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'HelloWorld', classifier: 'Hello World', file: 'pom.xml', type: 'war']], credentialsId: 'nexus-credentialss', groupId: 'internet.copy', nexusUrl: '18.224.155.110:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'devopstraining', version: 'HW-1.2'
            }
        }
        stage('Deployment to AWS'){
            steps{
            deploy adapters: [tomcat9(credentialsId: 'tomcatCredentials', path: '', url: 'http://3.15.0.139:8088/')], contextPath: 'InternetCopyDemoPipeline', onFailure: false, war: '**/target/*.war'
            }
        }
      }
}
