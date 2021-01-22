node {
    def commit_id
    stage('Prepare') {
        checkout scm
        h "git rev-parse --short HEAD > .git/commit-id"
        commit_id = readFile('.git/commit-id').trim()
    }
    stage('Build') {
        docker.withServer('tcp://10.20.165.26:2375'){
            def pythoni = docker.image('python:2-alpine')
            pythoni.inside {
                sh 'python -m py_compile sources/add.py sources/calc.py'
            }
        }
    }
    stage('Test') {
        docker.withServer('tcp://10.20.165.26:2375'){
            def pytesti = docker.image('qnib/pytest')
            pytesti.inside {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        }
    }
    stage('Deliver') {
        docker.withServer('tcp://10.20.165.26:2375'){
            def pyinstaller = docker.image('cdrx/pyinstaller-linux:python2')
            pyinstaller.inside {
                sh 'pyinstaller --onefile sources/add.py'
            }
        }
    }
}