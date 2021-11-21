pipeline {
    agent any
    stages {
        stage('CHECKOUT') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '$BRANCH_NAME']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/krzysztof-palubicki/aplikacja_kalkulator.git/']]])
                }
            }
        } //end of stage CHECKOUT
      
        stage('MAIN TEST') {
            parallel {
                stage ('Test funk 1') {
                agent { label 'moj_node'}
                    steps {
                        script {
                            sh "echo ${env.NODE_NAME}"
                            sh 'python3.8 -m pytest test_*.py --html=report.html'
                                        }
                }
            }
                stage ('Test funk 2') {
                agent { label 'master'}
                     steps {
                        script { 
                            sh "echo ${env.NODE_NAME}"
                            sh 'python3.8 -m pytest test_*.py --html=report.html'
                                            }
                }
            }
            stage ('Test funk 3') {
                steps {
                    script {
                        sh 'python3.8 -m pytest test_*.py --html=report.html'
                                            }
                }
            }
            } //end of parallel
        } //end of stage MAIN TEST
        
          stage('CLEANUP') {
            steps {
                script {
                   archiveArtifacts artifacts: 'report.html, assets/', followSymlinks: false
                   deleteDir()
                }
            }
        } //end of stage CLEANUP
        
} //end of stages
} //end of pipeline
