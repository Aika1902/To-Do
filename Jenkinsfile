pipeline {
    agent any
    tool {
        
    }
    environment {
        EC2_IP = '18.218.241.19'
          EC2_USER = 'ubuntu'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Aika1902/To-Do.git'
            }
        }
        stage('Build') {
            steps {
                sh '''
                cd todo
                npm install
                npm run build
                '''
            }
        }
        stage('Deploy') {
            steps {
                 sshagent(['your-ssh-credential-id']) {
                    sh """
                    scp -r todo/build ${EC2_USER}@${EC2_IP}:/home/${EC2_USER}/react-app
                    ssh ${EC2_USER}@${EC2_IP} << EOF
                    sudo rm -rf /var/www/html/*
                    sudo cp -r /home/${EC2_USER}/react-app/* /var/www/html/
                    sudo systemctl restart nginx
                    EOF
                    """
                }
            }
        }
    
    }
}