pipeline{
    stages{
        stage('Checkout'){
            steps{
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage{}
    }
    
}

