pipeline {
    agent any
    options {
        skipDefaultCheckout()
    }
    parameters {
        string(name: 'THREADS', defaultValue: '2', description: '가상 사용자 수 (5 이하)')
        string(name: 'LOOPS', defaultValue: '5', description: '반복 횟수 (10 이하)')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Smoke Test') {
            steps {
                bat '%JMETER_HOME%\\bin\\jmeter.bat -n -f -t perf/smoke.jmx -l reports/smoke.jtl -e -o html-report -Jthreads=%THREADS% -Jloops=%LOOPS%'
            }
        }
        stage('Response Time Test') {
            steps {
                bat '%JMETER_HOME%\\bin\\jmeter.bat -n -f -t perf/duration.jmx -l reports/duration.jtl'
                perfReport sourceDataFiles: 'reports/duration.jtl', errorFailedThreshold: 5, errorUnstableThreshold: 1
              
                }
            
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'reports/, html-report/', allowEmptyArchive: true
        }
    }
}
