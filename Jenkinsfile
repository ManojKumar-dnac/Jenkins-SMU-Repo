pipeline {
  agent any

  stages {
    stage('Initial Cleanup') {
      steps {
        echo 'This is Initial Cleanup'
        deleteDir()
      }
    }

    stage('Code Pull') {
      steps {
        echo 'Cloning code from GitHub'
        git branch: 'main', url: 'https://github.com/ManojKumar-dnac/Jenkins-SMU-Repo.git'
      }
    }

    stage('Docker Image Build') {
      steps {
        echo 'Building Docker image'
        sh "docker build -t ${params.GIT_IMAGE_NAME}:latest ."
      }
    }

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
          '''
        }
      }
    }

    stage('Docker Image Tag') {
      steps {
        sh "docker tag ${params.GIT_IMAGE_NAME}:latest ${params.DOCKER_USER_NAME}/${params.GIT_IMAGE_NAME}:latest"
      }
    }

    stage('Docker Image Push') {
      steps {
        sh "docker push ${params.DOCKER_USER_NAME}/${params.GIT_IMAGE_NAME}:latest"
      }
    }

    stage('Remove Old Container') {
      steps {
        sh "docker rm -f ${params.CONT_NAME} || true"
      }
    }

    stage('Docker Container Deploy') {
      steps {
        echo 'Running Docker container'
        sh "docker run -d --name ${params.CONT_NAME} -p ${params.PORT_NUM}:80 ${params.GIT_IMAGE_NAME}:latest"
      }
    }
  }
}
