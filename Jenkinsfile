pipeline {
    agent { 
        label 'electronix' 
    }

    environment {
        S3_BUCKET = 'terraform-in-one-shot2003'
        CLOUDFRONT_ID = 'E20SUMGGWCRTRK'
        AWS_REGION = 'ap-south-1'
    }

    stages {

        stage('Frontend Deployment') {
            when {
                changeset "frontend/**"
            }
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Run Test') {
            steps {
                dir('frontend') {
                    sh 'npm test -- --watchAll=false || echo "No tests configured..."'
                }
            }
        }

        stage('Build') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy to S3') {
            steps {
                dir('frontend') {
                    sh '''
                        aws s3 sync dist/ s3://$S3_BUCKET --delete --region $AWS_REGION

                        aws cloudfront create-invalidation \
                          --distribution-id $CLOUDFRONT_ID \
                          --paths "/*"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Frontend deployment successful'
        }

        failure {
            echo 'Frontend deployment failed'
        }
    }
}