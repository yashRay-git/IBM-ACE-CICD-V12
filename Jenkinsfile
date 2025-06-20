pipeline {
    agent any

    environment {
        ACE_PATH = 'C:\\Program Files\\IBM\\ACE\\12.0.12.0\\server\\bin'
        WORKSPACE_DIR = "${env.WORKSPACE}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashRay-git/IBM-ACE-CICD-V12.git'
            }
        }

        stage('Load ACE CLI') {
            steps {
                bat """
                call "${ACE_PATH}\\mqsiprofile.cmd"
                """
            }
        }

        stage('Build BAR') {
            steps {
                bat """
                cd "%WORKSPACE%"
                if exist myApp.bar del myApp.bar
                mqsicreatebar -data "%WORKSPACE%" -b myApp.bar -a myApp
                """
            }
        }

        stage('Deploy to ACE') {
            steps {
                bat """
                "${ACE_PATH}\\mqsideploy.exe" --integration-node b1 --integration-server server --bar-file myApp.bar --timeout-seconds 300
                """
            }
        }
    }
}
