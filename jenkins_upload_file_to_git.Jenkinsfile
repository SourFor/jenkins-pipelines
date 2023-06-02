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
                    git branch: 'main', url: 'git@github.com:SourFor/jenkins-pipelines.git'
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
                sh 'printenv'
                sh 'ls -la'
                sh 'sleep 5'
                sh "ls -la ${workspace}"
                // new hudson.FilePath(new File("${file_to_push}")).copyFrom(file_to_push)
                // file_to_push.delete()
                sh "ls -la ${workspace}"
                // sh "cp ${workspace}/${large} ./"
                unstash 'FILE'
                sh 'mv FILE $FILE_FILENAME'
                withCredentials([sshWithPrivateKey(credentialsId: 'sourfor-ssh-git',
                  gitToolName: 'git-tool')]) {
                    sh 'git config user.name SourFor'
                    sh 'git config user.email sour-89@mail.ru'
                    sh 'git add .'
                    sh "git commit -m 'Added $FILE_FILENAME'"
                    sh 'git push origin main'
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