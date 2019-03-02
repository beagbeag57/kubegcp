pipeline {
 agent {
      label "kube-slave"
		 }
 parameters {
        
        choice choices: ['prod', 'dev', 'stag'], description: 'deploy environment', name: 'environment'
    }
 stages {
        stage ('checkout code') {
            steps {
				container(name: 'maven', shell: '/bin/bash') {
                git branch: 'promote',
                    credentialsId: 'githublogin',
                    url: 'https://github.com/satishrawat/kube-pipeline'
                          
            
        }               
    }
	}
    stage ('Connect Kubernetes Cluster ') {
            steps {
			container(name: 'maven', shell: '/bin/bash') {
            sh 'gcloud auth activate-service-account --key-file precise-armor-223618-34d2901961d4.json'
		    sh 'gcloud container clusters get-credentials sk-cluster --zone us-central1-a --project precise-armor-223618'
            }
                
        }
		}
	
	stage ('rollback deployment') {
            steps {
				container(name: 'maven', shell: '/bin/bash') {
				sh 'kubectl rollout history deployment/test -n ${environment}'
				sh 'kubectl rollout undo deployment/test -n ${environment}'
                          
            
        }               
    }
}
}
}
