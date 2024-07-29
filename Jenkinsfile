pipeline {
    agent any

    stages {
        stage('Git') {
            steps {
                git 'https://github.com/adikesavanaidug2404/TaskPro.git'
            }
        }
        stage('Maven-Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('S3-Backup') {
            steps {
                sh 'aws s3 cp /var/lib/jenkins/workspace/taskpro/ s3://taskprobucket/taskpro/ --recursive'
            }
        }
        stage('Docker-Build') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'Docker') {
                        sh "docker build -t adikesavanaidug2404/taskpro:v1 ."
                    }
                }
            }
        }
        stage('Docker-Deploy') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'Docker') {
                        sh "docker run -d -p 8083:8080 adikesavanaidug2404/taskpro:v1"
                    }
                }
            }
        }
        stage('Docker-Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'Docker') {
                        sh "docker push adikesavanaidug2404/taskpro:v1"
                    }
                }
            }
        }
    }
}
