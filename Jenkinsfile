pipeline {
    agent 'any'
    environment {
        branchName = "{env.GIT_BRANCH.split('/').size() == 1 ? env.GIT_BRANCH.split('/')[-1] : env.GIT_BRANCH.split('/')[1..-1].join('/')}"
        key = credentials('175ad537-d702-46aa-82eb-bea8014b6252')
        destination = 'ashay@ec2-184-72-108-72.compute-1.amazonaws.com'
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
                scp -O -o StrictHostKeyChecking=no -r -i ${key} /home/ashay/workdir/test3/ ${destination}:/home/ashay/prod/
                '''
            }
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
