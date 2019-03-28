node{
  checkout scm

          def DOCKER_LOGIN     = credentials('docker-login')
          def DOCKER_PASSWORD = credentials('docker-password')
          def GOPATH = "/go/src/github.com/thiagotrennepohl/fortune-backend"

            stage('Unit Test') {
                    

                    sh """
                        docker run -d --rm --name=test-mongo --network=host
                       """
                
                   sh """ 
                       docker run --rm -v \${PWD}:${GOPATH}  -w ${GOPATH} \
                       -e GO111MODULE=on --network=host \
                       golang apk add --update git && go test -cover ./...
                     """
                     
                   sh """
                       docker rm -f test-mongo || true 
                      """
                
            }
            stage('Docker build') {
                
                sh """
                    docker login -u \${DOCKER_LOGIN} -p \${DOCKER_PASSWORD}
                    docker build -t . thiagotr/fortune-backend .
                    docker push thiagotr/fortune-backend
                    """
                
            }
        }