pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'target/*.war', fingerprint: true
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-creds',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASS'
                    )
                ]) {
                    bat """
                    curl -u %TOMCAT_USER%:%TOMCAT_PASS% ^
                    -T target/jenkins-demo.war ^
                    "http://localhost:8081/manager/text/deploy?path=/jenkins-demo&update=true"
                    """
                }
            }
        }
    }
}
