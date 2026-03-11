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
      ssh-keyscan -H 10.10.5.108 >> ~/.ssh/known_hosts

      rsync -avz --delete ./ edwin@10.10.5.108:/var/www/laravel
      '''
    }
  }
}
}

// node {
//   checkout scm

//   stage("Build") {
//     docker.image('composer:2.7').inside('-u root') {
//       sh 'composer install'
//     }
//   }

//   stage("Testing") {
//     docker.image('ubuntu').inside('-u root') {
//       sh 'echo "Ini adalah test"'
//     }
//   }

//   stage('Deploy') {
//         sh '''
//         mkdir -p /var/www/laravel
//         rsync -avz --delete ./ /var/www/laravel
//         '''
//     }
// }
