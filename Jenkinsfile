pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            //args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                //sh 'mvn clean'
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
                //deploy adapters: [tomcat7(credentialsId: 'tomcat_user', path: '', url: 'http://54.191.84.186:8080/')], contextPath: null, war: '**/*.war'
                //deploy adapters: [tomcat7(credentialsId: 'tomcat_user', path: '', url: 'http://54.191.84.186:8080/')], contextPath: '/home/ec2-user/apache-tomcat-10.1.34/webapps', war: '**/*.war'
                sshagent(['deploy_user']) {
                    sh 'scp -o StrictHostKeyChecking=no /target/my-app-1.0-SNAPSHOT.war ec2-user@54.191.84.186:/home/ec2-user/apache-tomcat-10.1.34/webapps'
                }
            }
        }
    }
}