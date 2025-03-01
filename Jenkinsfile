node {
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

    stage('Manual Approval') {
        script {
            def userInput = input(
                message: 'Lanjutkan ke tahap Deploy?',
                ok: 'Proceed',       // Tombol hijau
                submitterParameter: 'approval', // Hasil input
                parameters: []       // Kosong biar cuma tombol saja
            )

            if (userInput == 'Abort') {
                echo 'Pipeline dihentikan oleh pengguna.'
                currentBuild.result = 'ABORTED' // Pipeline diabort tanpa error
                return // Keluar dari pipeline secara normal
            }
        }

        echo 'User melanjutkan pipeline dengan tombol Proceed'
    }

    stage('Deploy') {
        echo 'Melanjutkan ke tahap Deploy...'
    }
}


/*}

    stage('Manual Approval') {
        input {
            message 'Lanjutkan ke tahap Deploy?'
            ok 'Proceed'
            parameters {
                choice(name: 'Approval', choices: ['Proceed', 'Abort'], description: 'Pilih apakah ingin melanjutkan atau menghentikan pipeline.')
            }
        }
    }

    stage('Deploy') {
        when {
            expression { params.Approval == 'Proceed' }
        }
        steps {
            echo 'Melanjutkan ke tahap Deploy...'
        }
    }
}*/
