node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      copyArtifacts filter: 'index.jsp', fingerprintArtifacts: true, projectName: 'index', selector: lastSuccessful(), target: 'src/main/webapp'
      git 'https://github.com/linuxacademy/content-jenkinscert.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean install'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/*.xml'
      archiveArtifacts 'target/*.jar'
      fingerprint 'target/*.jar'
   }
}
