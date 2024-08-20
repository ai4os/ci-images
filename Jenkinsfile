// vim: filetype=groovy


pipeline {
    agent {
        label 'docker'
    }

    stages {
        stage("Docker Variable initialization") {
            environment {
                AI4OS_REGISTRY_CREDENTIALS = credentials('AIOS-registry-credentials')
                }
            steps {
                script {
                    withFolderProperties{
                        docker_registry = env.AI4OS_REGISTRY
                    }
                    docker_ids = []
                    
                    docker_registry_credentials = env.AI4OS_REGISTRY_CREDENTIALS
                    
                }
            }
        }

        stage('Build docker images') {
            steps {
                // Iterate over the directories and subdirectories. Join directory and subdirectory together to create the image names
                script {
                    dirs = sh (script: 'find * -name Dockerfile -printf "%h\n" | sort | uniq', 
                               returnStdout: true).split("\n")
                    docker.withRegistry(docker_registry, docker_registry_credentials) {
                       for (dir in dirs) {
                            def image = dir.replace("/", "")
                            image = "ai4os-hub/ci-images:${image}"
                            def dockerfile = "${dir}/Dockerfile"
    
                            echo "Building image: ${image}"
    
                            image_id = docker.build(image, "--no-cache --force-rm -f ${dockerfile} ${dir}")
                            docker_ids.add(image)
                        }
                    }
                }
            }
        }
       stage('AI4OS Hub Docker delivery to registry') {
            when {
                expression {
                    docker_ids.size() > 0
                }

                expression {
                    env.BRANCH_NAME == 'master'
                }
            }
            steps {
                script {
                    docker.withRegistry(docker_registry, docker_registry_credentials) {
                        docker_ids.each {
                            println "Pushing image: ${it}"
                            docker.image(it).push()
                        }
                    }
                }
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }

}
