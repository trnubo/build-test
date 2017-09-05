node {
   // Mark the code checkout 'stage'....
   stage 'Checkout'

   // Checkout code from repository
   checkout scm

   stage 'Environment'
   sh 'printenv'

   // Mark the code build 'stage'....
   stage 'Build'
   // Run the maven build
   sh "docker build -t trnubo/captainhook:latest ."
}
