pipeline {
  agent any
  stages {
    stage('Package index.html') {
      steps {
        sh 'zip -r index.zip chandni/index.html'
      }
    }
    stage('Upload to S3') {
      steps {
        sh 'aws s3 cp index.zip s3://chandni-bucket/chandni/'
      }
    }
    
    stage('Deploy to Nginx EC2') {
      steps {
        sh '''
        ssh -i /var/lib/jenkins/.ssh/chandni.pem ubuntu@3.83.206.4
          rm -rf chandni/ index.zip
          aws s3 cp s3://chandni-bucket/chandni/index.zip .
          unzip -o index.zip
          sudo -S mkdir -p /var/www/html/chandni/
          sudo -S cp chandni/index.html /var/www/html/chandni/
        '''
      }
    }
  }
}
