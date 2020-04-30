properties(
	[parameters(
		[string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'graphite host name', name: 'graphite_host'),
		 string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'grafana host name', name: 'grafana_host'),
		 string(defaultValue: '10.11.5.19', description: 'dns server', name: 'dns_server'),
                 string(defaultValue: 'neutron', description: 'List of DFGs to test', name: 'dfg_list'),
                 string(defaultValue: '10.9.76.205', description: 'elasticsearch host', name: 'ES_host'),
		 text(defaultValue: '', description: 'Extra ansible vars', name: 'extra_vars')
		])
	])
// workaround for first build
params.each { k, v -> env[k] = v }

node('perfci') {
    checkout scm

    stage('setup jetpack') {
        echo 'setup jetpack'
        sh 'rm -rf jetpack'
        sh 'git clone https://github.com/redhat-performance/jetpack.git'
        sh 'cp instackenv.json jetpack/.'
        sh 'cp jetpack_all.yml jetpack/group_vars/all.yml'
    }

    stage('deploy osp using jetpack') {
	echo 'deploy osp'
	sh 'cd jetpack && ansible-playbook -vvv main.yml'
    }

    stage('setup browbeat') {
	echo 'setup browbeat'
	sh 'scp stack@172.16.0.2:/home/stack/browbeat/ansible/install/group_vars/all.yml browbeat_vars.yml'
        sh 'echo "graphite_host: ${graphite_host}" >> browbeat_vars.yml'
        sh 'echo "grafana_host: ${grafana_host}" >> browbeat_vars.yml'
        sh 'echo "dns_server: ${dns_server}" >> browbeat_vars.yml'
        sh 'echo "collectd_container: false" >> browbeat_vars.yml'
        sh 'ansible-playbook -vvv browbeat_install.yml'

        sh 'chmod +x create-vars.sh'
        sh './create-vars.sh'
    }

    def dfgs = params.dfg_list.split(',')
    for (int i = 0; i < dfgs.length; i++) {
        String dfg = dfgs[i].trim()
        stage("Run ${dfg} tests") {
           sh """cp ${dfg}-config-vars.yml browbeat-workload-vars.yml"""
           sh """ansible-playbook prepare-config.yml"""
           sh """sed -i "s/cloud_name: openstack/cloud_name: perfci-osp13-ovs-${dfg}/" browbeat-config.yaml"""
           sh """echo "graphite_prefix: perfci-osp13-ovs-${dfg}" >> browbeat_vars.yml"""
           sh 'ansible-playbook -vvv run_browbeat_tests.yml'
        }
    }
}
