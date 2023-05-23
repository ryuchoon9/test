node {
  stage ('======== Clone repository ========') {
    checkout scm
  }
  stage('======== Build image ========') {
    sh "git switch main"
    sh "git config --global user.email 'admin@example.com'"
    sh "git config --global user.name 'admin'"
    sh "git pull origin main"
    app = docker.build("shryu1/test")
  }
  stage('======== Push image ========') {
    docker.withRegistry('https://223.130.133.184', 'harbor') {
       app.push("${env.BUILD_NUMBER}")
       app.push("latest")
    }
  }
  stage('======== Update YAML file ========') {
    sh "git switch yaml"
    sh "git pull origin yaml"
    sh "sed -i s%223.130.133.184/test:.*%223.130.133.184/test:${env.BUILD_NUMBER}%g nginx.yaml"
    sh "cat nginx.yaml | grep image:"
    sh "git add ."
    sh "git commit -m 'image tag update ${env.BUILD_NUMBER}'"
    sh "git push -u origin yaml"
  }
}

