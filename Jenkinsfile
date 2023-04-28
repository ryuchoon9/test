node {
  stage ('======== Clone repository ========') {
    checkout scm
  }
  stage('======== Build image ========') {
    sh "git switch jenkins"
    sh "git pull"
    app = docker.build("test/nginx")
  }
  stage('======== Push image ========') {
    docker.withRegistry('https://shryu-ncr.kr.ncr.ntruss.com', 'ncr') {
       app.push("${env.BUILD_NUMBER}")
       app.push("latest")
    }
  }
  stage('======== Update YAML file ========') {
    sh "git switch yaml"
    sh "git pull"
    sh "git config --global user.email 'shryu@cloit.com'"
    sh "git config --global user.name 'shryu'"
    sh "sed -i s%shryu-ncr.kr.ncr.ntruss.com/test/nginx:.*%shryu-ncr.kr.ncr.ntruss.com/test/nginx:${env.BUILD_NUMBER}%g nginx.yaml"
    sh "cat nginx.yaml | grep image:"
    sh "git add nginx.yaml"
    sh "git commit -m 'image tag update ${env.BUILD_NUMBER}'"
    sh "git push -u origin yaml"
  }
}

