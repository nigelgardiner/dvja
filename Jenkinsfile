pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  STAGES {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
	stage('Check dependencies') {
		steps {
			dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
			dependencyCheckPublisher pattern: failedTotalCritical: 1, failedTotalHigh: 1, failedTotalLow: 10, failedTotalMedium: 5, pattern: '', unstableTotalCritical: 1, unstableTotalHigh: 1, unstableTotalLow: 10, unstableTotalMedium: 5
		}
	}
    stage('Publish to S3') {
      steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://ako2020-buildartifacts-9u83c8ja3te7/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}
