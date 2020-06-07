def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
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
                    script{
                        withCredentials([string(credentialsId: 'docker_username', variable: 'docker_username'), string(credentialsId: 'docker_password', variable: 'docker_password')]) {
                            sh '''
                                docker build -t $docker_username/jenkins-pipeline-container:$docker_tag .                                  
                                docker login -u $docker_username -p $docker_password
                                docker push $docker_username/jenkins-pipeline-container:$docker_tag
                            '''   
                        }
                    }
                }

            }
            stage('Test connection'){
                steps{
                    sh '''
                        pwd -L
                        ls -ltr
                        ssh -i ./ssh-keys/k8s-key jenkins@35.226.66.135
                        exit
                        '''
                }
            }	
		
        }	       	     	         
}
