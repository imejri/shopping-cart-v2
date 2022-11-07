pipeline {
agent { node { label 'maven' } }
environment { APP_NAMESPACE = 'recover' }
parameters {
        string(name: 'DEPLOY_TAG', description: 'Deploy to Tag')
    }
    stages {
        stage('Test') {
            when { expression { params.DEPLOY_TAG == '' } }
            options { timeout(time: 50, unit: 'SECONDS') }
            steps {
                    sh './mvnw clean test'
            }
        }
        stage('Build Image') {
            when { expression { params.DEPLOY_TAG == '' } }
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

        stage('Deploy') {
            environment { QUAY = credentials('QUAY_USER') }
            steps {
                script {
                        def TAG_TO_DEPLOY = "BUILD-$BUILD_NUMBER"
                        if (params.DEPLOY_TAG != '' && params.DEPLOY_TAG != null) {
                            def status = sh(
                                script: "./scripts/tag-exists-in-quay.sh $QUAY_USR/do400-recover ${params.DEPLOY_TAG}",
                                returnStatus: true)
                            if (status != 0) {
                                error("Tag not found in Quay!")
                            } //if
                            TAG_TO_DEPLOY = params.DEPLOY_TAG
                        } // if
                    sh """
                        ./scripts/deploy-image.sh \
                        -u $QUAY_USR \
                        -n $APP_NAMESPACE \
                        -t $TAG_TO_DEPLOY
                    """
                } // script
            } // steps
        } // stage
    } // stages
} // pipeline