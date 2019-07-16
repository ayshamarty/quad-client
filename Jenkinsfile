pipeline{
	agent any
        stages{
		stage('---get commit hash---'){
                        steps{
                               sh "tag=\$(git rev-parse HEAD)"
                        }
                }
		stage('---set hash as version---'){
                        steps{
                               sh "sed -i \"s/{{TAG}}/\${tag}/g\" ./deployment.yaml"
                        }
                }
		stage('---build---'){
                        steps{
                               sh "sudo docker build . -t ayshamarty/\${JOB_NAME}:$tag"
                        }
                }
		stage('---push---'){
			steps{
				sh "sudo docker push ayshamarty/${JOB_NAME}:${tag}"
			}
		}
		stage('---run in kubes---'){
			steps{
				sh "kubectl apply -f ./deployment.yaml -f ./service.yaml"
			}
		}
	}
}

