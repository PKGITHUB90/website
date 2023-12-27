pipeline{
    agent none
 environment {
	DOCKERHUB_CREDENTIAL = credentials("fcd47733-f244-4217-b59d-f4a4e39f026f")
 }
 stages {
	stage ("git") {
		agent {
		    label "k8s-master"
		}
		steps {
			script {
				git "https://github.com/PKGITHUB90/website.git"
			}
		}
	}
	stage ("docker") {
		agent {
		    label "k8s-master"
		}		
		steps {
			script {
				sh "sudo docker build /home/ubuntu/jenkins/workspace/test-pipeline/ -t pkt90docker/devopswebsite"
				sh "sudo docker login -u ${DOCKERHUB_CREDENTIAL_USR} -p ${DOCKERHUB_CREDENTIAL_PSW}"
				sh "sudo docker push pkt90docker/devopswebsite"
			}
		}
	}
	stage ("k8s deploy") {
		agent {
		    label "k8s-master"
		}		
		steps {
			script {
				sh "kubectl delete deploy website-deployment"
				sh "kubectl apply -f deploy.yaml"
				sh "kubectl delete svc my-service"
				sh "kubectl apply -f service.yaml"
			}
		}
	}	
 }
}
