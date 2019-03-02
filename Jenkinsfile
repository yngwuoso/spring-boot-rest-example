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
		
		stage('Creación de la imagen ejecutando el Dockerfile') {
			steps {
				openshift.withCluster() {
					openshift.withProject() {
						sh "cat Dockerfile | oc new-build --name node-container --dockerfile='-'"
					}
				} 
			}
		}
	}
}