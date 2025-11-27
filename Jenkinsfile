pipeline {
  agent any

  stages {
    stage('Preparing Infrastructure') {
      steps {
        echo '-- Preparing Infrastructure Files Structure --'
        sh '''
#!/bin/bash
#sudo mkdir -p /etc/kolla
#sudo chown kolla:kolla /etc/kolla
git checkout dev_deploy
sudo cp -r /home/longth1/kolla-ansible/etc/kolla/globals.yml /etc/kolla/
'''
      }
    }

    stage('Secrets Setup') {
      steps {
        echo '-- Generating OpenStack Services Secrets --'
        sh '''
bash -c '
set -x
whoami
hostname
id
groups
git checkout dev_deploy
ls -l /home/longth1/kolla-ansible/local/bin/activate
cat /home/longth1/kolla-ansible/local/bin/activate | head -n 5
source /home/longth1/kolla-ansible/local/bin/activate
#kolla-genpwd -p /etc/kolla/passwords.yml
'
'''
      }
    }

    stage('Infrastructure Pre-Checks') {
      steps {
        echo '-- Running Ansible Kolla Prechecks Script --'
        sh '''
bash -c '
source /home/longth1/kolla-ansible/local/bin/activate
cp -r "/home/longth1/kolla-ansible/ansible/inventory/"* /etc/kolla/
kolla-ansible -i /etc/kolla/all-in-one prechecks
'
'''
      }
    }

    stage('Deploy Infrastructure') {
      steps {
        echo '-- Running Ansible Kolla Deploy Script --'
        sh '''
bash -c '
source /home/longth1/kolla-ansible/local/bin/activate
kolla-ansible -i /etc/kolla/all-in-one deploy
'
'''
      }
    }
  }
}
