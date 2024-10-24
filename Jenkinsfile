pipeline {
    triggers {
  pollSCM('* * * * *')
    }
   agent any
      environment {
        JFROG_URL = 'http://3.80.82.36:8081/artifactory' // Set Artifactory URL
        JFROG_REPO = 'geolocation' // The target repository name in Artifactory
    }
    tools {
  maven 'M2_HOME'
  jfrog 'Jfrog remote cli'
}
    stages {
        stage('maven package') {
            steps {
                sh 'mvn clean'
                sh 'mvn package -DskipTests'
                sh 'ls target'
            }
        }
        stages {
        stage('PUSH JFROG') {   
            steps {
                script {
                    // Install pipeline-utility-steps plugin for the readMavenPom method
                    def mavenPom = readMavenPom file: 'pom.xml'
                    POM_VERSION = "${mavenPom.version}"
                    APP_NAME = "${mavenPom.name}"
                    echo "POM Version: ${POM_VERSION}"
                    echo "App Name: ${APP_NAME}"
                    
                    // Upload the JAR file to Artifactory
                    sh """
                    curl -u${JFROG_USER}:${JFROG_PASSWORD} \
                        -T target/${APP_NAME}-${POM_VERSION}.jar \
                        ${JFROG_URL}/${JFROG_REPO}/${APP_NAME}-${POM_VERSION}.jar
                    """
                }
            }
        }
    }
}
