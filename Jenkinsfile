pipeline {
    agent {label 'docker-slave'}
    options {        
        skipStagesAfterUnstable()
    }
    
    stages {                
        stage("Define Release Version") {
            steps {
                script {                
                    // Prepare  Available releases and write to a file                    
                    sh "git ls-tree --name-only HEAD | grep -Eo '([0-9]+\\.?)+' > ${WORKSPACE}/mayor-versions-list"
                                        
                    // Load the list into a variable
                    env.MAYOR_RELEASES_LIST = readFile (file: "${WORKSPACE}/mayor-versions-list")
                    
                    // Show the select input
                    env.RELEASE_SCOPE = input message: 'Please Select the Mayor Release to verify', ok: 'Build',
                    parameters: [choice(name: 'Mayor Release', choices: env.MAYOR_RELEASES_LIST, description: 'Please select the Mayor Release that you want to verify')]
                }
                
                echo "Mayor release selected: ${env.RELEASE_SCOPE}"
            }
        }
        stage('Validate App') {
        steps{
           script {               
               echo "Validating Release: ${env.RELEASE_SCOPE}"                                                                                    
               echo 'Bringing Application Up. This is the first step!'
               sh "docker-compose -f ${WORKSPACE}/${env.RELEASE_SCOPE}/docker-compose.yml up -d"
                
                  echo 'Validating App is Up!' 
               
                  sh """docker-compose -f ${WORKSPACE}/${env.RELEASE_SCOPE}/docker-compose.yml ps -q | xargs docker inspect -f \'{{ .State.ExitCode }}\' | while read code; do
                     if [ "\$code" != "0" ]; 
                         then exit -1 
                     fi 
                     done
                     """                                   
            }
        }
        }
        stage('Run Platform Specific Tests') {
            parallel {
                stage('Windows Testing') {
                    agent {
                        label "windows-testing"
                    }
                    steps {                            
                       echo "Executing script to start Testing in Windows"
                    }
                    post {
                        always {
                             echo "Save Windows Testing Result"
                        }
                    }
                }
                stage('Linux Testing') {
                    agent {
                        label "linux-testing"
                    }
                    steps {                       
                       echo "Executing script to start Testing in Linux"
                     }
                    post {
                        always {
                             echo "Save Linux Testing Result"
                        }
                    }
                }
            }
        }
    }
    post { 
        always { 
                sh "docker-compose -f ${WORKSPACE}/${env.RELEASE_SCOPE}/docker-compose.yml down"                
        }
    }
}
