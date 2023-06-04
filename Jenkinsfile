pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://http://18.60.85.16:9000" -e SONAR_LOGIN="sqp_0ea25d8e10a0c2fdd27b5c077a6ef026cbd56fa5" --volume ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms-cicd'
            }
        }
        stage('Releasing') {
            steps {
                echo 'Releasing Nexus'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist'
                sh 'cd webapp && curl -v -u admin:gopal --upload-file dist-${BUILD_NUMBER}.zip http://18.60.85.16:8081//repository/lms/ '
            }
        }
    }
}
