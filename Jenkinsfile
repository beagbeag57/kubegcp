pipeline {
 agent {
      label "kube-slave"
		 }
 parameters {
        string (defaultValue: "10", description: 'Deployment Version', name: 'version')
        choice choices: ['prod', 'dev', 'stag'], description: 'deploy environment', name: 'environment'
    }
 stages {
        stage ('checkout code') {
            steps {
				container(name: 'maven', shell: '/bin/bash') {
                git branch: 'promote',
                    credentialsId: 'githublogin',
                    url: 'https://github.com/techmartguru/kube-poc'
                          
            
        }               
    }
	}
    stage ('Connect Kubernetes Cluster ') {
            steps {
			container(name: 'maven', shell: '/bin/bash') {
            sh 'gcloud auth activate-service-account --key-file kube-cluster-231707-6b9613e3dd78.json'
		    sh 'gcloud container clusters get-credentials kube-poc --zone us-central1-a --project kube-cluster-231707'
            }
                
        }
		}
        stage ('Deploy the App ') {
            steps {
			container(name: 'maven', shell: '/bin/bash') {
            sh 'sed -i s/hello-docker:10/hello-docker:${version}/g test-app.yaml'
            sh 'kubectl apply -f test-app.yaml -n ${environment}'
            
            }
                
        }
    }
 }
    
}
