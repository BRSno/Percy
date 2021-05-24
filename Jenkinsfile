node {
   def mvnHome
   def artifactname = "percy.war"
   def repoName = "Percy"
   def pkgName = "percy-package"
   stage('Setup') { // for display purposes
      snDevOpsStep(enabled:true)
      // Get some code from a GitHub repository
      git branch: 'main', credentialsId: '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', url: 'https://github.com/BRSno/Percy'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.          
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      snDevOpsStep(enabled:true)
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      snDevOpsStep(enabled:true)
      //includeBuildInfo=true
     
      archiveArtifacts 'target/*.war'
      snDevOpsArtifact(artifactsPayload: """{"artifacts": [{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}","semanticVersion": "1.${env.BUILD_NUMBER}.0","repositoryName": "${repoName}"}],"branchName":"master"}""")
   }
   
   
   stage('UAT Test') {
       snDevOpsStep(enabled:true)
      // junit 'target/surefire-reports/*.xml'
       junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
       

   }
      stage('Deploy') {
      snDevOpsStep(enabled:true)
      snDevOpsPackage(name: "${pkgName}-${env.BUILD_NUMBER}", artifactsPayload: """{"artifacts":[{"name": "${artifactname}", "version": "1.${env.BUILD_NUMBER}", "repositoryName": "${repoName}"}], "branchName":"main"}""")
      snDevOpsChange()
      sh "mv  target/percy.war target/dev.war"
   }
   
}
   
