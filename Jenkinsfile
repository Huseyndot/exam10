pipeline {
    agent any
    environment {
      ANSIBLE_PRIVATE_KEY=credentials('ssh-key')
    }
    triggers {
       pollSCM '* * * * *'
    }
    stages {
        stage('Build and Push') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'Docker-hub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                   sh "docker build -t huseyndot/exam10:v$BUILD_ID ."
                   sh "docker login -u $user -p $pass"
                   sh "docker push Huseyndot/exam10:v$BUILD_ID"
                }
            }
        }
        stage('Deploy') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'Docker-hub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                   sh "ansible-playbook playbook.yaml -u ansible --private-key=ANSIBLE_PRIVATE_KEY -e username=$user -e password=$pass -e BUILD_ID=$BUILD_ID --become -i inventory"
                }
            }
        }
    }
}
