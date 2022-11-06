pipeline {
agent { node { label 'maven' } }
environment { APP_NAMESPACE = 'recover' } 
stages {
        stage('Test') {
            steps {
                    sh './mvnw clean test'
            }
        } 
    }
}