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
                git branch: 'rollback',
                    credentialsId: 'githublogin',
                    url: 'https://github.com/techmartguru/kube-poc'
                          
            
        }               
    }
	}
    stage ('Connect Kubernetes Cluster ') {
            steps {
			container(name: 'maven', shell: '/bin/bash') {
            sh 'gcloud auth activate-service-account --key-file precise-armor-223618-34d2901961d4.json'
		    sh 'gcloud container clusters get-credentials kube-poc --zone us-central1-a --project precise-armor-223618'
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
