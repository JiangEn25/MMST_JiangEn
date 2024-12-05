pipeline {
    agent any

    environment {
        BRANCH_NAME = 'main'
        PYTHON_FILE_PATH = 'test.py'  // 指定要运行的 Python 文件路径
        VENV_PATH = '/var/jenkins_home/venv'  // 定义虚拟环境路径
    }

    stages {
        stage('Clone Repository') {
            steps {
                // 使用 Git 插件克隆仓库
                checkout([$class: 'GitSCM', 
                          branches: [[name: "refs/heads/${env.BRANCH_NAME}"]], 
                          userRemoteConfigs: [[url: 'https://github.com/JiangEn25/MMST_JiangEn.git']]])
            }
        }

        stage('Create Virtual Environment') {
            steps {
                sh """
                if [ ! -d "${env.VENV_PATH}" ]; then
                    python3 -m venv ${env.VENV_PATH}
                fi
                source ${env.VENV_PATH}/bin/activate
                """
            }
        }

        stage('Install Dependencies') {
            steps {
                sh """
                source ${env.VENV_PATH}/bin/activate
                pip install --upgrade pip
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
            cleanWs() // 清理工作空间
        }
        success {
            echo 'Python script executed successfully.'
        }
        failure {
            echo 'Failed to execute the Python script.'
        }
    }
}
