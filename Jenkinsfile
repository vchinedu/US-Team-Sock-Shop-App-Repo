pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.4.105 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage.yml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '18th-september-sock-shop-microservices-kubernetes-project---eu-team-2', message: 'New notification', tokenCredentialId: 'slack'
      }
    }
    stage ('prompt for approval'){
      steps {
        timeout(activity: true, time: 5) {
          input message: 'Review before approval', submitter: 'admin'
        }
      }
    }
    stage ('deploying to prod environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.4.105 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod.yml"'
        }
      }
    }
  }
}
