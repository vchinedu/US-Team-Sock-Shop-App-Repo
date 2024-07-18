pipeline {
  agent any
  stages {
    stage ('deploying to stage environment') {
      steps {
        sshagent(['ansible-key']) {
          sh '''
            ssh -t -t -o StrictHostKeyChecking=no -o ProxyCommand="ssh -W %h:%p -o StrictHostKeyChecking=no ubuntu@52.208.21.171" ubuntu@10.0.6.100 "ansible-playbook /home/ubuntu/playbooks/stage-env.yaml"
          '''
        }
      }
    }
    // stage ('deploying to stage environment'){
    //   steps {
    //     sshagent(['ansible-key']) {
    //       sh 'ssh -t -t ubuntu@10.0.6.100 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/stage-env.yaml"'
    //     }
    //   }
    // }
    stage ('slack notification a'){
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
    stage ('deploying to prod environment') {
      steps {
        sshagent(['ansible-key']) {
          sh '''
            ssh -t -t -o StrictHostKeyChecking=no -o ProxyCommand="ssh -W %h:%p -o StrictHostKeyChecking=no ubuntu@52.208.21.171" ubuntu@10.0.6.100 "ansible-playbook /home/ubuntu/playbooks/prod-env.yaml"
          '''
        }
      }
    }

    // stage ('deploying to prod environment'){
    //   steps {
    //     sshagent(['ansible-key']) {
    //       sh 'ssh -t -t ubuntu@10.0.6.100 -o StrictHostKeyChecking=no "ansible-playbook /home/ubuntu/playbooks/prod-env.yaml"'
    //     }
    //   }
    // }
    stage ('slack notification b'){
      steps {
        slackSend channel: '24th-june-sock-shop-kubernetes-using-kubeadm', message: 'New Deployment into production', tokenCredentialId: 'slack'
      }
    }
  }
}
