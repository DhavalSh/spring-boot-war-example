pipeline {
    agent any
    tools {
        maven 'Maven' // Ensure that Maven is correctly configured in Jenkins global tool settings
    }
    stages {
        stage("Test") {
            steps {
                sh "mvn test" // Run the unit tests
                // slackSend channel: 'youtubejenkins', message: 'Job Started'
            }
        }
        stage("Build") {
            steps {
                sh "mvn package" // Package the application as a WAR file
            }
        }
        stage("Deploy on Test") {
            steps {
                // Deploy on Tomcat container - Test Environment
                deploy adapters: [tomcat9(credentialsId: 'admin123', path: '', url: 'http://13.127.241.151:8081')], 
                       contextPath: '/app', 
                       war: 'target/*.war' // Ensure this matches your actual WAR location
            }
        }
        stage("Deploy on Prod") {
            steps {
                // Deploy on Tomcat container - Production Environment
                deploy adapters: [tomcat9(credentialsId: 'admin123', path: '', url: 'http://13.127.241.151:8081')], 
                       contextPath: '/app', 
                       war: 'target/*.war' // Same as above, should match the WAR file's location
            }
        }
    }
    post {
        always {
            echo "========Pipeline Finished========"
        }
        success {
            echo "========Pipeline executed successfully========"
            // slackSend channel: 'youtubejenkins', message: 'Success'
        }
        failure {
            echo "========Pipeline execution failed========"
            // slackSend channel: 'youtubejenkins', message: 'Job Failed'
        }
    }
}
