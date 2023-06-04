pipeline {
    agent any
    
    parameters {
        stashedFile name: 'FILE', description: 'Select the file to push to Git'
        // file(name: 'new_file', description: 'Select the file to push to Git')
    }

    stages {
        stage('Clone Git repository') {
            steps {
                sshagent(['sourfor-ssh-git']) {
                    // git branch: 'main', url: 'git@github.com:SourFor/jenkins-pipelines.git'
                    sh 'GIT_SSH_COMMAND="ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no" git clone git@github.com:SourFor/jenkins-pipelines.git' // fix name
                }
            }
        }

        stage('Push file to Git') {
            when{
                expression {
                    env.FILE_FILENAME
                }
            }
            steps {
                //TODO: убрать персональную информацию из названия переменных и кода (максимально нейтральные, обезличенные )
                sshagent(['sourfor-ssh-git']) { // fix name
                    dir('jenkins-pipelines'){
                    unstash 'FILE'
                    sh 'mv FILE $FILE_FILENAME'
                    sh 'git config user.name SourFor' // fix name
                    sh 'git config user.email sour-89@mail.ru' // fix name
                    sh 'git add .'
                    sh "git commit -m 'Added $FILE_FILENAME'"
                    sh 'GIT_SSH_COMMAND="ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no" git push origin main'    
                    }
                    
                  }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}