pipeline {
    agent any

    environment {
        PYTHON_PATH = 'E:\\python\\Scripts\\python.exe '
    }

    stages {
        stage('Verify Python Path') {
            steps {
                bat """
                    @echo off
                    echo "=== 验证Python313路径及版本 ==="
                    ${PYTHON_PATH} --version  // 仅保留此关键验证（成功即证明路径正确）
                    echo "Python路径验证通过！"
                """
            }
        }
        stage('Checkout') {
            steps {
                git url: 'https://ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCY8k79L1trY2fDhGBZ0blaSzKpY+NAJtgcSS0V88UVNLdtEOGNvZXjB0Lc/et0WLi4ixcvWq/VHvm6TlAkzNwHtOzeMS62Im5oE6JtrXXhbgN+icAlyk/oTgYWhYsr6kQO/uePxaai5jNzoWxHFRu9GLPzKkBO3DTXtuA7OX7Fpqx7+L/ex5EH+gHIOjSkj3YYrbMGPMbZ0MtPzwV9ShJJTQJNppe57JwUNTeAl6o5LBNpxlaiytpUgDAiFOKkLH5T1W9RGtGrNOzC6xoll4oUexfZkUDNB4nsJbpLzHoHNmBL3YwqKMiS5YLxk3A3xrxq0mPMIjn5/Fw6yGLirGcBETaduVZR37y8DzkcdclkgwVIYyYzEJM+PPVLR8hjKhLU7XJDg8cGsK9Md2jfiwFDdHT6uhyic216Xo6l7kEpfkCmdQWqq2q0beIx1snGrJjdkMHVOKvzD1hclK9MsNUHiTL5lfSaQHSlkpEwaUrgH6gT9x6Xn3Y2dZ2Ft7ttH9wgQPK2R0XhC+qOrZxSQlIODYlOMIqBNoQHqqDMX8VcGdrp3QvR99hdTx59T5VNFBze7nLuXmeEfdc/yPQFTvV7bu1+jirfKLXg7kbJrihrZyDNBH//cx2o4opP3M0DKo96cOI6iMWpe+OsDoETb5fz12L7q6UHnzi3al0Is+fLJw== 华硕@LAPTOP-999746UQ@github.com/super-sausage/myflaskapp.git', branch: 'main'
            }
        }
        stage('Fix pip') {
            steps {
                bat """
                    @echo off
                    echo "=== 修复Python313的pip环境 ==="
                    ${PYTHON_PATH} -m ensurepip --upgrade
                    ${PYTHON_PATH} -m pip --version
                """
            }
        }
        stage('Install Dependencies') {
            steps {
                bat """
                    @echo off
                    ${PYTHON_PATH} -m pip install --upgrade pip
                    ${PYTHON_PATH} -m pip install -r requirements.txt
                """
            }
        }
        stage('Lint') {
            steps {
                bat "${PYTHON_PATH} -m pip install flake8 && ${PYTHON_PATH} -m flake8 app.py tests/"
            }
        }
        stage('Test') {
            steps {
                bat """
                    ${PYTHON_PATH} -m pip install pytest
                    ${PYTHON_PATH} -m pytest --cov=app tests/ --cov-report=html
                """
            }
            post {
                always {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'htmlcov',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        stage('Build') {
            steps {
                bat """
                    ${PYTHON_PATH} -m pip install pyinstaller
                    ${PYTHON_PATH} -m PyInstaller --onefile app.py
                """
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat "start ${PYTHON_PATH} app.py"  // 启动Flask应用（按需调整）
            }
        }
    }
    post {
        success { echo 'CI/CD pipeline completed successfully!' }
        failure { echo 'CI/CD pipeline failed!' }
    }
}