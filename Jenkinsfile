// pipeline {
//     agent any

//     environment {
//         VERCEL_TOKEN = credentials('vercel_token')
//     }

//     stages {
//         stage('Install') {
//             steps {
//                 bat 'npm install'
//             }
//         }
//         stage('Test') {
//             steps {
//                 echo 'Skipping tests - no test script found'
//             }
//         }
//         stage('Build') {
//             steps {
//                 bat 'npm run build'
//             }
//         }
//         stage('Deploy') {
//             steps {
//                 bat 'npx vercel --prod --yes --token=%VERCEL_TOKEN%'
//             }
//         }
//     }
// }

node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace...'
        deleteDir()
    }

    stage('Clone Repository') {
        echo 'Cloning repository...'
        git(
            branch: 'main',
            url: 'https://github.com/Pushpak-kumar1/CI-CD-Pipeline_Auto-Deploy-Vercel'
        )
    }
    stage('Deploy to EC2') {
        echo 'Deploying to EC2...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            sudo run start
            """
    }
}