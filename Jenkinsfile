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
		
		stage('Pregunta') {
			steps {
				input message: "¿Desea crear la imagen?", ok: "Aceptar"
			}
		}
		
		stage('Create Image Builder') {
	      when {
	        expression {
	          openshift.withCluster() {
	            return !openshift.selector("bc", "rsexample").exists();
	          }
	        }
	      }
	      
	      steps {
	        script {
	          openshift.withCluster() {
	            openshift.newBuild("--name=rsexample", "--strategy=docker")
	          }
	        }
	      }
	    }
	    
	    stage('Build Image') {
	      steps {
	        script {
	          openshift.withCluster() {
	            openshift.selector("bc", "rsexample").startBuild()
	          }
	        }
	      }
	    }
	}
}