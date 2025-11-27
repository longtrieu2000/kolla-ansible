pipeline {
  agent any
  stages {
    stage('Preparing Infrastructure') {
          steps {
            echo '--Preparing Infrastructure Files Structure --'
            sh ''' #!/bin/bash
                sudo mkdir -p /etc/kolla
                sudo chown kolla:kolla /etc/kolla
                cp -r /home/longth1/kolla-ansible/etc/kolla/globals.yml \
                /etc/kolla/
                '''
               }
             }
    stage('Secrets Setup') {
       steps {
                echo '--Generating OpenStack Services Secrets --'
                sh '''#!/bin/bash
                    kolla-genpwd -p /etc/kolla/passwords.yml
                   '''
              }
            }
        
    stage('Infrastructure Pre-Checks') {
       steps {
                echo '--Running Ansible Kolla Prechecks Script --'
                sh '''#!/bin/bash
                    kolla-ansible -i /etc/kolla/all-in-one prechecks
                   '''
              }
            }
    stage('Deploy Infrastructure') {
       steps {
                echo '--Running Ansible Kolla Prechecks Script --'
                sh '''#!/bin/bash
                kolla-ansible -i /etc/kolla/all-in-one deploy
                '''
              }
            }
   }
}
