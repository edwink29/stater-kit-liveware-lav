node {
  checkout scm
  env.PROD_HOST="10.10.21.86"

  stage("Build") {
    docker.image('composer:2.7').inside('-u root') {
      sh 'composer install'
    }
  }

  stage("Testing") {
    docker.image('ubuntu').inside('-u root') {
      sh 'echo "Ini adalah test"'
    }
  }

  stage('Deploy') {
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
      sshagent (credentials: ['ssh-prod']) {

        sh '''
        mkdir -p ~/.ssh
        ssh-keyscan -H $PROD_HOST >> ~/.ssh/known_hosts

        # SUDAH DIGANTI KE EDWIN
        rsync -rav --delete ./ \
        edwin@$PROD_HOST:/home/edwin/prod.kelasdevops.xyz/ \
        --exclude=.env \
        --exclude=storage \
        --exclude=.git
        '''

      }
    }
  }
}