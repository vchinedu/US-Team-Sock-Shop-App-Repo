pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.2.138 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbook/stage.yml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '5th-feb-sock-shop-project-eu-team-1', message: 'New notification', tokenCredentialId: 'slack'
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
          sh 'ssh -t -t ubuntu@10.0.2.138 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbook/prod.yml"'
        }
      }
    }
  }
}
