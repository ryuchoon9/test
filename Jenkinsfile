node {
  stage ('======== Clone repository ========') {
    checkout scm
  }
  stage('======== Build image ========') {
    sh "git pull"
    app = docker.build("shryu1/test")
  }
  stage('======== Push image ========') {
    docker.withRegistry('https://hub.docker.com/r/shryu1/test', 'docker') {
       app.push("${env.BUILD_NUMBER}")
       app.push("latest")
    }
  }
  stage('======== Update YAML file ========') {
    sh "git pull"
    sh "sed -i s%shryu1/test:.*%shryu1/test:${env.BUILD_NUMBER}%g nginx.yaml"
    sh "cat nginx.yaml | grep image:"
    sh "git add nginx.yaml"
    sh "git commit -m 'image tag update ${env.BUILD_NUMBER}'"
    sh "git push -u origin master"
  }
}

