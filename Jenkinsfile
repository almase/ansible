pipeline {
  agent any
/* Triggers a lanzar
  triggers {
       pollSCM '* * * * *'
       upstream 'ProbarAgente'
  }
  */
  
  stages {
    stage('compilar') {
      steps {
        echo "Estoy compilando el codigo de Sprint Boot"
        sh './mvnw compile'
             }
       }
    stage('test') {
      steps {
        echo "Estoy probando el codigo de Sprint Boot"
        sh './mvnw test'
             }
       }
    stage('GenerarJAR') {
      steps {
        echo "Estoy  generando el JAR del proyecto"
        sh './mvnw package'
             }
      post {
    	     failure {
        	   echo "He tenido un problema en el Build"
	           sh 'rm -rf target'
	          }  
          }
    }
    stage('Desplegar en los servidores:') {
      steps {
        echo "Estoy desplegando al directorio /tmp del servidor Jenkins "
        //sh 'cp target/calculadora-0.0.1-SNAPSHOT.jar /tmp'
        // Ahora como ansible
        //sh 'ansible all -i maquinas -a "cp target/calculadora-0.0.1-SNAPSHOT.jar /tmp/calculadora-0.0.1-SNAPSHOT.jar"'
          ansiblePlaybook credentialsId: 'bc2e258e-a384-4fa0-b80d-ed0fd4da441c', installation: 'Ansible', inventory: 'maquinas', playbook: 'p1.yml'
             }
       }
   }
   post {
        success {
		echo "He conseguido terminar"
		sh 'echo ${BRANCH_AUTHOR} Alberto > /tmp/autor.txt'
                }
        failure {
		echo "H tenido algun problema"
        }
}
}
