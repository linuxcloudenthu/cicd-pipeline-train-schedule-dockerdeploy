pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            step {
                script {
                    app = docker.build("linuxcloudenthu/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Puch Docker Image to Registry') {
            when {
                branch 'master'
            }
            steps {
                 docker.withRegistry('https://registry.hub.docker.com', 'DockerHub_ID') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                 } 
            }
        }
    }
}
