/*node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        step([$class: 'JUnitResultArchiver', testResults: 'test-reports/results.xml'])
    }
}*/

node {
    stage('Build') {
        sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
    }

    stage('Test') {
        try {
            sh 'python3 -m pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        } finally {
            junit 'test-reports/results.xml'
        }
    }

    stage('Manual Approval') {
        def userInput = input message: 'Do you want to proceed to the deployment stage?'
    }

    stage('Deploy') {
        sh 'pyinstaller --onefile sources/add2vals.py'
        sh './dist/add2vals &'
        sleep 60

        archiveArtifacts 'dist/add2vals'
    }
}
