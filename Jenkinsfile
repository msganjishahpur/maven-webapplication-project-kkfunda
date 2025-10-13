node
{
    echo "git branch name: ${env.BRANCH_NAME}"
    echo "build number is: ${env.BUILD_NUMBER}"
    echo "node name is: ${env.NODE_NAME}"
    
    def mavenHome =tool name: "maven-3.9.6"
    try{
        stage('Git checkout Stage'){
            git branch: 'dev', url: 'https://github.com/msganjishahpur/maven-webapplication-project-kkfunda.git'
        }   //ending git stage
    
        stage('Maven Build Stage'){
           sh "${mavenHome}/bin/mvn clean package"
        }   //ending maven stage
    
        stage('Code Quality Check Stage'){
          sh "${mavenHome}/bin/mvn sonar:sonar"
        }   //ending sonar stage
    
        stage('Deploy to NexusRepo Stage'){
         sh "${mavenHome}/bin/mvn deploy"
        }   //ending Nexus stage
    
        stage('Deploy to TomcatServer Stage'){
         echo "Deploying War file to TomcatServer by using curl url.."
         sh '''
         curl -u diya:password \
         --upload-file /var/lib/jenkins/workspace/jio-dev-scptd-project/target/maven-web-application.war \
            "http://13.201.101.42:8282/manager/text/deploy?path=/maven-web-application&update=true"
            '''
        }   //ending Tomcat stage
    
    }   // try ending
    catch (e){
        currentBuild.result ="FAILED"
        }finally{
            notifyBuild(currentBuild.result)
        }
    }

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'SkyBlue'
  def colorCode = '#1DDED6'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'GREEN'
    colorCode = '#1DDE27'
  } else if (buildStatus == 'SUCCESS') {
    color = 'BLUE'
    colorCode = '#1D40DE'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '##airtel-devops-team-project')
 // slackSend (color: colorCode, message: summary, channel: '##airtel-devops-team-project')
}
