node {
   // Mark the code checkout 'stage'....
   stage 'checkout'

   // Get some code from a GitHub repository
   git url: 'https://ahhosni@devvbustash.vistaprint.net/scm/devops/blueocean'
   sh 'git clean -fdx; sleep 4;'

   // Get the maven tool.
   // ** NOTE: This 'mvn' maven tool must be configured
   // **       in the global configuration.
   def mvnHome = tool 'mvn'

   stage 'build'

   
   withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'M3') {
            // set the version of the build artifact to the Jenkins BUILD_NUMBER so you can, map artifacts to Jenkins builds
            sh "mvn versions:set -DnewVersion=${env.BUILD_NUMBER}"
            sh "mvn package"
        } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe reports and FindBugs reports

   stage 'test'
   parallel 'test': {
         withMaven(maven: 'M3') {
           sh "mvn test; sleep 2;"
         }
   }, 'verify': {
         withMaven(maven: 'M3') {
     sh "mvn verify; sleep 3"
         }
   }

   stage 'archive'
   archive 'target/*.jar'
}


node('devwltvipjnk001') {
   stage 'deploy Canary'
   sh 'echo "write your deploy code here"; sleep 5;'

   stage 'deploy Production'
   input 'Proceed?'
   sh 'echo "write your deploy code here"; sleep 6;'
   archive 'target/*.jar'
}
