def image

pipeline {
    agent any

    stages {
    
        stage("Print Build Information") {
            steps {
                sh "echo 'Build Number:'${BUILD_NUMBER}  'GIT Commit:'${GIT_COMMIT}"
            }
        }
    
        stage('Token Replacement') {
             steps {
                 sh 'echo "Replacing build details in html and deplyment files"'
                 sh '''
                     sed -i -e "s/@BuildNumber@/${BUILD_NUMBER}/; s/@GIT_COMMIT@/${GIT_COMMIT}/;" application/templates/about.html
					 sed -i -e "s/@BuildNumber@/${BUILD_NUMBER}/;" k8s-deployment/deployment.yaml
					 cat application/templates/about.html
					 cat k8s-deployment/deployment.yaml
                 '''
             }
        }
        
        stage('Lint Dockerfile') {
            steps {
                script {
                    docker.image('hadolint/hadolint:latest-debian').inside() {
                            sh 'hadolint ./application/Dockerfile | tee -a hadolint.txt'
                            sh '''
                                lintErrors=$(stat --printf="%s"  hadolint.txt)
                                if [ "$lintErrors" -gt "0" ]; then
                                    echo "Errors linting Dockerfile"
                                    cat hadolint.txt
                                    exit 1
                                else
                                    echo "Done linting Dockerfile"
                                fi
                            '''
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    image = docker.build("shijujohn1093/capstoneudacity", "-f application/Dockerfile application")
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        image.push("${env.BUILD_NUMBER}")
                        image.push("latest")
                    }
                }
            }
        }

      stage('Apply deployment') {
            steps {
                    sh "kubectl --v=6 apply -f k8s-deployment/deployment.yaml"
                    sh "kubectl --v=6 apply -f k8s-deployment/service.yaml"
            }
        }
         
        stage('Post deployment test') {
            steps {
                    sh '''
                        HOST=$(kubectl get service capstoneudacity | grep 'amazonaws.com' | awk '{print $4}')
                        curl $HOST -f
                    '''
            }
        }

        stage("Prune docker") {
            steps {
                sh "docker system prune -f"
            }
        }
        
    }
}
