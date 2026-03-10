node {
  checkout scm

  // Build Stage
  stage("Build") {
  docker.image('composer:2.7').inside('-u root') {
    sh 'composer install'
  }
}

  // Testing Stage
  stage("Testing") {
    docker.image('ubuntu').inside('-u root') {
      sh 'echo "Ini adalah test"'
    }
  }

  // Deploy to production stage
  stage('Deploy') {
    docker.image('agung3wi/alpine-rsync:1.1').inside {
        sshagent(['ssh-prod']) {
            sh '''
            mkdir -p ~/.ssh
            ssh-keyscan -H 192.168.1.14 >> ~/.ssh/known_hosts

            rsync -avz --delete ./ edwin@192.168.1.14:/var/www/laravel
            '''
        }
    }
}
}
