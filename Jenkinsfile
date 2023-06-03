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
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://18.61.42.237:9000" -e SONAR_LOGIN="sqp_e15bbf48d899c941399a16e68e2f21d69efaa90c" --volume ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms-sonar'
            }
        }
        stage('Releasing') {
            steps {
                echo 'Releasing Nexus'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist'
                sh 'cd webapp && curl -v -u admin:gopal --upload-file dist-${BUILD_NUMBER}.zip http://18.61.42.237:8081/repository/lms-artifact/'
            }
        }
    }
}
