properties(
	[parameters(
		[string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'graphite host name', name: 'graphite_host'),
		 string(defaultValue: 'perfci-osp13-neutron-ovs', description: 'graphite prefix name', name: 'graphite_prefix'),
		 string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'grafana_host name', name: 'grafana_host'),
		 string(defaultValue: '10.11.5.19', description: 'dns server', name: 'dns_server'),
                 string(defaultValue: 'neutron-ovs, nova', description: 'List of DFGs to test', name: 'dfg_list'),
		 text(defaultValue: '', description: 'Extra ansible vars', name: 'extra_vars')
		])
	])

node('perfci') {
    checkout scm

    stage('setup jetpack') {
        echo 'setup jetpack'
        //sh 'rm -rf jetpack'
        //sh 'git clone https://github.com/redhat-performance/jetpack.git'
        //sh 'cp instackenv.json jetpack/.'
        //sh 'cp osp13_vars.yml jetpack/group_vars/all.yml'
    }

    stage('deploy osp using jetpack') {
	echo 'deploy osp'
	//sh 'cd jetpack && ansible-playbook -vvv main.yml'
    }

    stage('setup browbeat') {
	echo 'setup browbeat'
        sh 'echo "graphite_host: ${graphite_host}" >> browbeat_vars.yml'
	sh 'echo "graphite_prefix: ${graphite_prefix}" >> browbeat_vars.yml'
        sh 'echo "grafana_host: ${grafana_host}" >> browbeat_vars.yml'
        sh 'echo "dns_server: ${dns_server}" >> browbeat_vars.yml'
        sh 'echo "collectd_container: false" >> browbeat_vars.yml'
	sh 'sed -i "s/cloudname/${graphite_prefix}/g" perfci-neutron.yaml'
        sh 'ansible-playbook -vvv browbeat_task.yml'
    }

    def dfgs = params.dfg_list.split(',')
    for (int i = 0; i < dfgs.length; i++) {
        stage("Run ${dfgs[i]} tests) {
           echo "running ${dgfs[i]} ..."
        }
    }
}
