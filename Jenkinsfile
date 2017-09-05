node {
    // Mark the code checkout 'stage'....
    stage('Checkout') {

        // Checkout code from repository
        checkout scm
    }

    stage('Environment') {
        sh 'printenv'
    }

    // Mark the code build 'stage'....
    stage('Build') {

        def dockerImage = docker.build("trnubot/captainhook:${env.BUILD_ID}")
    }

    stage('Test') {
        environment {
            GOSS_VER = 'v0.3.4'
        }
        steps {
            sh "wget -O goss  https://github.com/aelsabbahy/goss/releases/download/$GOSS_VER/goss-linux-amd64 && chmod +x goss"
            sh "wget -O dgoss https://raw.githubusercontent.com/aelsabbahy/goss/$GOSS_VER/extras/dgoss/dgoss  && chmod +x dgoss"
            sh "./dgoss trnubot/captainhook:${env.BUILD_ID}"

            dockerImage.inside {
                sh 'echo "OK"'
                sh 'ls -l /entry.sh'
            }
        }
    }

    stage('Push') {
        docker.withRegistry('https://registry.hub.docker.com', '22ec7b27-f486-4683-a51d-606b88dac043') {
            dockerImage.push()
        }
    }
}
