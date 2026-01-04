DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins

curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 \
    -o $DOCKER_CONFIG/cli-plugins/docker-compose

chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose

docker compose version

docker compose up --build -d
-------------------------------------------------------
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

                use these update version

sudo mkdir -p /usr/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.25.0/docker-compose-linux-x86_64 \
  -o /usr/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/lib/docker/cli-plugins/docker-compose

pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                git 'https://github.com/chaitanya461/hdfcwebsite.git'
            }
        }

        stage('Stop Existing Containers') {
            steps {
                sh 'docker compose down || true'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Run Containers') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Verify Running Containers') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo '✅ Docker images built and containers running successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
        always {
            sh 'docker compose ps'
        }
    }
}


-----
pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/chaitanya461/hdfcwebsite.git'
            }
        }

        stage('Build Role Image') {
            steps {
                dir("$folder") {
                    sh 'docker build -t $image .'
                }
            }
        }

        stage('Run Mobile Container') {
            steps {
                sh '''
                docker rm -f $contain || true
                docker run -d -p $port:80 --name $contain $image
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Images built and containers running successfully'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}
