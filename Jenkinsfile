pipeline {

	agent none

	environment {
	    MyKeyID="myCustomValue1"
	}
	
	stages {
	
	stage('Init') {
    	steps {
    		script {
          		node {
                      timestamps  {
                          println "Descargar codigo fuente"
			  dir("myFolder") {
				docker.withRegistry("https://registry.hub.docker.com") {
					docker.image("98640321id/primer-docker:mi-etiqueta5test"){
				  checkout scm
				  bat """
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
        			 bat """
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
        			 bat """
				 	dir
					
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
        			 bat """
					npm start
				    """	
				}
          		}
        	}
    	}
	}    
}

}
