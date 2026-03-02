pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo '开始构建...'
                sh 'echo "构建时间: $(date)" > build-info.txt'
            }
        }
        
        stage('Test') {
            steps {
                echo '运行测试...'
                sh 'echo "测试通过!"'
                // 这里可以替换为真实的测试命令，如：npm test
            }
        }
        
        stage('Deploy') {
            steps {
                echo '部署到生产环境...'
                sh 'cp index.html index-deployed.html'
                sh 'echo "部署完成于: $(date)" >> index-deployed.html'
                sh 'cat index-deployed.html'
            }
        }
    }
    
    post {
        always {
            echo '流水线执行完毕!'
        }
        success {
            echo '构建成功!'
        }
        failure {
            echo '构建失败!'
        }
    }
}
