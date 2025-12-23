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
                sh """
                    docker ps -q --filter "publish=8000" | xargs -r docker stop
                    docker ps -aq --filter "publish=8000" | xargs -r docker rm
                    docker compose up -d
                """
            }
        }
    }
}
