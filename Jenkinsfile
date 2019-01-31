#!/usr/bin/env groovy

node {
    
//    stage("npm build") {
//        stage('Welcome')
//        {
//            echo 'You are welcome'
//        }
//        dir(path: 'cucumber-test-generator-ui-new')
//        {
//            stage('git checkout'){
//                sh 'pwd'
//                git url: 'https://github.com/TAI-EPAM/cucumber-test-generator-ui.git'
//            }
//
//            docker.withTool('docker') {
//                docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS')
//                {
//                    docker.image('node').inside {
//                        print "inside a docker"
//                        sh "ls -la; npm install"
//                        sh "ls -la; npm run build"
//
//                    }
//                }
//            }
//            stage('archive Artifacts and stash result'){
//                // Archive the build output artifacts.
//                archiveArtifacts artifacts: 'dist/*', excludes: ''
//                stash name: 'npmstash', includes: 'dist/*'
//            }
//        }
//    }
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
            stage('checkout jdi-cucumber-test-generator') {
                sh 'pwd'
                git url: 'https://github.com/TAI-EPAM/jdi-cucumber-test-generator.git', tag: '1.0.0'
//                unstash name: 'npmstash'
                sh "ls -la"
            }

            docker.withTool('docker')
            {
                docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS')
                {
//                    docker.image('openjdk:8-jdk').withRun('-it -p 8081:8080 bash') { c ->
//                    docker.image('openjdk:8-jdk').withRun('-it -p 999:8080') {
                    docker.image('ubuntu:16.04').withRun('-it -p 999:8080') { c ->
//                    docker.image('openjdk:8-jdk').withRun('-it -p 8081:8080').inside {
//                        docker.image('openjdk:8-jdk').inside
                        docker.image('ubuntu:16.04').inside
                            {
                                //sh 'while true ; do sleep 1; done'
                                sh "${tool 'docker'}/bin/docker ps -a; sleep 10"
                                sh "uname -a"
                                
                                stage('maven build package')
                                    {
//                                        withEnv(["MVN_PATH=${tool 'maven'}/bin"]) {
//                                        withEnv(["JAVA_HOME=${ tool 'JDK' }", "PATH+MAVEN=${tool 'maven'}/bin:${env.JAVA_HOME}/bin"]) {
                                        withEnv(["JAVA_HOME=${ tool 'JDK' }", "MVN_PATH=${tool 'maven'}/bin", "PATH=${PATH}:${tool 'maven'}/bin:${env.JAVA_HOME}/bin"]) {
                                            print "inside a withEnv block"
                                            println(PATH)
                                            println(MVN_PATH)
                                            println(JAVA_HOME)
                                            
                                            sh "pwd; echo $PATH; which ls; ls -la ${MVN_PATH}"
                                            sh "mvn clean package -DskipTests=true"
//                                            sh "${MVN_PATH}/mvn clean package -DskipTests=true"
//                                            sh "${MVN_PATH}/mvn spring-boot:run -f bdd-generator"
                                            sh "curl -I localhost:8080"
                                            sh "java -jar bdd-generator/target/bdd-generator-1.0.0-exec.jar"
                                            sh "which java"
//                                            sh "which mvn"
                                        }
                                    }
//                                stage('gathering the artifacts')
//                                    {
//                                        // Archive the build output artifacts.
//                                        archiveArtifacts artifacts: 'bdd-generator/target/bdd-generator-1.0.0*.jar', excludes: ''
//                                    }
                            }
                    }
    
                }
            }
        }
    }
    
}// end of