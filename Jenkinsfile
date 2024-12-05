pipeline {
    agent any

    environment {
        BRANCH_NAME = 'main'
        NOTEBOOK_PATH = 'lab3.ipynb'  // 指定要运行的 notebook 文件路径
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
                // 假设有一个 requirements.txt 文件列出所有依赖项
                sh 'pip install -r requirements.txt'
                // 安装 nbconvert 用于转换或执行 notebook
                sh 'pip install nbconvert'
            }
        }

        stage('Run Specific Jupyter Notebook') {
            steps {
                script {
                    echo "Executing specific notebook: ${env.NOTEBOOK_PATH}"
                    try {
                        sh "jupyter nbconvert --execute --to notebook --inplace --ExecutePreprocessor.timeout=600 ${env.NOTEBOOK_PATH}"
                        echo "Successfully executed notebook: ${env.NOTEBOOK_PATH}"
                    } catch (err) {
                        error "Failed to execute notebook: ${env.NOTEBOOK_PATH}. Error: ${err.message}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // 如果需要添加测试步骤，请在此处定义。
                // 例如运行单元测试或其他验证过程。
                // 如果当前没有测试，则可以保留为空步骤或注释掉此阶段。
                echo 'No tests defined.'
            }
        }
    }

    post {
        always {
            cleanWs() // 清理工作空间
        }
        success {
            echo 'All specified notebooks executed successfully.'
        }
        failure {
            echo 'Failed to execute the specified notebook.'
        }
    }
}
