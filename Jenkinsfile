pipeline {
    agent any



    stages {

        stage('Hello') {
            steps {
                echo 'Hello world depuis Jenkinsfile !'
            }
        }

        stage('Checkout') {
            steps {
                echo 'Code récupéré automatiquement'
                checkout scm
            }
        }

        stage('Compile') {
            steps {
                echo 'Compilation du projet'
                sh 'mvn clean compile -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                        sh '''
                            mvn sonar:sonar \
                            -Dsonar.login=$SONAR_TOKEN \
                            -Dmaven.test.skip=true
                        '''
                    }
                }
            }
        }


    }

    post {
        success {
            echo '🎉 Build réussi !'
        }
        failure {
            echo '❌ Build échoué !'
        }
    }
}
