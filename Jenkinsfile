pipeline{

    agent{

        label 'aws-agent'

    }

    stages{

        stage('build'){

            steps{

                script{

                    sh 'docker build -t java-app .'

                }

            }

        }

        stage('push'){

            steps{

                script{

                    withCredentials([usernamePassword(credentialsId: 'Docker-Hub-up', passwordVariable: 'Password', usernameVariable: 'Username')]) {

                    sh 'docker login --username $Username --password $Password'

                    sh 'docker tag java-app $Username/java-app'

                    sh 'docker push $Username/java-app'

                    }

                }

            }

        }

        stage('deploy'){

            steps{

                script{

                    withAWS(credentials: 'AWS-key', region: 'eu-central-1') {

                    sh 'aws eks update-kubeconfig --region eu-central-1 --name eks'

                    sh 'kubectl apply -f ./k8s/deployment.yaml'

                    }

                }

            }

        }

    }

}