pipeline {
    agent any

    environment {
        BRANCH_NAME = 'main'
        PYTHON_FILE_PATH = 'test.py'
        VENV_PATH = 'venv' // 定义虚拟环境路径
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: "refs/heads/${env.BRANCH_NAME}"]], 
                          userRemoteConfigs: [[url: 'https://github.com/JiangEn25/MMST_JiangEn.git']]])
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                sh """
                python3 -m venv ${env.VENV_PATH}
                source ${env.VENV_PATH}/bin/activate
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                sh """
                source ${env.VENV_PATH}/bin/activate
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Python Script') {
            steps {
                script {
                    echo "Executing Python script: ${env.PYTHON_FILE_PATH}"
                    try {
                        sh """
                        source ${env.VENV_PATH}/bin/activate
                        python ${env.PYTHON_FILE_PATH}
                        """
                        echo "Successfully executed Python script: ${env.PYTHON_FILE_PATH}"
                    } catch (err) {
                        error "Failed to execute Python script: ${env.PYTHON_FILE_PATH}. Error: ${err.message}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'No tests defined.'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Python script executed successfully.'
        }
        failure {
            echo 'Failed to execute the Python script.'
        }
    }
}
