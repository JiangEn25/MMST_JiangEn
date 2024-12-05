pipeline {
    agent any // 指定在这个 pipeline 中使用的节点（agent），"any" 表示任意可用的节点

    stages { // 定义不同的构建阶段
        stage('Checkout') {
            steps {
                // 这一步通常由 Jenkins 自动处理，因为它是从源码仓库检出代码的地方
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install' // 假设这是一个 Maven 项目，这里执行构建命令
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test' // 执行单元测试
            }
        }
    }

    post { // 构建后的操作
        always {
            cleanWs() // 清理工作空间
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}