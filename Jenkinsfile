//library "sharedlibrarysonar@main"
pipeline {
    agent any  // This specifies that the pipeline will run on any available agent

    environment {
        // Define environment variables
       SCANNER_HOME = tool 'sonar-scanner'
       TEST = "Test"
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
                //cleanWs()
            }
        }

         stage('File system scan') {

             agent {
                 label 'slave'
             }
             
            steps {
                script{
                   sh 'whoami'
                    // Create the directory if it doesn't exist
            //sh 'mkdir -p /var/lib/jenkins/pipe/tmp/trivy'
            
            // Set the appropriate permissions
            //sh 'chmod -R 777 /var/lib/jenkins/pipe/tmp/trivy'
             //sh 'ls -ld /var/lib/jenkins/pipe/tmp/trivy'
            // Run the trivy command and save the output
             sh """trivy fs . --output '${WORKSPACE}/trivy-report.html' """

                    archiveArtifacts artifacts: '**/trivy-report.html', followSymlinks: false
                    
                }
                
            }
        }

       
        stage('SonarQube Analysis') {

            steps {
                script{

                    //def var = new sharedlibrary()

                    

                    sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=BoardGame \
                    -Dsonar.host.url=http://192.168.244.132:9000 \
                    -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
    
/*
        stage('Quality Gate') {
            steps {
                script { 
                    waitForQualityGate abortPipeline: false, credentialsId: 'sqp_c98892cff4bf90179e9bd8a090d0e2f0520d6262'  
                }
            }
        }
*/

        stage('Build') {
            steps {
                sh "mvn package"
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }

        stage('Nexus Repo') {
            steps {
                nexusPublisher nexusInstanceId: 'nexus-repo-server', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '${WORKSPACE}/target/database_service_project-0.0.2.jar']], mavenCoordinate: [artifactId: 'database_service_project', groupId: 'BoardGameDeploy', packaging: 'jar', version: '0.0.2']]], tagName: '1.0'
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
        
    }
}

