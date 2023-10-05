pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    script {
                        if (env.GIT_BRANCH == 'origin/dev') {
                            sh '''
                            docker build -t juber81/trio-flask-app:latest -t juber81/trio-flask-app:v${BUILD_NUMBER} ./flask-app
                            docker build -t juber81/trio-mynginx:latest -t juber81/trio-mynginx:v${BUILD_NUMBER} ./nginx
                            docker build -t juber81/trio-db:latest -t juber81/trio-db:v${BUILD_NUMBER} ./db
                            '''
                        } else {
                            sh '''
                            echo 'Build not required!'
                            '''
                        }
                    }

                }
            }
            stage('Push Images'){
                steps{
                    script {
                        if (env.GIT_BRANCH == 'origin/dev') {
                            sh '''
                            docker push juber81/trio-flask-app:latest
                            docker push juber81/trio-flask-app:v${BUILD_NUMBER}
                            docker push juber81/trio-mynginx:latest
                            docker push juber81/trio-mynginx:v${BUILD_NUMBER}
                            docker push juber81/trio-db:latest
                            docker push juber81/trio-db:v${BUILD_NUMBER}
                            '''
                        } else {
                            sh '''
                             echo 'Push not required!'
                             '''
                        }
                    }
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                     script {
                        if (env.GIT_BRANCH == 'origin/dev') {
                            sh '''
                            docker rmi juber81/trio-flask-app:latest
                            docker rmi juber81/trio-flask-app:v${BUILD_NUMBER}
                            docker rmi juber81/trio-mynginx:latest
                            docker rmi juber81/trio-mynginx:v${BUILD_NUMBER}
                            docker rmi juber81/trio-db:latest
                            docker rmi juber81/trio-db:v${BUILD_NUMBER}
                            '''
                        } else {
                            sh '''
                             echo 'No Clean up required!'
                             '''
                        }
                    }
                }
            }
            stage('Deploy Containers'){
                steps{
                    script {
                        if (env.GIT_BRANCH == 'origin/dev') {
                            sh '''
                            kubectl apply -f ./k8s -n development
                            kubectl rollout restart deployment flask-deployment -n production
                            '''
                        } else if (env.GIT_BRANCH == 'origin/main') {
                            sh '''
                            kubectl apply -f ./k8s -n production
                            kubectl rollout restart deployment flask-deployment -n production
                            '''
                        }
                    }
                }
            }
        }
}