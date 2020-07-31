pipeline {
    agent any
    tools {
        maven "Maven"
    }
    stages {
        stage("Maven Install") {
            steps {
                script {
                    configFileProvider([configFile(fileId: '043e98da-82d2-4415-8ce2-3fa4a49083bb', variable: 'MAVEN_GLOBAL_SETTINGS')]){
                        withSonarQubeEnv('SonarQube') {
                            sh "mvn clean install sonar:sonar -s $MAVEN_GLOBAL_SETTINGS"
                        }
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
                    configFileProvider([configFile(fileId: '28b2ee07-1d47-41e8-8cde-1cfe4bbef73a', variable: 'MAVEN_GLOBAL_SETTINGS')]){
                        sh "mvn deploy -DskipTests -s $MAVEN_GLOBAL_SETTINGS"
                    }
                }
            }
        }
    }
}
