pipeline {
    agent any

    tools {
        maven 'maven-3.9.6'
    }

    stages {
        stage('Git Checkout Stage') {
            steps {
                git branch: 'dev', url: 'https://github.com/msganjishahpur/maven-webapplication-project-kkfunda.git'
            }
        }

        stage('Maven Build Stage') {
            steps {
                sh "${tool 'maven-3.9.6'}/bin/mvn clean package"
            }
        }

        stage('SonarQube Report Stage') {
            steps {
                sh "${tool 'maven-3.9.6'}/bin/mvn sonar:sonar"
            }
        }

        stage('Deploy to Nexus Artifact Stage') {
            steps {
                sh "${tool 'maven-3.9.6'}/bin/mvn deploy"
            }
        }

        stage('Deploy into Tomcat Server Stage') {
            steps {
                sh '''
                curl -v -u diya:password \
                --upload-file target/maven-web-application.war \
                "http://65.2.127.151:8282/manager/text/deploy?path=/maven-web-application&update=true"
                '''
            }
        }
    }
}
