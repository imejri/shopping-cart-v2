pipeline {
agent { node { label 'maven' } }
environment { APP_NAMESPACE = 'recover' } 
    stages {
        stage('Test') {
            options { timeout(time: 50, unit: 'SECONDS') }
            steps {
                    sh './mvnw clean test'
            }
        }
        stage('Build Image') {
            environment { QUAY = credentials('QUAY_USER') }
            steps { 
                sh  './scripts/include-container-extensions.sh'
                sh """
                    ./scripts/build-and-push-image.sh \
                    -u $QUAY_USR \
                    -p $QUAY_PSW \
                    -b $BUILD_NUMBER
                """
            } // steps
        } // stage
} // pipeline