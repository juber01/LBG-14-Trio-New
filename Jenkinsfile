pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    sh '''
                    docker build -t juber81/trio-flask-app:latest -t juber81/trio-flask-app:v${BUILD_NUMBER} ./flask-app
                    docker build -t juber81/trio-mynginx:latest -t juber81/trio-mynginx:v${BUILD_NUMBER} ./nginx
                    docker build -t juber81/trio-db:latest -t juber81/trio-db:v${BUILD_NUMBER} ./db
                    '''
                }
            }
            stage('Push Images'){
                steps{
                    sh '''
                    docker push juber81/trio-flask-app:latest
                    docker push juber81/trio-flask-app:v${BUILD_NUMBER}
                    docker push juber81/trio-mynginx:latest
                    docker push juber81/trio-mynginx:v${BUILD_NUMBER}
                    docker push juber81/trio-db:latest
                    docker push juber81/trio-db:v${BUILD_NUMBER}
                    '''
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                    sh '''
                    docker rmi juber81/trio-flask-app:latest
                    docker rmi juber81/trio-flask-app:v${BUILD_NUMBER}
                    docker rmi juber81/trio-mynginx:latest
                    docker rmi juber81/trio-mynginx:v${BUILD_NUMBER}
                    docker rmi juber81/trio-db:latest
                    docker rmi juber81/trio-db:v${BUILD_NUMBER}
                    '''
                }
            }
            stage('Deploy Containers'){
                steps{
                    sh '''
                    kubectl apply -f ./k8s -n production
                    kubectl rollout restart deployment flask-deployment -n production
                    '''
                }
            }
        }
}
