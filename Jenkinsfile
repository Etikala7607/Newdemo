pipeline {
    agent any

    environment {
        function_name = 'java-sample'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }
        stage('SonarQubee analysis'){
            agent any
            steps{
                withSonarQubeEnv('Sonar'){
                    sh'mvn sonar:sonar'
                }
            }
        }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://bermtecbatch31"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build'

                sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bermtecbatch31 --s3-key sample-1.0.3.jar"
            }
        }
    }
}
