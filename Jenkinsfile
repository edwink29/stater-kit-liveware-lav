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
            ssh-keyscan -H 192.168.1.14 >> ~/.ssh/known_hosts

            # Rsync dengan tambahan exclude .env
            rsync -avz --delete \
            -e "ssh -o StrictHostKeyChecking=no" \
            --exclude=.git \
            --exclude=node_modules \
            --exclude=.env \
            ./ edwin@192.168.1.14:/home/edwin/laravel-app

            # Jalankan perintah SSH untuk memperbaiki permission di server tujuan
            ssh edwin@192.168.1.14 "
                cd /home/edwin/laravel-app && \
                chmod -R 775 storage bootstrap/cache && \
                if [ ! -f .env ]; then echo '.env MISSING!'; fi
            "
            '''
        }
    }
}
}