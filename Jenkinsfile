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
	            openshift.newBuild("--name=rsexample", "registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift", "--binary=true")
	          }
	        }
	      }
	    }
	    
	    stage('Build Image') {
	      steps {
	        script {
	          openshift.withCluster() {
	            openshift.selector("bc", "rsexample").startBuild("--from-file=target/spring-boot-rest-example-0.0.1-SNAPSHOT.jar", "--follow")
	          }
	        }
	      }
	    }
	}
}