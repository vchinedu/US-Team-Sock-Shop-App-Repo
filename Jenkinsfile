pipeline {
  agent any
  stages {
    stage ('deploying to stage environment'){
      steps {
        sshagent(['ansible-key']) {
          sh 'ssh -t -t ubuntu@10.0.6.100 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage-env.yaml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '24th-june-sock-shop-kubernetes-using-kubeadm', message: 'New Deployment into stage', tokenCredentialId: 'slack'
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
          sh 'ssh -t -t ubuntu@10.0.6.100 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod-env.yaml"'
        }
      }
    }
    stage ('slack notification'){
      steps {
        slackSend channel: '24th-june-sock-shop-kubernetes-using-kubeadm', message: 'New Deployment into production', tokenCredentialId: 'slack'
      }
    }
  }
}
