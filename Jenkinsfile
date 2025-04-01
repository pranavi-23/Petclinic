
pipeline {
     agent any
    stages {
        stage('Clone the project') {
            steps {
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Petclinic.git'
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
        stage('Publish the artifacts results') { 
            steps {
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
        stage('deploy to tomcat')
        {
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcatt', path: '', url: 'http://localhost:8080/')], contextPath: 'this is petclinic', war: 'target/*.war'
            }
        }
    }
}