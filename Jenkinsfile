node
{
   echo "git branch name: ${env.JOB_NAME}"
   echo "build number is: ${env.BUILD_NUMBER}"
   echo "node name is: ${env.NODE_NAME}"

  def mavenHomeee =tool name: "maven3.9.11"

  stage('git checkout')
   {
    git branch: 'development', credentialsId: '0f77403d-c90c-492e-8a54-ae72101bacb5', url: 'https://github.com/validev123/mvn-web-app.git'
   }
  stage('maven build')
   {
    sh "${mavenHomeee}/bin/mvn clean package"
   }
  stage('sonarqube report')
   {
    sh "${mavenHomeee}/bin/mvn sonar:sonar"
   }
  stage('deploying to nexus')
   {
   sh "${mavenHomeee}/bin/mvn deploy"
   }
  stage('Deploy to Tomcat') {
    echo "Deploying WAR file using curl..."

    sh """
        curl -u sai:password \
        --upload-file /var/lib/jenkins/workspace/jio-dev-scriptedway-PL/target/maven-web-application.war \
        "http://18.60.107.228:8080/manager/text/deploy?path=/maven-web-application&update=true"
    """
  }

} catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
} // node ending

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
    def colorName = 'RED'
    def colorCode = '#FF0000'
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
    if (buildStatus == 'STARTED') {
      color = 'YELLOW'
      colorCode = '#FFFF00'
    } else if (buildStatus == 'SUCCESS') {
      color = 'GREEN'
      colorCode = '#00FF00'
    } else {
      color = 'RED'
      colorCode = '#FF0000'
    }
  // Send notifications
    slackSend (color: colorCode, message: summary, channel: '#prod')
}
