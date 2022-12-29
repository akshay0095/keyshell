pipeline {
    agent any

    tools {
         nodejs 'nodejs 16.16.0' //Provide Node & npm bin/folder to PATH 

    }

    stages {

        stage('SCM') {
            steps {
                git 'https://github.com/akshay0095/keyshell.git'
            }
        }

        stage('NPM install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'ng build'
            }
            post {
                success {
                    fingerprint '/dist/keyshell/' // to enable the plugin 'fingerprint' to this dir. 
                }
            }
        }

        stage('Deploy') {
            steps { 
                sh '''
                  rm -rf /var/www/html/*
                  sleep 3
                  rsync -avzP dist/keyshell /var/www/html/
                '''
            }
        }

        stage('Artefact') {
            steps {
                sh 'tar cvzf app.tar.gz dist/keyshell/'
                archiveArtifacts artifacts: 'app.tar.gz', fingerprint: true, followSymlinks: false, onlyIfSuccessful: true
            }
        }


    }
}
