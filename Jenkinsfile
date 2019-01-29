#!/usr/bin/env groovy

node {



    stage('Welcome')
    {
        echo 'You are welcome'
    }

    stage("build") {
        docker.withTool('docker') {
            docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS') {
                docker.image('node').inside {
                print "inside a docker"
//                    sh 'mkdir -p cucumber-test-generator-ui'
                dir (path: 'cucumber-test-generator-ui2')
                        {
                            sh 'pwd'
                            stage('checkout cucumber-test-generator-ui'){
                                git url: 'https://github.com/TAI-EPAM/cucumber-test-generator-ui.git'
                            }
                            stage('npm build ')
                                    {
                                        withEnv(["NPM_PATH=${tool 'nodeJS'}/bin"]) {
                                            print "inside a withEnv block"
                                            sh "ls -la ${NPM_PATH}; ${NPM_PATH}/npm"
                                            sh "ls -la; ${NPM_PATH}/npm install"
                                            sh "ls -la; ${NPM_PATH}/npm run build"
                                        }
                                    }
//                    withTool('nodeJS'){
//                        sh 'npm install'
//                        sh 'npm run build'
//                    }
                        }
                }

                docker.image('openjdk:8-jdk').inside {


/*
                    stage('checkout jdi-cucumber-test-generator') {
                        git url: 'https://github.com/TAI-EPAM/jdi-cucumber-test-generator.git', tag: '1.0.0'
                    }


                    stage('maven build package')
                            {
                                withEnv(["MVN_PATH=${tool 'maven'}/bin"]) {
                                    print "inside a withEnv block"
                                    sh "ls -la; ${MVN_PATH}/mvn clean package -DskipTests=true"
                                }
                            }

                    stage('gathering the artifacts')
                            {
                                // Archive the build output artifacts.
                                archiveArtifacts artifacts: 'bdd-generator/target/bdd-generator-1.0.0*.jar', excludes: ''
                            }*/
                }
            }
        }
    }


    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }

}