pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    sh '''
                    echo "Hello You"
                    '''
                }
            }
            stage('Push Images'){
                steps{
                    sh '''
                    echo "Push Images"
                    '''
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                    sh '''
                    echo "Cleanup Jenkins"
                    '''
                }
            }
            stage('Deploy Containers'){
                steps{
                    sh '''
                    echo "Deploy Containers"
                    '''
                }
            }
        }
}
