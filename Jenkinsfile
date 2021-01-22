pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image: python2-alpine
                }
            }
            steps {
                sh 'python -m py_compile sources/add.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker{
                    imgae 'qnib/pytest'
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
                sh 'pyinstaller --onefile source/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}