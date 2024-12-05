pipeline {
    agent any  // 使用任何可用的 Jenkins Agent

    environment {
        BRANCH_NAME = 'main'
        PYTHON_FILE_PATH = 'test.py'  // 指定要运行的 Python 文件路径
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

        stage('Install Dependencies') {
            steps {
                sh """
                # 安装依赖项（如果需要）
                pip3 install -r requirements.txt
                """
            }
        }

        stage('Run Python Script') {
            steps {
                script {
                    echo "Executing Python script: ${env.PYTHON_FILE_PATH}"
                    try {
                        sh """
                        python3 ${env.PYTHON_FILE_PATH}
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
