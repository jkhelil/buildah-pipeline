podTemplate(cloud: 'openshift', yaml:'''
spec:
  securityContext:
    runAsuser: 0
  containers:
  - name: jnlp
    image: jenkins/jnlp-slave:4.0.1-1
    volumeMounts:
    - name: home-volume
      mountPath: /home/jenkins
    env:
    - name: HOME
      value: /home/jenkins
  - name: buildah
    image: quay.io/buildah/stable:v1.17.0
    command: ['cat']
    tty: true
    volumeMounts:
    - name: home-volume
      mountPath: /home/jenkins
  volumes:
  - name: home-volume
    emptyDir: {}
''') {
  node(POD_LABEL) {
    stage('Build a buildah project') {
      container('buildah') {
        git 'https://github.com/jkhelil/buildah-pipeline.git'
        sh 'buildah bud -f ./Dockerfile -t jawed-buildah:1.0. --storage-driver vfs .'
      }
    }
  }
}
