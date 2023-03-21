pipeline {
    // The “agent” section configures on which nodes the pipeline can be run.
    // Specifying “agent any” means that Jenkins will run the job on any of the
    // available nodes.

	agent any

    stages {
        stage('Git Pull') {
            steps {
                // Get code from a GitHub repository
                // Make sure to add your own git url and credentialsId
				git url: 'https://github.com/ramrodwal/SPE_miniProject_Calculator.git',
				branch: 'master',
                credentialsId: 'GitCredential'
            }
        }
        stage('Maven Build') {
            steps {
                // Maven build, 'sh' specifies it is a shell command
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Images') {
            steps {
                sh 'docker build -t ramrodwal/miniproject_calculator:latest .'
            }
        }
        stage('Publish Docker Images') {
            steps {
                withDockerRegistry([ credentialsId: "docker", url: "" ]) {
                    sh 'docker push ramrodwal/miniproject_calculator:latest'
                }
            }
        }
        stage('Clean Docker Images') {
            steps {
                sh 'docker rmi -f ramrodwal/miniproject_calculator:latest'
            }
        }
        stage('Deploy and Run Image'){
            steps {
                sh 'echo "hello calculator"'
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'playbook.yml', sudoUser: null
            }
        }
    }
}