pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker.withServer('tcp://10.20.165.26:2375') {
                    image 'python:3-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker.withServer('tcp://10.20.165.26:2375') {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps {
                sh 'pyinstaller --onefile sources/add.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add'
                }
            }
        }
    }
}