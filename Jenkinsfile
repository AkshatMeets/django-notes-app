@Library("Shared") _
pipeline {
    agent { label "vinod" }

    stages {
        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("code") {
            steps {
                echo "Cloning code"
                script{
                clone("https://github.com/AkshatMeets/django-notes-app.git","main")
                }
            }
        }

        stage("build") {
            steps {
              script{
                docker_build("notes-app","latest","akshatmeets")
                }
            }
        }

        stage("Push to DockerHub") {
                steps {
                   script{
                       docker_push("notes-app","latest","akshatmeets")
                   }
                }
            }

            stage("Deploy") {
                steps {
                    sshagent(['ec2-ssh-key']) {
                        sh """
                          ssh -o StrictHostKeyChecking=no ubuntu@16.171.13.137 '
                            cd /home/ubuntu/django-notes-app &&
                            docker pull akshatmeets/notes-app:latest &&
                            docker compose down || true &&
                            docker compose up -d
                          '
                        """
                    }
                }
            }

    }
}
