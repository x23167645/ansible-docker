pipeline {
  agent any

  environment {
    // Update this with your actual GitHub credentials ID from Jenkins
    GIT_CREDENTIALS_ID = 'github-creds'  // Or 'github-ssh' if using SSH
  }

  stages {
    stage('Clone Code') {
      steps {
        git credentialsId: "${GIT_CREDENTIALS_ID}", url: 'https://github.com/x23167645/ansible-docker.git', branch: 'main'
      }
    }

    stage('Install Ansible (if needed)') {
      steps {
        sh '''
          if ! command -v ansible > /dev/null; then
            echo "Installing Ansible..."
            sudo apt-get update
            sudo apt-get install -y software-properties-common
            sudo add-apt-repository --yes --update ppa:ansible/ansible
            sudo apt-get install -y ansible
          else
            echo "Ansible already installed"
          fi
        '''
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        sh 'ansible-playbook -i inventory.ini deploy.yml'
      }
    }
  }

  post {
    success {
      echo '✅ Deployment completed successfully.'
    }
    failure {
      echo '❌ Deployment failed.'
    }
  }
}

