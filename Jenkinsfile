pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    //proses mulai dari mengambil isian repositori dari github, sampai ke tahap deployment ke prod
    stages {
        stage('Checkout/Mengambil Repositori dari GitHub') {
            steps {
                git url: 'https://github.com/afighufran/next-jenkins.git'
            }
        }

        //instalasi dependensi
        stage('Install Dependencies Next') {
            steps {
                sh 'npm install'
            }
        }

        //build dependensi setelah di instal
        stage('Build setelah Install Dependencies') {
            steps {
                sh 'npm run build'
            }
        }

        //deploy ke branch development jika pushnya mengarah ke dev branch
        stage('Testing di Branch Deployment') {
            when {
                branch 'development'
            }
            steps {
                sh 'npm run test'
            }
        }

        //deploy ke branch staging jika pushnya mengarah ke staging branch
        stage ('Proses Staging') {
            when {
                branch 'staging'
            }
            steps {
                echo 'Deploy ke Environment Staging'
                sh 'npm run deploy:staging'
            }
        }

        //deploy ke branch production jika pushnya mengarah ke prod branch
        stage ('Proses Deploy ke Production') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploy ke Production Staging'
                sh 'npm run deploy:production'
            }
        }

        //archive build .next dan rekursif sebagai artefak
        stage('Archive Build') {
            steps {
                archiveArtifacts artifacts: './next**', allowEmptyArchive: true
            }
        }
    }

    //membersihkan workspace setelah pipeline dijalankan
    post {
        always {
            junit '**/test-results.xml'
            cleanWs()
        }
    }
}