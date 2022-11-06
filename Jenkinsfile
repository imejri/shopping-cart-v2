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
    }
}