pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
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
        stage('Sonar') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=simple-java-maven-app -Dsonar.host.url=http://10.3.202.213:9000 -Dsonar.login=34879617e359ea82f301296dd351012f560a3d0e'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
