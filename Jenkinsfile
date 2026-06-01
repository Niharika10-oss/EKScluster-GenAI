pipeline {
    agent any

    environment {
        // This pulls your Access Key and Secret Key from the 'aws-creds' store
        AWS_CREDS = credentials('aws-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                // No 'dir' block needed because files are in the root
                sh 'terraform init -upgrade -no-color'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan -no-color'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: "Review the plan. Do you want to proceed?", ok: "Apply Changes"
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve > error.log 2>&1'
            }
        }
    }

    post {
        always {
            sh 'rm -f tfplan'
        }
        success {
            echo "SUCCESS: Infrastructure is live!"
        }
        failure {
            echo "FAILURE: Check the logs above."
            // --- NEW GENAI SELF-HEALING BLOCK ---
            echo "Invoking Gemini AI Cloud Diagnoser..."
            withEnv(["GEMINI_API_KEY=${env.GEMINI_API_KEY}"]) {
                // 2. Execute the script: Tell Python to analyze the console log text.
                // Note: Jenkins automatically tracks console data, but we pass your log file or pipe the log tail directly.
                sh 'python cloud_diagnoser.py error.log || echo "[AI-Warning] Diagnoser script execution skipped or failed."'
            }
        }
    }
}
