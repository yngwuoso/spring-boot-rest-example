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
				input message: "¿Desea realizar el paso a pre-producción?", ok: "Aceptar"
			}
		}
		
		stage('Despliegue') {
			steps {
				sh "echo terminado"
			}
		}
	}
}