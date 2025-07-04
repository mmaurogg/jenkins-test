pipeline{
    agent any

    enviroment{
        DOCKER_IMAGE = 'apimonedastt-image'
        CONTAINER_NAME = 'apimonedastt-docker'
        DOCKER_NETWORK = 'dockermonedas_red'
        DOCKER_BUILD_DIR = 'presentacion'
        HOST_PORT = '9080'
        CONTAINER_PORT = '8080'
    }

    stages{
        stage('Compilacion Maven'){
            steps{
                bat 'mvn clean package -Dskipteste'
            }
        }

        stage('Crear imagen docker'){
            steps{
                dir("${DOCKER_BUILD_DIR}"){
                    bat "docker build -t . ${DOCKER_IMAGE}"
                }
            }
        }
        
        stage('Desplegar contenedor'){
            steps{
                script{
                    bat "docker run --network ${DOCKER_NETWORK} --name  ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${CONTAINER_NAME}"
                }
            }
        }
    }
}