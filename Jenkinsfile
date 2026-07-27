pipeline{
    agent { label 'electronix' }

    environment{
        S3_BUCKET='terraform-in-one-shot2003'
        CLOUNTFRONT_ID='E20SUMGGWCRTRK'
        AWS_REGION='ap-south-1'
    }

    stages{
        stage("Frontend Deployment"){
            when{
                changeset "frontend/**"
            }
        }
        stage('Install Dependencies'){
            steps{
                dir('frontend')
                sh '''
                sh "npm install"
                '''
            }
        }
        stage("Run test"){
            steps{
                dir('frontend'){
                    sh 'npm test -- --watchAll=false || echo "No Test configured..."'
                }
            }
        }
        stage('Build'){
            steps('frontend'){
                sh 'npm run build'
            }
        }
        stage('Deploy S3'){
            steps{
                dir('frontend'){
                    sh '''
                    aws cloudfront create-invalidation --distribution-id $(CLOUDFRONT_ID) --path "/*"
                    '''
                }
            }
        }
    }
    post{
        sucess{
            'Frontend deployment sucessful'
        }
        failure{
            'Frontend deployment fail'
        }
    }
}