pipeline {
    agent any  // This specifies that the pipeline will run on any available agent

    environment {
        // Define environment variables
        SCANNER_HOME = tool 'sonar-scanner'
    }
/*
    tools {
        // Define tools like JDK or Maven
        jdk 'JDK17'
        maven 'maven3'
    }
    */

    stages {
        /*stage('Checkout') {
            steps {
                // Checkout the code from version control
                git 'https://github.com/example/repository.git'
            }
        }*/

        stage('Compile') {
            steps {
                // Compile or build the project
                sh 'mvn compile'  // Example shell command
            }
        }

        stage('Test') {
            steps {
                // Run unit tests or other tests
                sh 'mvn test'
            }
        }

       
    }

    post {
        success {
            // Actions to perform when the pipeline succeeds
            echo 'Pipeline was successful!'
        }
        failure {
            // Actions to perform when the pipeline fails
            echo 'Pipeline failed!'
        }
        always {
            // Actions to perform no matter what (success or failure)
            cleanWs()  // Clean up workspace after the build
        }
    }
}
