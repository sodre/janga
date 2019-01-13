pipeline {
    agent {
        label 'condaforge/linux-anvil-comp7'
    }
    stages {
        stage('create-env') {
            steps {
                sh """#!/bin/bash -il
                   conda env update -f environment.yml
                   """
            }
        }
        stage('build-docs') {
            steps {
                sh """#!/bin/bash -il
                   conda activate janga-env
                   make html
                   """
            }
        }
        stage('gh-pages') {
            environment {
                GH_TOKEN=credentials("GH_TOKEN")
            }
            steps {
                sh """#!/bin/bash -il
                   cd build/html/

                   git init
                   git config user.name "CurtLH"
                   git config user.email "CurtLHampton@gmail.com"

                   git add .
                   git commit -m "Deployed from Jenkins"
                   git push --force --quiet "https://${GH_TOKEN}@github.com/CurtLH/janga.git" master:gh-pages 
                   """
            }
        }
    }
}
