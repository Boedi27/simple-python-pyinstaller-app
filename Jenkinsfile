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
        sh 'echo "Aplikasi berjalan selama 1 menit..."'
        sleep(time: 1, unit: 'MINUTES') // Menjeda eksekusi selama 1 menit
        echo 'Aplikasi selesai berjalan setelah 1 menit.'
    }
}


/*
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
*/

/*}

    stage('Manual Approval') {
        script {
            def userInput = input(message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', 
                parameters: [choice(name: 'Approval', choices: ['Proceed', 'Abort'], 
                description: 'Pilih apakah ingin melanjutkan atau menghentikan pipeline.')])
            if (userInput == 'Abort') {
                error('Pipeline dihentikan oleh pengguna.')
            }
        }
    }

    stage('Deploy') {
        echo 'Melanjutkan ke tahap Deploy...'
    }
}*/
