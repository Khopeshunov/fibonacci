pipeline {
    agent any
    parameters {
        choice(name: 'NUMBER',
            choices: [10,20,30,40,50,60,70,80,90],
            description: 'Select the value for F(n) for the Fibonacci sequence.')
    }
    options {
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '10'))
        timeout(time: 12, unit: 'HOURS')
        timestamps()
    }
    triggers {
        cron '@midnight'
    }
    stages {
        stage('Make executable') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'chmod +x ./scripts/fibonacci.sh'
                    } else {
                        echo 'Skipping chmod on Windows agent.'
                    }
                }
            }
        }
        stage('Run Fibonacci Script') {
            steps {
                script {
                    if (isUnix()) {
                        sh "./scripts/fibonacci.sh ${params.NUMBER}"
                    } else {
                        def windowsPath = env.WORKSPACE.replace('\\', '/')
                        def driveLetter = windowsPath[0].toLowerCase()
                        def pathWithoutDrive = windowsPath.substring(2) // skip 'C:'
                        def bashPath = "/${driveLetter}${pathWithoutDrive}"

                        // No escaping inside the path, just wrap it in quotes
                        bat "\"C:\\Program Files\\Git\\bin\\bash.exe\" \"${bashPath}/fibonacci.sh\" ${params.NUMBER}"
                    }
                }
            }
        }
    }
}
