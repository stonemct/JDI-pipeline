#!/usr/bin/env groovy

node {

    stage('Welcome')
    {
        echo 'You are welcome'
    }

    stage("npm build") {
        dir(path: 'cucumber-test-generator-ui-new')
                {
                    steps('checkout cucumber-test-generator-ui') {
                        sh 'pwd'
                        git url: 'https://github.com/TAI-EPAM/cucumber-test-generator-ui.git'
                    }

                    docker.withTool('docker') {
                        docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS')
                                {
                                    docker.image('node').inside {
                                        print "inside a docker"
                                        sh "ls -la; npm install"
                                        sh "ls -la; npm run build"

                                    }
                                }
                    }
                    steps('gathering the artifacts')
                            {
                                // Archive the build output artifacts.
                                archiveArtifacts artifacts: 'dist/*', excludes: ''
                                stash name: 'npmstash', includes: 'dist/*'
                            }
                }
    }
//                                        withEnv(["NPM_PATH=${tool 'nodeJS'}/bin"]) {
//                                            print "inside a withEnv block"
//                                            sh "ls -la ${NPM_PATH}; ${NPM_PATH}/npm"
//                                            sh "ls -la; ${NPM_PATH}/npm install"
//                                            sh "ls -la; ${NPM_PATH}/npm run build"
//                                        }

//                    withTool('nodeJS'){
//                        sh 'npm install'
//                        sh 'npm run build'
//                    }
    
    stage("maven build") {
        dir(path: 'cucumber-test-generator')
            {
    
                steps('checkout jdi-cucumber-test-generator') {
                    sh 'pwd'
                    git url: 'https://github.com/TAI-EPAM/jdi-cucumber-test-generator.git', tag: '1.0.0'
                    unstash name: 'npmstash'
                    sh "ls -la"
                }
    
                docker.withTool('docker')
                    {
                        docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS')
                            {
                                docker.image('openjdk:8-jdk').inside
                                    {
    
    
                                        stage('maven build package')
                                            {
                                                withEnv(["MVN_PATH=${tool 'maven'}/bin"]) {
                                                    print "inside a withEnv block"
                                                    sh "ls -la; ${MVN_PATH}/mvn clean package -DskipTests=true"
                                                }
                                            }
                                        steps('gathering the artifacts')
                                            {
                                                // Archive the build output artifacts.
                                                archiveArtifacts artifacts: 'bdd-generator/target/bdd-generator-1.0.0*.jar', excludes: ''
                                            }
                                    }
                            }
                    }
            }
    }
    
}// end of