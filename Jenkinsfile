pipeline {
    agent {label "dev"};
    stages {
        stage("code") {
            steps {
                git url:"https://github.com/Bahadur143/two-tier-flask-app-demo.git", branch:"main"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
    }
        stage("Test") {
            steps {
                echo "Test code"
            }
        }
        stage("push to docker hub"){
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHubCreds",
                passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")])
                {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
post {
            success {
                script {
                    emailext from: "anilsahu350@gmail.com"
                             to: "anilsahu350@gmail.com"
                             body: "Build success for CICD"
                             subject: "Build Success"
                }
            }
    failure {
                script {
                    emailext from: "anilsahu350@gmail.com"
                             to: "anilsahu350@gmail.com"
                             body: "Build failure for CICD"
                             subject: "Build failure"
                }
            }
        }

    
        
        
