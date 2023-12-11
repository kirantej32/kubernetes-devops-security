pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        } 
       stage('Unit Tests - JUnit and JaCoCo') {
       steps {
         sh "mvn test"
       }
        post { 
         always { 
           junit 'target/surefire-reports/*.xml'
           jacoco execPattern: 'target/jacoco.exec'
             }
        }     
     } 
     stage('Mutation Tests - PIT') {
       steps {
         sh "mvn org.pitest:pitest-maven:mutationCoverage"  } //gthor
     } 
  pipeline {
    agent any

    environment {
        CONTRAST_SERVICE_KEY = 'your_contrast_service_key'
        CONTRAST_ORG_UUID = 'your_contrast_org_uuid'
        CONTRAST_SERVER_URL = 'https://app.contrastsecurity.com/Contrast'
        CONTRAST_APPLICATION_NAME = 'numeric-app'
    }

    stages {
        stage('Checkout') {
            steps {
                // Assuming your source code is in a Git repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Your build steps go here
                // For example: sh 'mvn clean install'
            }
        }

        stage('Contrast Security Scan') {
            steps {
                script {
                    def contrastScanCommand = """
                        contrast-cli --username \${CONTRAST_USERNAME} \
                                     --api-key \${CONTRAST_API_KEY} \
                                     --service-key \${CONTRAST_SERVICE_KEY} \
                                     --url \${CONTRAST_SERVER_URL} \
                                     --org-id \${CONTRAST_ORG_UUID} \
                                     --application-name \${CONTRAST_APPLICATION_NAME} \
                                     --auto-exit
                    """
                    sh contrastScanCommand
                }
            }
        }

        stage('Deploy') {
            steps {
                // Your deployment steps go here
            }
        }
    }

    post {
        always {
            // Cleanup or post-processing steps go here
        }
    }
}
  
  
  }
}
