pipeline {
    agent any
    stages {
        stage ('Initial Clean Up') {
            steps {
               deleteDir()
            }
        }
        stage ('Code Initialization') {
            steps {
                git 'https://github.com/ManojKumar-dnac/Git-Practice-Repo.git'
            }
        }
        stage ('Docker Deploy') {
            steps {
                sh 'docker run -itd --name cont1 -p $PORT_NUMBER:80 nginx'
            }
        }
    }
}
