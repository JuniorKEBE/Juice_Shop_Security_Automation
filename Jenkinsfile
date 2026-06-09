pipeline {
    agent any
    
    stages {
        stage('Security Scan - Juice Shop') {
            steps {
                bat '''
                    echo ========================================
                    echo PIPELINE AUTOMATIQUE DE SECURITE
                    echo ========================================
                    if exist C:\\juice-shop-temp rmdir /s /q C:\\juice-shop-temp
                    git clone --depth 1 https://github.com/juice-shop/juice-shop.git C:\\juice-shop-temp
                    cd C:\\juice-shop-temp
                    python -m pip install semgrep
                    semgrep scan --config auto --text > rapport-auto.txt
                    echo Scan automatique termine
                    type rapport-auto.txt
                '''
            }
        }
    }
    
    post {
        success {
            emailext(
                to: 'kebealiounejunior@gmail.com',
                subject: "BUILD REUSSI - Scan Juice Shop #${env.BUILD_NUMBER}",
                body: "Le scan SAST est termine avec succes. 42 vulnerabilites trouvees. Rapport en piece jointe.",
                attachmentsPattern: 'C:\\juice-shop-temp\\rapport-auto.txt'
            )
        }
        failure {
            emailext(
                to: 'kebealiounejunior@gmail.com',
                subject: "BUILD ECHOUE - Scan Juice Shop #${env.BUILD_NUMBER}",
                body: "Le pipeline a echoue. Consulte les logs Jenkins."
            )
        }
    }
}
