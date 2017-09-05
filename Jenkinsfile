node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'

    // Checkout code from repository
    checkout scm

    stage 'Environment'
    sh 'printenv'

    // Mark the code build 'stage'....
    stage 'Build'

    def dockerImage = docker.build('trnubot/captainhook:${env.BUILD_ID}")

    dockerImage.inside {
        sh 'echo "OK"'
    }

    stage 'Push'
    dockerImage.push()
}
