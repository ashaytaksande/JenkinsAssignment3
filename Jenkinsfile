pipeline {
    agent 'any'
    environment {
        branchName = "{env.GIT_BRANCH.split('/').size() == 1 ? env.GIT_BRANCH.split('/')[-1] : env.GIT_BRANCH.split('/')[1..-1].join('/')}"
    }
    stages {
        stage('Once push is made to “develop” branch in git, trigger job “test"') {
            agent {
                label 'TestNode'
            }
            when {
                expression {
                    branchName == 'develop'
                }
            }
            steps {
                sh '''
                echo 'copid git files to "$(pwd)"'
                ls -al
                '''
            }
            stage('trigger another job') {
                steps {
                    build 'prod'
                }
            }
            stage('message') {
                steps {
                    sh 'echo both jobs completed successfully'
                }
            }
        }
    }
}
