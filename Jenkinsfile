pipeline {
    agent { 
        label 'master'
        }
    triggers { pollSCM('* * * * *') }
    options {
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
    }
	environment {
        ARTIFACT_NAME = 'process-management.jar'
    }
    
    stages {
        stage('Back-end') {
            agent {
                docker { image 'openjdk:8' }
            }
            steps {
                sh 'chmod +x gradlew && bash ./gradlew bootJar'
		sh 'mv build/libs/process-management-0.0.1-SNAPSHOT.jar env.ARTIFACT_NAME'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'env.ARTIFACT_NAME', onlyIfSuccessful: true
        }
    }
}
