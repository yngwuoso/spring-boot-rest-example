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
	            openshift.newBuild("--name=rsexample", "--image-stream=redhat-openjdk18-openshift:1.1", "--binary")
	          }
	        }
	      }
	    }
	    
	    stage('Build Image') {
	      steps {
	        script {
	          openshift.withCluster() {
	            openshift.selector("bc", "rsexample").startBuild("--from-file=target/spring-boot-rest-example-0.0.1-SNAPSHOT.jar", "--wait")
	          }
	        }
	      }
	    }
	}
}