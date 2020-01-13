pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  stages {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
	stage('Check dependencies') {
		steps {
			dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
			dependencyCheckPublisher pattern: failedTotalCritical: 60, failedTotalHigh: 60, failedTotalLow: 80, failedTotalMedium: 70, pattern: '', unstableTotalCritical: 60, unstableTotalHigh: 60, unstableTotalLow: 80, unstableTotalMedium: 70
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
