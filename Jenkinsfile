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
                    script{
                        withCredentials([string(credentialsId: 'docker_username', variable: 'docker_username'), string(credentialsId: 'docker_password', variable: 'docker_password')]) {
                            sh '''
                                echo 'The docker tag is ' $docker_tag
                                docker build -t $docker-username/jenkins-pipeline-container:$BUILD_NUMBER .                                  
                                docker login -u $docker-username -p $docker-password
                                docker push $docker-username/jenkins-pipeline-container:$BUILD_NUMBER
                                '''   
                        }
                    }
                }

            }	
		
        }	       	     	         
}
