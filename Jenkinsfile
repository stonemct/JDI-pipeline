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
        sh 'cat test.txt' // will be "modified-inside-container" here
        writeFile file: "test.txt", text: "test"

        docker.withServer('tcp://192.168.99.100:2375') {
            docker.image('maven').withRun('') {
                /* do things */
                sh 'ls -la'
                sh 'cat /mounted' // we can mount any file from host
                sh 'cat test.txt' // we can access files from workspace
                sh 'echo "modified-inside-container" > test.txt' // we can modify files in workspace
                sh 'printenv' // jenkins is passing all envs variables into container
            }

        sh 'cat test.txt' // will be "modified-inside-container" here
    }


    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }

}