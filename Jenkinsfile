node {
  stage ('Build') {
    node ('linux-build') {
      checkout scm
      sh 'pip install -U virtualenv'
      sh 'python -m virtualenv --clear env'
      // sh './scripts/jenkins_build.sh'
      // sh './scripts/jenkins_archive.sh'
      deleteDir()
    }
  } 
  stage ('Performance Test') {
    def platforms = ['perf-ubuntu-a0', 'perf-ubuntu-ds1']
    def perftests = [:]
    def perf_closure = {
      platform -> 
        node (platform) {
          checkout scm
          echo "Branch ${env.BRANCH_NAME}"
          // echo "node ${node_label}"
          echo "plat ${platform}"
          sh 'ifconfig'
          // sh 'pip install -U virtualenv'
          // sh 'python -m virtualenv --clear env'
          // sh './scripts/jenkins_perf.sh'
          deleteDir()
        }
    }

    for (int i = 0; i < platforms.size(); i++) {
      platform = platforms.get(i)
      perftests["Test ${platform}"] = { perf_closure(platform) }
    }
    perftests.failFast = false

    parallel perftests
  }
}
