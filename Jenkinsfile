node {
  def app

  stage('Clone repository') {
    checkout scm
  }

  stage('Update GIT') {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
          //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
          sh "git config user.email neopubl@gmail.com"
          sh "git config user.name srsoft"
          sh "git switch ceresinventory/nuxt"
          sh "git config pull.rebase false"
          sh "git pull"
          sh "cat deployment.yaml"
          sh "sed -i 's+harbor.ks.io:8443/ceresinventory/nuxt.*+harbor.ks.io:8443/ceresinventory/nuxt:${DOCKERTAG}+g' deployment.yaml"
          sh "cat deployment.yaml"
          sh "git add ."
          sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
          sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:ceresinventory/nuxt"
        }
      }
    }
  }
}