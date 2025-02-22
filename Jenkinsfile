pipeline {
    agent any 
    
    tools {
        jdk 'java'
        maven 'maven'
    }
    
    environment {
        ARTIFACTORY_URL = 'https://your-jfrog-instance/artifactory' // Replace with your JFrog Artifactory URL
        ARTIFACTORY_REPO = 'maven-repo' // Replace with your target JFrog repository
        CREDENTIALS_ID = 'jfrog-cred' // Jenkins credential ID for JFrog authentication
    }
    
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'master', changelog: false, poll: false, url: 'https://github.com/lucky143225/maven-project.git'
            }
        }
        
        stage("Compile") {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage("Package") {
            steps {
                sh "mvn clean package"
            }
        }
        
        stage("Publish to JFrog") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: CREDENTIALS_ID, usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASSWORD')]) {
                        sh "mvn deploy -DaltDeploymentRepository=${ARTIFACTORY_REPO}::default::${ARTIFACTORY_URL}/${ARTIFACTORY_REPO} -Dusername=${JFROG_USER} -Dpassword=${JFROG_PASSWORD}"
                    }
                }
            }
        }
    }
}
