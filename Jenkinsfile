pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.4.171 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage.yml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '26th-june--sock-shop-microservices-kubernetes-project---us-team', message: 'New notification', tokenCredentialId: 'slack'
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
          sh 'ssh -t -t ubuntu@10.0.4.171 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod.yml"'
        }
      }
    }
  }
}
