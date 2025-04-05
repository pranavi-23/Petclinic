pipeline {
    agent any

    stages {
        stage('Clone the project') {
            steps {
                git branch: 'feature/2025.03.20', url: 'https://github.com/pranavi-23/spring-petclinic.git'
            }
        }

        stage('Build the project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Publish the Test results') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Publish the Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '''
                    mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=Petclinic \
                    -Dsonar.projectName=Petclinic \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=sqp_e3324627a0d9594d74dfd0b550cf9c9be956c963
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    deploy adapters: [
                        tomcat9(credentialsId: 'tomcatt', path: '', url: 'http://localhost:8080/')
                    ], contextPath: 'spring-petclinic SCM', war: 'target/*.war'
                }
            }
        }
    }
}





