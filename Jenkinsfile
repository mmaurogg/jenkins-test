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

        stage('Limpiar contenedor existente') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                        bat """
                        docker container inspect ${CONTAINER_NAME} >nul 2>&1 && (
                            docker container stop ${CONTAINER_NAME}
                            docker container rm ${CONTAINER_NAME}
                        ) || echo "No existe el contenedor '${CONTAINER_NAME}'."
                        """
                    }
                }
            }
        }

        stage('Crear imagen docker'){
            steps{
               // dir("${DOCKER_BUILD_DIR}"){
                    bat "docker build -t . ${DOCKER_IMAGE}"
                //}
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