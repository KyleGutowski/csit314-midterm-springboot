pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'java -version'
                sh 'sh ./mvnw -B clean package'
            }
        }

        stage('Verify Fat JAR') {
            steps {
                sh '''
                    jar tf target/spring-boot-complete-0.0.1-SNAPSHOT.jar > target/jar-contents.txt
                    grep '^BOOT-INF/classes/' target/jar-contents.txt | head -5
                    grep '^BOOT-INF/lib/' target/jar-contents.txt | head -5
                    grep -q '^BOOT-INF/classes/' target/jar-contents.txt
                    grep -q '^BOOT-INF/lib/' target/jar-contents.txt
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/spring-boot-complete-0.0.1-SNAPSHOT.jar', fingerprint: true
            }
        }
    }
}
