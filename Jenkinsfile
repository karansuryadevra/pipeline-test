pipeline{

      agent none
        
        stages{

              stage('Quality Gate Status Check'){
                  agent {
                      {
                        docker {
                            image 'maven'
                            args '-v $HOME/.m2:/root/.m2'
                        }
                    }
                  }
                  steps{
                      script{
		    	    sh "mvn clean install"
                 	}

               	 }  
              }	
		
            }	       	     	         
}
