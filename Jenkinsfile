def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdOut: true
    return tag;
}
pipeline{

      agent any

      environment {
          docker_tag = getDockerTag()
      }

      tools {
          maven 'Maven'
      }
        
        stages{

            stage('Quality Gate Status Check'){                
                steps{
                    script{
                        withSonarQubeEnv('sonar-server'){
                            sh 'mvn sonar:sonar'
                        }
                    timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualityGate()
                            if (qg.status != 'OK'){
                                error "Pipeline aborted due to Quality gate failure: ${qg.status}"
                            }                                
                    }
                    sh 'mvn clean install'
                        
                 	}

               	}  
            }
            stage('Build'){
                steps{
                    step{
                        sript{
                            withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'docker-password', usernameVariable: 'docker-username')]) {
                                sh '''
                                    docker build -t $docker-username/jenkins-pipeline-container:$docker_tag .                                  
                                    docker login -u $docker-username -p $docker-password
                                    docker push $docker-username/jenkins-pipeline-container:$docker_tag
                                 '''   
                            }
                        }
                    }
                }

            }	
		
        }	       	     	         
}
