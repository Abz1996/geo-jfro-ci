pipeline {
    triggers {
  pollSCM('* * * * *')
    }
   agent any
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
        stage('PUSH JFROG') {   
            steps {
                script{
                    // install pipeline-utility-steps plugin for the ReadMavenPom method
                    def mavenPom = readMavenPom file: 'pom.xml'
                    POM_VERSION = "${mavenPom.version}"
                    APP_NAME = "${mavenPom.name}"
                    echo "${POM_VERSION}"
                    echo "${APP_NAME}"
                    sh "curl -u${JFROG_USER}:${JFROG_PASSWORD} -T target/${APP_NAME}-${POM_VERSION}.jar http://3.80.82.36:8081/artifactory/geolocation/${APP_NAME}-${POM_VERSION}.jar"
                } 
            }
        }    	    
    }
}
