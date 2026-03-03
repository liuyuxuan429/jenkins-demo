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
                // 模拟测试执行和覆盖率统计
                sh 'echo "Tests run: 10, Failures: 0" > test-result.txt'
                sh 'echo "Coverage: 85%" > coverage.txt'  // 先演示失败场景
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    // 读取测试通过率
                    def testResult = sh(
                        script: "cat test-result.txt",
                        returnStdout: true
                    ).trim()
                    
                    // 读取覆盖率
                    def coverageStr = sh(
                        script: "cat coverage.txt | grep -o '[0-9]\\+' | head -1",
                        returnStdout: true
                    ).trim()
                    def coverage = coverageStr as Integer
                    
                    echo "========== 质量门禁检查 =========="
                    echo "测试情况: ${testResult}"
                    echo "代码覆盖率: ${coverage}%"
                    echo "质量门禁阈值: 覆盖率≥80%，测试通过率100%"
                    
                    // 规则1：检查测试通过率（简单字符串匹配）
                    if (testResult.contains("Failures: 0")) {
                        echo "✅ 测试通过率检查通过"
                    } else {
                        error("❌ 质量门禁阻断！存在测试失败，禁止部署！")
                    }
                    
                    // 规则2：检查覆盖率（关键！）
                    if (coverage >= 80) {
                        echo "✅ 覆盖率检查通过 (${coverage}% ≥ 80%)"
                    } else {
                        error("❌ 质量门禁阻断！覆盖率 ${coverage}% 低于阈值 80%，禁止进入部署阶段！")
                    }
                    
                    echo "========== 质量门禁通过，允许部署 =========="
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo '✅ 部署到生产环境...'
                sh 'cp index.html index-deployed.html'
                sh 'echo "部署完成时间: $(date)" >> index-deployed.html'
            }
        }
    }
    
    post {
        failure {
            echo '🚫 流水线因质量门禁未通过而失败！请修复代码质量后重试。'
        }
        success {
            echo '🎉 流水线执行成功！代码质量符合标准，已部署上线。'
        }
    }
}
