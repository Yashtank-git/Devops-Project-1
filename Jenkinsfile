pipeline {
    agent {label "todoapp-agent"}

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'yashtank/dev', url: 'https://github.com/Yashtank-git/Devops-Project-1.git'
                
                // Run docker-compose to build the image
                sh 'docker-compose --version'
                sh '''
                    docker-compose down 
                    docker-compose build
                   '''
            }
            post{
                success{
                    sh 'echo build success'
                }
                failure{
                    sh 'echo build failed'
                }
            }

        }
        stage('Deploy') {
            steps {
                // Run Docker-compose to start the container and expose to port 8000
                
                sh '''
                    docker-compose up -d
                   '''

            }
            post{
                success{
                    sh 'echo deploy success'
                }
                failure{
                    sh 'echo deploy failed'
                }
            }

        }
    }
}
