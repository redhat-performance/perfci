properties(
	[parameters(
		[string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'graphite host name', name: 'graphite_host'),
		 string(defaultValue: 'perf-ci', description: 'graphite prefix name', name: 'graphite_prefix'),
		 string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'grafana_host name', name: 'grafana_host'),
		 string(defaultValue: '10.11.5.19', description: 'dns server', name: 'dns_server'),
		 text(defaultValue: '', description: 'Extra ansible vars', name: 'extra_vars')
		])
	])

def remote = [:]
remote.name = "undercloud"
remote.host = "172.16.0.2"
remote.allowAnyHosts = true

node('perfci') {
    checkout scm

    stage('setup jetpack') {
        echo 'setup jetpack'
        /*sh 'rm -rf jetpack'
        sh 'git clone https://github.com/redhat-performance/jetpack.git'
        sh 'cp instackenv.json ~/.'
        sh 'cp osp13_vars.yml jetpack/group_vars/all.yml'*/
    }

    stage('deploy osp using jetpack') {
	echo 'deploy osp'
	//sh 'cd jetpack && ansible-playbook -vvv main.yml 2>&1 | tee log'
    }

    stage('setup browbeat') {
	echo 'setup browbeat'
        sh 'echo "graphite_host: ${graphite_host}" >> browbeat_vars.yml'
	sh 'echo "graphite_prefix: ${graphite_prefix}" >> browbeat_vars.yml'
        sh 'echo "grafana_host: ${grafana_host}" >> browbeat_vars.yml'
        sh 'echo "dns_server: ${dns_server}" >> browbeat_vars.yml'
        sh 'echo "collectd_container: false" >> browbeat_vars.yml'
	//sh "dns=`cat /etc/resolv.conf | grep nameserver | head -n1 | cut -d ' ' -f2` && echo 'dns_server: $dns' >> browbeat_vars.yml"
        //sh 'echo "dns_server: ${dns}" >> browbeat_vars.yml'
    withCredentials([sshUserPrivateKey(credentialsId: 'privkey', keyFileVariable: 'keyfile', usernameVariable: 'username')]) {
        remote.user = username
        remote.identityFile = keyfile
        sshPut remote: remote, from: 'browbeat_vars.yml', into: '/home/stack/browbeat/ansible/install/group_vars/all.yml'
	sshCommand remote: remote, command: 'cd /home/stack/browbeat/ansible && ansible-playbook -i hosts install/browbeat.yml' 
	sshCommand remote: remote, command: 'cd /home/stack/browbeat/ansible && ansible-playbook -i hosts install/collectd.yml' 
    }
    }
}
