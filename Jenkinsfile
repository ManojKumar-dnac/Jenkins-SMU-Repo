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
        stage ('Final Stage') {
            steps {
                echo "My Final Stage"
            }
        }
    }
}
