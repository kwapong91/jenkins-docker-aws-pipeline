pipeline {
    agent any

    stages {
        stage('Clone GitHub Repo') {
            steps {
                git branch: 'may1-log', url: 'https://github.com/kwapong91/devops-journal.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    cd docker-practice
                    docker build -t awesome-app:${env.BUILD_ID} .
                """
            }
        }

        stage('AWS CLI SetUp/ECR Push') {
            environment {
                AWS_ACCESS_KEY_ID     = credentials('AWS_Accesskey')
                AWS_SECRET_ACCESS_KEY = credentials('AWS_SecretKey')
                AWS_DEFAULT_REGION    = 'us-east-1'
            }
            steps {
                script {
                    def tag = env.BUILD_ID
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 831926615921.dkr.ecr.us-east-1.amazonaws.com
                        docker tag awesome-app:${tag} 831926615921.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:${tag}
                        docker push 831926615921.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:${tag}
                    """
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def tag = env.BUILD_ID
                    sshagent(credentials: ['ec2-ssh-key']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ubuntu@52.23.21.86 '
                                docker pull 831926615921.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:${tag}
                                docker rm -f awesome-app || true
                                docker run -d -p 80:5000 --name awesome-app 831926615921.dkr.ecr.us-east-1.amazonaws.com/ecr-repo:${tag}
                            '
                        """
                    }
                }
            }
        }
    }
}
