node {
    checkout scm

    stage("Build") {

      docker.image('composer:2.7').inside('-u root') {

      sh 'composer install'

      }

      }

    stage("Test") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage("Deploy") {
    docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
        sshagent (credentials: ['ssh-prod']) {
            sh '''
            mkdir -p ~/.ssh
            chmod 700 ~/.ssh
            
            ssh-keyscan -H 10.10.182.123 >> ~/.ssh/known_hosts

            ssh -o BatchMode=yes -o StrictHostKeyChecking=no edwin@10.10.182.123 "echo CONNECTED"

            # Rsync dengan opsi SSH khusus
            rsync -avz --delete \
            -e "ssh -o StrictHostKeyChecking=no" \
            ./ edwin@10.10.182.123:/home/edwin/laravel-app\
            --exclude=.git \
            --exclude=node_modules
            '''
        }
    }
}
}