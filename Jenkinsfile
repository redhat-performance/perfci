pipeline {
    agent { label 'perfci' }
    stages {
        stage('setup jetpack') {
            steps {
                sh 'git clone https://github.com/redhat-performance/jetpack.git' 
            }
        }

        stage('deploy osp using jetpack') {
            steps {
                echo 'deploy goes here'
            }
        }
    }
}
