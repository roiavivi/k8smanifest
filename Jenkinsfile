pipeline {
  agent {
    kubernetes {
      yamlFile 'DeploymentPod.yaml'
    }
  }
  parameters {
    string(name: 'BUILD_NUMBER', defaultValue: '1', description: 'Build number for the job')
  }
  stages {
    stage('Update GIT') {
      steps {
        script {
          catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            withCredentials([usernamePassword(credentialsId: 'GITHUB', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
              sh "git config user.email 'roie710@gmail.com'"
              sh "git config user.name 'roie710'"
              sh "cat manifest/deployment.yaml"
              sh "sed -i 's+roie710/app.*+roie710/app:${params.BUILD_NUMBER}+g' manifest/deployment.yaml"
              sh "cat manifests/deployment.yaml"
              sh "git add ."
              sh "git commit -m 'Done by Jenkins Job change manifest: ${params.BUILD_NUMBER}'"
              sh '''
                git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/k8smanifest.git
                git push origin HEAD:main
              '''
            }
          }
        }
      }
    }
  }
}
