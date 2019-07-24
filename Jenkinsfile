//@Library('retort-lib') _
import java.text.SimpleDateFormat
import java.lang.StringBuffer
import java.lang.Integer
def label = "jenkins-${UUID.randomUUID().toString()}"

def ZCP_USERID = 'mos'
def DOCKER_IMAGE = 'skn-mos/skn-mos-master'
def K8S_NAMESPACE = 'skn-mos-app-dev'

def dateFormat1 = new SimpleDateFormat("yyyyMMddHHmm")
def dateFormat2 = new SimpleDateFormat("HH")
def date = new Date()
def tempVer = dateFormat2.format(date).toInteger() % 2
if(tempVer==0){
	VERSION = 'EVEN'
}else{
	VERSION = 'ODD'
}

println("------------------------------------------------------------------------------------------------------------")
println(dateFormat1.format(date))
println(tempVer)
println(VERSION)
println("------------------------------------------------------------------------------------------------------------")

def b1 = new StringBuffer()
def b2 = new StringBuffer()
def b3 = new StringBuffer()
def b4 = new StringBuffer()
/*
def proc1 = "sudo docker pull tomcat:8".execute()
proc1.consumeProcessErrorStream(b1)
println proc1.text
println b1.toString()
"sleep 20".execute()

def proc2 = "sudo docker login -u wisdomguy -p Qhdks77!#".execute()
proc2.consumeProcessErrorStream(b2)
println proc2.text
println b2.toString()


"sleep 5".execute()
*/
def proc3 = "sudo docker tag tomcat:8 registry.koreacentral.cloudapp.azure.com/tomcat:8-fromjenkins".execute()
proc3.consumeProcessErrorStream(b3)
println proc3.text
println b3.toString()

"sleep 4".execute()

def proc4 = "sudo docker push registry.koreacentral.cloudapp.azure.com/tomcat:8-fromjenkins".execute()
proc4.consumeProcessErrorStream(b4)

println proc4.text
println b4.toString()


//docker build .
/*
label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label) {
    node(label) {
        stage('Run shell') {
            sh 'echo hello world'
        }
    }
}


//docker build -t username/demo:$VERSION . && docker push minudev1212/demo:$VERSION
//docker build .


//def label = "maven-${UUID.randomUUID().toString()}"
label = "maven-${UUID.randomUUID().toString()}"
def mvnOpts = '-DskipTests -ntp -Darguments="-DskipTests -ntp"'

podTemplate(label: label, containers: [
  containerTemplate(name: 'maven', image: 'maven:3.6.1-jdk-8', ttyEnabled: true, command: 'cat')
  ], volumes: [
  persistentVolumeClaim(mountPath: '/root/.m2/repository', claimName: 'maven-repo', readOnly: false)
  ]) {

  node(label) {
    stage('Checkout') {
      checkout scm
    }
    stage('Release prepare') {
      container('maven') {
          sh "mvn -B ${mvnOpts} release:clean release:prepare -DreleaseVersion=${releaseVersion} -DdevelopmentVersion=${developmentVersion}"
      }
    }
    stage('Release perform') {
      container('maven') {
          sh "mvn -B ${mvnOpts} release:perform"
      }
    }
  }
}
*/
// HARBOR_REGISTRY : 별도 Global로 세팅 된 변수임.
// HARBOR_CREDENTIALS : Jenkins폴더에 Credential로 등록한 이름과 같아야 함

/*
podTemplate(label:label,
    serviceAccount: "zcp-system-sa-${ZCP_USERID}",
    containers: [
        containerTemplate(name: 'jnlp', image: 'skn-mos-dev-registry.cloudzcp.io/cloudzcp/jnlp-slave:alpine', args:'${computer.jnlpmac} ${computer.name}'),
        containerTemplate(name: 'maven', image: 'skn-mos-dev-registry.cloudzcp.io/cloudzcp/maven:3.5.2-jdk-8-alpine', ttyEnabled: true, command: 'cat'),
        containerTemplate(name: 'docker', image: 'skn-mos-dev-registry.cloudzcp.io/cloudzcp/docker:17-dind', ttyEnabled: true, command: 'dockerd-entrypoint.sh', privileged: true),
        containerTemplate(name: 'kubectl', image: 'skn-mos-dev-registry.cloudzcp.io/cloudzcp/k8s-kubectl:v1.13.6', ttyEnabled: true, command: 'cat')
    ],
    volumes: [
        persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'zcp-jenkins-mvn-repo')
    ]) {

    node(label) {
        stage('SOURCE CHECKOUT') {
            def repo = checkout scm
            env.SCM_INFO = repo.inspect()
        }

        stage('BUILD MAVEN') {
            container('maven') {
                mavenBuild goal: 'clean package -U', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"], globalSettingsID: 'MAVEN_GLOBAL_SETTINGS'
            }
        }

        stage('BUILD DOCKER IMAGE') {
            container('docker') {
                dockerCmd.build tag: "${HARBOR_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"
                dockerCmd.push registry: HARBOR_REGISTRY, imageName: DOCKER_IMAGE, imageVersion: VERSION, credentialsId: "HARBOR_CREDENTIALS"
            }
        }

        stage('DEPLOY') {
            container('kubectl') {
                kubeCmd.apply file: 'k8s/configmap.yaml', namespace: K8S_NAMESPACE
                kubeCmd.apply file: 'k8s/service.yaml', namespace: K8S_NAMESPACE
                yaml.update file: 'k8s/deployment.yaml', update: ['.spec.template.spec.containers[0].image': "${HARBOR_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"]
//                def exists = kubeCmd.resourceExists file: 'k8s/deployment.yaml', namespace: K8S_NAMESPACE
//                if (exists) {
//                    kubeCmd.scale file: 'k8s/deployment.yaml', replicas: 0, namespace: K8S_NAMESPACE
//                }
                kubeCmd.apply file: 'k8s/deployment.yaml', namespace: K8S_NAMESPACE, wait: 300
            }
        }

//        stage('TAGGING') {
//            if ('develop' != VERSION) {
//                gitCmd.tag tag: VERSION, credentialsId: 'GIT_CREDENTIALS'
//            }
//        }
    }
}
*/
