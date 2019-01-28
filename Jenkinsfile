#!/usr/bin/env groovy

node {



    stage('Welcome')
    {
        echo 'You are welcome'
    }

    stage("build") {
        docker.withTool('docker') {
            docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS') {
//                docker.image('maven').inside {
//                docker.image('java').inside {
//                docker.image('mcr.microsoft.com/java/maven:8u192-zulu-debian9').inside {
                docker.image('openjdk:8-jdk').inside {
                    /* do things */

                    stage('checkout') {
                        git url: 'https://github.com/stonemct/JDI.git'
                    }

                    print "inside a node server\\docker\\"
//                    sh("echo test");
//                    //sh("npm install");
//                    sh 'ls -la ./Java/'
//                    sh 'cat test.txt' // we can access files from workspace
//                    sh 'echo "modified-inside-container" > test.txt' // we can modify files in workspace
//                    sh 'printenv' // jenkins is passing all envs variables into container
//                    sh 'mvn clean -f ./Java/pom.xml'

                    stage('maven build package')
                    {
                        withEnv(["MVN_PATH=${tool 'maven'}/bin"]) {
                        print "inside a withEnv block"
                        sh "cd ./Java/; ls -la; ${MVN_PATH}/mvn clean install"
                        sh "cd ./Java/; ls -la; ${MVN_PATH}/mvn clean package"
                        }
                    }
//                    withMaven(maven: 'm339') {
//                        println(MVN_CMD_DIR)
//                        // some block
//                        print "inside a node server\\docker\\withMaven"
//                        //                        sh 'mvn clean package'
//                        sh "ls -la ${MVN_CMD_DIR}"
//                        sh 'export PATH=$MVN_CMD_DIR:$PATH; export $MVN_CMD; export $MVN_CMD_DIR; echo $MVN_CMD_DIR; echo; echo $MVN_CMD; ls -la $MVN_CMD_DIR'
////                        sh '$MVN_CMD clean package'
//                        sh "export PATH=$MVN_CMD_DIR:$PATH && mvn clean package"
//                    }

                    stage('gathering the artifacts')
                            {
                                // Archive the build output artifacts.
                                archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
                            }
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