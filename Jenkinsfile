node {
    stage("preparation") {
        git credentialsId: 'github_user', url: 'https://github.com/bonnv/LaravelCICD.git'
    }

    stage("Checkout") {
        checkout scm
    }
    
    stage("composer_install") {
        sh 'composer install --ignore-platform-reqs'
        sh 'php artisan dump-autoload'
    }

    stage("environment") {
        sh 'cp .env.example .env'
        sh 'php artisan key:generate'
        //sh 'php artisan config:clear'
		//sh 'php artisan config:cache'
    }

    stage("check_convention") {
        sh 'phpcs --standard=PSR2 app'
    }

    stage("phpunit") {
        sh 'php artisan test'
    }

	stage("Build Docker"){
		//sh 'rsync -avzhP --delete --exclude=.git/ --exclude=Jenkinsifle $WORKSPACE/ root@192.168.1.112:/root/docker/'
		sh 'bash docker-compose.sh'
	}
	
    stage("deploy_product") {
        sh 'cd /var/www/LaravelCICD'
        sh 'git pull origin master'
    }
}