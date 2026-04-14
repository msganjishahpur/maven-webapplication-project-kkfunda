
node{
    ///var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/maven-3.9/bin
    def mavenHome =tool name: "maven-3.9"
    stage('Git Checkout Stage') {
                git branch: 'dev', url: 'https://github.com/msganjishahpur/maven-webapplication-project-kkfunda.git'
        }
        
        stage('Maven Build Stage') {
                sh "${mavenHome}/bin/mvn clean package"
        } 
        
        stage('Generating SonarQube Report Stage') {
                sh "${mavenHome}/bin/mvn sonar:sonar"
        }
        
        stage('Deploy to Nexus Artifact Stage') {
                sh "${mavenHome}/bin/mvn deploy"
        }
        
        stage('Deploy to Tomcat-Server Stage') {
            echo " Deploying war into Tomcat-Server by using curl..."
            sh """
            curl -u admin:password --upload-file /var/lib/jenkins/workspace/jio-dev-scripted-pipeline/target/maven-web-application.war \
            "http://13.234.75.102:8080/manager/text/deploy?path=/maven-web-application&update=true"
            """
        }
}
