pipeline {
    agent any
    stages{
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and push the image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'omarsa999',
                                usernameVariable: 'DOCKER_USER',
                                passwordVariable: 'DOCKER_PASS')]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
                bat 'docker build -t omarsa999/mysecondimage:%BUILD_NUMBER% ./myproj2/htmlpage'
                bat 'docker push omarsa999/mysecondimage:%BUILD_NUMBER%'

                bat "docker tag omarsa999/mysecondimage:%BUILD_NUMBER% omarsa999/mysecondimage:latest"
                bat "docker push omarsa999/mysecondimage:latest"
            }
        }

        stage('Deploy K8s cluster with ansible') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible_vm', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    bat '''
                        @echo off
                        setlocal enabledelayedexpansion

                        wsl ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null %SSH_USER%@192.168.1.123 "echo CONNECTED_OK"

                        wsl ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null %SSH_USER%@192.168.1.123 "ansible-playbook -i /home/omar/ansiblefolder/inventory.ini /home/omar/ansiblefolder/apply-cluster.yml --vault-password-file /home/omar/ansiblefolder/vault_key.txt --ssh-extra-args='-tt' -vvv"

                        endlocal
                    '''
                }
            }
        }
    }
}