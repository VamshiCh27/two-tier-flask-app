pipeline{
    agent any
    stages{
        stage("Code Clone"){
            steps{
                git url:"https://github.com/VamshiCh27/two-tier-flask-app.git",branch:"master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t my_app ."
            }
        }
        stage("Test"){
            steps{
                echo "No test cased for now"
            }
        }
        stage("Push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: 'docker_creds',
                    usernameVariable: 'docker_user',
                    passwordVariable: 'docker_pass'
                        )]) {
                             sh '''
                    echo "$docker_pass" | docker login -u "$docker_user" --password-stdin
                    docker image tag my_app "$docker_user/two-teir-flask-app"
                    docker push "$docker_user/two-teir-flask-app"
                    '''
                        }
                echo "Test"
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    
}
