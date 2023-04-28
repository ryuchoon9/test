node {
  stage ('======== Clone repository ========') {
    checkout scm
  }
  stage('======== Build image ========') {
    sh "git pull origin master"
    sh "git config --global user.email 'shryu@cloit.com'"
    sh "git config --global user.name 'shryu'"
    app = docker.build("shryu1/test")
  }
  stage('======== Push image ========') {
    docker.withRegistry('', 'docker') {
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

