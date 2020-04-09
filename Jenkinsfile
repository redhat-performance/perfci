properties(
	[parameters(
		[string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'graphite host name', name: 'graphite_host'),
		 string(defaultValue: 'norton.perf.lab.eng.rdu2.redhat.com', description: 'grafana host name', name: 'grafana_host'),
		 string(defaultValue: '10.11.5.19', description: 'dns server', name: 'dns_server'),
                 string(defaultValue: 'neutron, nova', description: 'List of DFGs to test', name: 'dfg_list'),
                 string(defaultValue: 'elk-b09-h30-r720xd.rdu.openstack.engineering.redhat.com', description: 'elasticsearch host', name: 'ES_host'),
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
        //sh 'cp jetpack_all.yml jetpack/group_vars/all.yml'
    }

    stage('deploy osp using jetpack') {
	echo 'deploy osp'
	//sh 'cd jetpack && ansible-playbook -vvv main.yml'
	sh 'echo "boo: ${Boo}"'
    }

    /*stage('setup browbeat') {
	echo 'setup browbeat'
	sh 'scp stack@172.16.0.2:/home/stack/browbeat/ansible/install/group_vars/all.yml browbeat_vars.yml'
        sh 'echo "graphite_host: ${graphite_host}" >> browbeat_vars.yml'
        sh 'echo "grafana_host: ${grafana_host}" >> browbeat_vars.yml'
        sh 'echo "dns_server: ${dns_server}" >> browbeat_vars.yml'
        sh 'echo "collectd_container: false" >> browbeat_vars.yml'
        sh 'ansible-playbook -vvv browbeat_install.yml'

	sh 'scp stack@172.16.0.2:/home/stack/browbeat/browbeat-config.yaml .'
        sh 'sed -i "/elasticsearch/{n;s/: false/: true/}" browbeat-config.yaml'
        sh 'sed -i "s/host: 1.1.1.1/host: ${ES_host}/" browbeat-config.yaml'
    }

    def dfgs = params.dfg_list.split(',')
    for (int i = 0; i < dfgs.length; i++) {
        sh 'cp browbeat-config.yaml browbeat_config.yaml'
        String dfg = dfgs[i].trim()
        stage("Run ${dfg} tests") {
           if (dfg == 'neutron') {
               echo "neutron job"
               sh """sed -i "s/cloud_name: openstack/cloud_name: perfci-osp13-${dfg}-ovs/" browbeat_config.yaml"""
	       sh """sed -i "/- name: ${dfg}/{n;s/: false/: true/}" browbeat_config.yaml"""

               sh """echo "graphite_prefix: perfci-osp13-${dfg}-ovs" >> browbeat_vars.yml"""
           } else {
               echo "running ${dfg} ..."
               sh """sed -i "s/cloud_name: openstack/cloud_name: perfci-osp13-${dfg}/" browbeat_config.yaml"""
	       sh """sed -i "/- name: ${dfg}/{n;s/: false/: true/}" browbeat_config.yaml"""

               sh """echo "graphite_prefix: perfci-osp13-${dfg}" >> browbeat_vars.yml"""
           }
           sh 'ansible-playbook -vvv run_browbeat_tests.yml'
        }
    }*/
}
