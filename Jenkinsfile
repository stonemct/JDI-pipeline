#!/usr/bin/env groovy

node {

    stage('WelcomeToHell')
    {
        echo 'Hello World'
    }


    stage('checkout') {
        git url: 'https://github.com/stonemct/JDI.git'
    }


    stage("build") {
        writeFile file: "test.txt", text: "test"
        sh 'cat test.txt' // will be "modified-inside-container" here
        docker.withTool('docker') {
            docker.withServer('tcp://192.168.99.100:2376', 'dockerTLS') {
//                docker.image('maven').inside {
                docker.image('java').inside {
                    /* do things */

                    print "inside a node server"
//                    sh("echo test");
//                    //sh("npm install");
//                    sh 'ls -la ./Java/'
//                    sh 'cat test.txt' // we can access files from workspace
//                    sh 'echo "modified-inside-container" > test.txt' // we can modify files in workspace
//                    sh 'printenv' // jenkins is passing all envs variables into container
//                    sh 'mvn clean -f ./Java/pom.xml'
                    withMaven(jdk: 'JDK', maven: 'maven') {
                        // some block
                        print "inside a node server\\docker\\withMaven"
//                        sh 'mvn clean package'
                        sh '$MVN_CMD clean deploy'
                    }


                }
            }
            sh 'cat test.txt' // will be "modified-inside-container" here
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