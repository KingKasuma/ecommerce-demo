pipeline {

	agent none
	
	//agent {
        	//docker { image 'node:7-alpine' }
    	//}

	environment {
	    MyKeyID="myCustomValue1"
	}
	
	stages {
	
	stage('Init') {
	//agent {
                //docker { image 'node:7-alpine' }
            //}
    	steps {
    		script {
          		node {
                      timestamps  {
                          println "Descargar codigo fuente"
				  dir("myFolder") {
						docker.withRegistry('https://registry.hub.docker.com/',"DockerHubCredential2") {
							docker.image('latoso/container:node').inside("-u root:root") {
							  checkout scm
							  sh """
							  	npm --version
								npm install
								"""
							     }
							 }
				  }
				    stash name: "myFolder", include: "myFolder/**"
				}
        			
          		}
        	}
    	}
	}
		
		
	stage('Analisis de codigo con Sonar') {
    	steps {
    		script {
          		node {
                      timestamps  {
                          unstash "myFolder"
				dir("myFolder") {
        			 sh """
				 	dir
				    """	
				}
                      }
        			
          		}
        	}
    	}
	}
		
		stage('Contenedor Docker') {
    	steps {
    		script {
          		node {
                      timestamps  {
                          unstash "myFolder"
				dir("myFolder") {
        			 sh """
					 docker login
					 docker build -t primer-docker2:my-etiqueta .
					 docker tag primer-docker2:my-etiqueta 98640321id/primer-docker:my-etiqueta
					 docker push primer-docker:my-etiqueta
				    """	
				}
                      }
        			
          		}
        	}
    	}
	}
	
	stage('Deploy') {
    	steps {
    		script {
          		node {
				unstash "${stashName}"
				dir("myFolder") {
        			 sh """
					npm start
				    """	
				}
          		}
        	}
    	}
	}    
}

}
