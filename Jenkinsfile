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
                      timestamp {
                          println "Descargar codigo fuente"
                          scm checkout
                          bat """
                                npm install
                            """
                      }
        			
          		}
        	}
    	}
	}
	
	stage('Deploy') {
    	steps {
    		script {
          		node {
        			println "Mi segundo stage esta en ejecucion. KeyID: $MyKeyID"
          		}
        	}
    	}
	}    
}

}
