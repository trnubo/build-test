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
    stage('Build')
    def dockerImage = docker.build("trnubot/captainhook:${env.BUILD_TAG}")

    stage('Test') {
        try {
            withEnv(["GOSS_VER=v0.3.4","GOSS_PATH=${env.WORKSPACE}/goss","GOSS_OPTS=--format junit"]) {
                sh "wget -O goss  https://github.com/aelsabbahy/goss/releases/download/$GOSS_VER/goss-linux-amd64 && chmod +x goss"
                sh "./dgoss run trnubot/captainhook:${env.BUILD_TAG} | tee junit-out.xml"

                dockerImage.inside {
                    sh 'echo "OK"'
                    sh 'ls -l /entry.sh'
                }
            }
        }
        finally {
            junit 'junit-out.xml'
        }
    }

    stage('Push') {
        docker.withRegistry('https://registry.hub.docker.com', '22ec7b27-f486-4683-a51d-606b88dac043') {
            dockerImage.push()
        }
    }
}
