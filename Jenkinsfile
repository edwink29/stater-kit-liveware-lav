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

            # Perubahan 1: Gunakan -rlptD (menghindari error set times/perms pada folder sistem)
            # Perubahan 2: Tambahkan --exclude untuk folder yang isinya dinamis di server
            rsync -rvz --delete \
--no-perms --no-owner --no-group --no-times \
-e "ssh -o StrictHostKeyChecking=no" \
--exclude=.git \
--exclude=node_modules \
--exclude=.env \
--exclude=storage/framework/views/* \
--exclude=storage/logs/* \
./ edwin@192.168.1.14:/home/edwin/laravel-app

            # Perubahan 3: Paksa perbaikan permission setelah rsync selesai
            ssh edwin@192.168.1.14 "
                cd /home/edwin/laravel-app && \
                chmod -R 775 storage bootstrap/cache && \
                php artisan cache:clear || echo 'Artisan not found'
            "
            '''
        }
    }
}
}