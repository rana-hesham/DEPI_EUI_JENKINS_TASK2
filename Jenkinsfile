pipeline {
    agent any
    stages {
        stage(clone code) {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github_user_password', usernameVariable: 'username', passwordVariable: 'pass')]) {
                sh 'cd /home/rana/Desktop/'
                sh 'git clone git@github.com:rana-hesham/DEPI_EUI_JENKINS_TASK2.git'
                }
            }
        }        
        stage(run app) {
            steps {
                sh 'cd MyApp/'
                sh 'dotnet build'
                sh 'sudo dotnet publish -c Release -o /var/www/ --runtime linux-x64'
                sh 'sudo systemctl daemon-reload'
                sh 'sudo systemctl start app'
                sh 'sudo systemctl status app'
            }
        }
        stage(build image) {
            steps {
                sh 'cd MyApp/'
                sh 'docker build -t mydotnetapp .'
            }
        }
        stage(run container) {
            steps {
                sh 'sudo docker run -d -p 8080:80 --name dotnet mydotnetapp'
                }
            }
        }
        stage(push to dockerhub) {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'username', passwordVariable: 'pass')]) {
                sh 'sudo docker push ranahesham/mydotnetapp:byjenkins'
                }
            }                                    
        }
    }
}
