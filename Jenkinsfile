pipeline{

      agent any

      tools {
          maven 'Maven'
      }
        
        stages{

            stage('Build'){                
                steps{
                    script{
                        sh 'mvn clean install'
                 	}

               	}  
            }	
		
        }	       	     	         
}
