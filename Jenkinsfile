node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/walidsaad/training-app-gk.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'Maven3'
      jdkhome = tool 'jdk11'
   }
        
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome", "JAVA_HOME=$jdkhome"]) {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean build"
      } else {
         bat(/"${mvnHome}\bin\mvn" -B -DskipTests -Dmaven.test.failure.ignore clean package/)
      }
   }
   }
   
   stage('Test') {
       withEnv(["MVN_HOME=$mvnHome", "JAVA_HOME=$jdkhome"]) {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' test"
      } else {
         bat(/"${mvnHome}\bin\mvn" test/)
      }
   }
   }
   
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts  'target/*.jar'
   }
   
      stage('Publish Artefact') {
          withEnv(["MVN_HOME=$mvnHome", "JAVA_HOME=$jdkhome"]) {
        
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -DdeployOnly deploy"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.skip=true deploy -s "C:\apache-maven-3.6.3-bin\conf\settings.xml"/)
      }
   }
      }
   
   stage('Execute Jar') {
       withEnv(["MVN_HOME=$mvnHome", "JAVA_HOME=$jdkhome"]) {
      if (isUnix()) {
         sh "java -jar  target/*.jar"
      } else {
         bat(/java -jar target\/training-app-1.0-SNAPSHOT-jar-with-dependencies.jar/)
      }
   }
   }
}
