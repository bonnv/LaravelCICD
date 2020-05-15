node {
    stage("preparation") {
        git credentialsId: 'f8978de1-8213-4590-b216-cbdc12869ca1', url: 'https://github.com/thanhgiong195/demoCICD.git'
    }

    stage("Checkout") {
        checkout scm
    }
    
    stage("composer_install") {
        sh 'composer install --ignore-platform-reqs'
    }

    stage("environment") {
        sh 'cp .env.example .env'
        sh 'php artisan key:generate'
    }

    stage("check_convention") {
        sh 'phpcs --standard=PSR2 app'
    }

    stage("phpunit") {
        sh 'php artisan test'
    }

	stage("Build Docker"){
		sh 'rsync -avzhP --delete --exclude=.git/ --exclude=Jenkinsifle $WORKSPACE/ root@192.168.1.112:/root/docker/'
		//build docker & mount source to /var/www/html
		sh 'ssh root@192.168.1.112 "cd /root/docker/ && bash docker-compose.sh"'
	}
	
    stage("deploy_product") {
        sh 'cd /var/www/demoCICD'
        sh 'git pull origin master'
    }
}