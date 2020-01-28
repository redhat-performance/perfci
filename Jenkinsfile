pipeline {
    agent { label 'perfci' }
    stages {
        stage('setup jetpack') {
            steps {
                sh 'rm -rf jetpack'
                sh 'git clone https://github.com/redhat-performance/jetpack.git' 
	        sh 'cp instackenv.json ~/.'
                sh 'cp osp13_vars.yml jetpack/group_vars/all.yml'
            }
        }

        stage('deploy osp using jetpack') {
            steps {
                sh 'cd jetpack && ansible-playbook -vvv main.yml 2>&1 | tee log'
            }
        }
    }
}
