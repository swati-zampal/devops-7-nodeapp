pipeline {
    agent any
    tools{
        nodejs 'mynode'
    }
    stages {
        stage('git clone') {
            steps {
                echo 'cloning repo from github'
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/swati-zampal/devops-7-nodeapp.git']])
                echo "Cloned successfully..."
            }
        }
       stage('Build') {
            steps {
                echo 'Building myapp'
                sh 'npm install'
                echo 'Build successfully'
            }
        }
       stage('Test') {
            steps {
                echo 'Testing myapp'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
            }
        }
        stage('Deploy') {
            steps {
                script{
                     echo 'Deploying myapp om live server'
                    sshagent(['ssh-crendentials-for-livesever']) {
                      sh '''
                      ssh -o StrictHostKeyChecking=no ubuntu@18.234.87.201<<EOF
                       mkdir nodeapp1
                       cd nodeapp1
                    git init
                    git pull "https://github.com/swati-zampal/devops-7-nodeapp.git" 
                    
                    sudo apt update
                   sudo apt install nodejs npm -y
                    npm install
                    sudo npm install -g pm2
                    pm2 restart app.js || pm2 start app.js
                    exit
                    EOF
                    '''
            }
        }
    }
}
}
}
