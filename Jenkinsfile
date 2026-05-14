pipeline {
    agent any

    tools {
        maven 'Maven8'      // Use the name configured in your Jenkins
        jdk 'JDK21'        // Make sure this matches your JDK installation name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Likhitha-HM/lab6.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Run Application') {
            steps {
                // Run in background so pipeline doesn't hang
                sh 'mvn exec:java -Dexec.mainClass="com.example.app.App" > app.log 2>&1 &'
                sh 'sleep 5'  // Give it time to start
                sh 'echo "Application started in background. Check app.log for output."'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "SUCCESS: ${JOB_NAME} #${BUILD_NUMBER}",
                body: "Build succeeded!\nCheck: ${BUILD_URL}",
                to: "likhithahm953@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "FAILED: ${JOB_NAME} #${BUILD_NUMBER}",
                body: "Build failed!\nCheck: ${BUILD_URL}",
                to: "likhithahm953@gmail.com"
            )
        }
    }
}
