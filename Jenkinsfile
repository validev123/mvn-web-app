node
{
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

}
