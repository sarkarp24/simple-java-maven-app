pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                //sh './jenkins/scripts/deliver.sh'
                //deploy adapters: [tomcat7(credentialsId: 'tomcat_user', path: '', url: 'http://127.0.0.1:8888/')], contextPath: null, war: '**/*.war'
                deploy war: 'target/*.war', 
                        contextPath: '', 
                        container: 'tomcat', 
                        url: 'http://localhost:8888', 
                        credentialsId: 'tomcat_user' 
            }
        }
    }
}
