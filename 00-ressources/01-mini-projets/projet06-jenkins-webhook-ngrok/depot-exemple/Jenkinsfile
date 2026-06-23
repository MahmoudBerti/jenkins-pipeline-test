pipeline {
    agent any

    // Declencheur : a chaque "git push", GitHub envoie un webhook a Jenkins,
    // qui lance ce pipeline automatiquement (pas de "Build Now" manuel).
    triggers {
        githubPush()
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 15, unit: 'MINUTES')
    }

    stages {

        stage('Checkout') {
            steps {
                // Recupere le code de la branche qui a declenche le build.
                checkout scm
            }
        }

        stage('Build Java') {
            steps {
                sh 'javac HelloWorld.java'
                sh 'java HelloWorld'
            }
        }

        stage('Run Python') {
            steps {
                sh 'python3 hello.py'
            }
        }
    }

    post {
        success { echo 'Pipeline declenche par le webhook : SUCCESS.' }
        failure { echo 'Echec : consultez les logs de l etape en rouge.' }
    }
}
