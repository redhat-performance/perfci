pipeline {
    agent { label 'perfci' }
    stages {
        stage('setup jetpack') {
            steps {
                sh 'git clone https://github.com/masco/perfci.git' 
            }
        }
        stage('deploy osp using jetpack') {
        }
    }
}
