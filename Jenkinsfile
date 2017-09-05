node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'

    // Checkout code from repository
    checkout scm

    stage 'Environment'
    sh 'printenv'

    // Mark the code build 'stage'....
    stage 'Build'

    def dockerImage = docker.build("trnubot/captainhook:${env.BUILD_ID}")

    dockerImage.inside {
        sh 'echo "OK"'
        sh 'ls -l /entry.sh'
    }

    stage 'Push'
    docker.withRegistry('https://registry.hub.docker.com', '22ec7b27-f486-4683-a51d-606b88dac043') {
        dockerImage.push()
    }
}
