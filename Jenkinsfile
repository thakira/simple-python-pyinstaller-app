pipeline {
    agent none 
    stages {
        stage('Build') { 
            agent {
                any {
                    image 'python:2-alpine' 
                }
            }
            steps {
                sh 'python3 -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'zmarffy/pybuilder'
                }
            }
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }		
    }
}
