#!groovy

def MIN_VERSION;

pipeline {

	//agent none
	
	agent {
	    node {
		label 'master'
	    }
	}

	environment {
	    MyKeyID="myCustomValue1"
		folderTrabajo="MiCarpeta-${BUILD_ID}"
	}
	
	stages {
	
    	stage('Init') {
    	//agent {
                    //docker { image 'node:7-alpine' }
                //}
        	steps {
        		script {
              		node {
    				docker.withRegistry('https://registry.hub.docker.com/',"DockerHubCredential2") {
    					docker.image('98640321id/nodejs:pipeline').inside("-u root:root") {
							timestamps  {
    						  println "Descargar codigo fuente"
							  	dir("$folderTrabajo") {
    								  
	    							def secret;
	    							withCredentials([string(credentialsId: "AccessTokenPrueba", variable: "AccessToken")]) {
	    								withEnv( ["JAVA_HOME=JavaPath"]  ) {
	    									println "AccessToken: $AccessToken";
	    									secret = "$AccessToken"
	    									sh "env"
	    								}
	    							}

	    							echo "AccessToken2: $secret";
	    							sh "env"
	    						
	    							  checkout scm
								  MIN_VERSION=sh(returnStdout: true, script: "git rev-parse --short HEAD").trim()
	    							  sh """
	    							  	npm --version
	    								npm install
	    								"""
    							     }
								}
    						
    							
    						
    				  		}
						    stash name: "${folderTrabajo}", include: "${folderTrabajo}/**"
    					}
            			
              		}
            	}
        	}
    	} // Cerrar bloque Init
    		
    		
    	stage('Analisis de codigo con Sonar') {
    		steps {
    			script {
    				node {
    					docker.withRegistry('https://registry.hub.docker.com/',"DockerHubCredential2") {
    						docker.image('98640321id/sonar_cli:scanner').inside("-u root:root") {
						//docker.image('98640321id/nodejs:pipeline').inside("-u root:root") {
							//ws {
    						      timestamps  {
							      //unstash "${folderTrabajo}"
							      
							      dir("${folderTrabajo}") {
								      checkout scm
    								 sh """
    								 	echo "Analisis de codigo con Sonar"
									/tmp/sonar-scanner-3.0.2.768/bin/sonar-scanner -D"sonar.version=${MIN_VERSION}"
    								    """	
    								}
    						      		
							}
    						}
    					}

    				}
    			}
    		}
    	} // "Cerrar Analisis de codigo con Sonar"
    		
    	stage('Contenedor Docker') {
    		steps {
    			script {
    				node {
    					//docker.withRegistry('https://registry.hub.docker.com/',"DockerHubCredential2") {
    						//docker.image('98640321id/nodejs:pipeline').inside("-u root:root") {
    						      timestamps  {
    							  unstash "${folderTrabajo}"
    								dir("${folderTrabajo}") {
    								 sh """
    									 docker login
    									 docker build -t ecommerce:first .
    									 docker tag  ecommerce:first 98640321id/ecommerce:first
    									 docker push 98640321id/ecommerce:first
    								    """	
    								}
    						      }
    						//}
    					//}

    				}
    			}
    		}
    	} // Cerrar bloque Contenedor Docker
    	
    	// stage('Deploy') {
        // 	steps {
        // 		script {
        //       		node {
    	// 			docker.withRegistry('https://registry.hub.docker.com/',"DockerHubCredential2") {
    	// 				docker.image('98640321id/nodejs:pipeline').inside("-u root:root") {
    	// 					unstash "${stashName}"
    	// 					dir("myFolder") {
    	// 					 sh """
    	// 						npm start
    	// 					    """	
    	// 					}
    	// 				}
    	// 			}
        //       		}
        //     	}
        // 	}
    	// }// cerrar bloque deploy

} // Cerrar bloque stages
	
post { 
	 	always { 

	 		script {

	 			node {
	 				echo "Ejecutando function always post."
					cleanWs()
	 				}
	 			}
        }

        success {
            echo 'El pipeline se ejecuto existosamente'
        }
        unstable {
            echo 'El estado de la ejecucion del pipeline es inestable'
        }
        failure {
            echo 'La ejecucion del pipeline ha fallado :('
        }
        changed {
            echo 'Le ejecucion del pipeline ha cambiado'
        }
		
		
}

}
