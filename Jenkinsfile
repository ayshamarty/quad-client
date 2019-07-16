pipeline{
	agent any
        stages{
		stage('---get commit hash---'){
                        steps{
                               sh "tag=$(git rev-parse HEAD)"
                        }
                }
		stage('---set hash as version---'){
                        steps{
                               sh "sed -i "s/{{TAG}}/${tag}/g" ./${JOB_NAME}/deployment.yaml"
                        }
                }
		stage('---build---'){
                        steps{
                               sh "sudo docker build -t ayshamarty/${JOB_NAME}:${tag} ./${JOB_NAME}"
                        }
                }
		stage('---push---'){
			steps{
				sh "sudo docker push ayshamarty/${JOB_NAME}:${tag}"
			}
		}
		stage('---run in kubes---'){
			steps{
				sh "kubectl apply -f ./${JOB_NAME}/deployment.yaml -f ./${JOB_NAME}/service.yaml"
			}
		}
	}
}
