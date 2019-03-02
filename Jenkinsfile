pipeline {

	agent {
		label 'maven'
	}

	stages {
		stage('Construcción') {
			steps {
  				sh "mvn install -DskipTests=true"
    		}
		}
	}
}