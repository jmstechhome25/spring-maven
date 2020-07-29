pipeline {
    agent any
    tools {
        maven "Maven"
    }
    stages {
        stage("Maven Install") {
            steps {
                script {
                    
                        withSonarQubeEnv('SonarQube') {
                            sh "mvn clean install sonar:sonar -s $MAVEN_GLOBAL_SETTINGS"
                        }
                    
                }
            }
        }
        stage("Quality Gate"){
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: false
              }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                        sh "mvn deploy -DskipTests -s $MAVEN_GLOBAL_SETTINGS"
                    }
              
            }
        }
    }
}
