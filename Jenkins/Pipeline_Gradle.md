```
node{
    def gradleHome
    def artifactId
    def version
    def jar

    // GitLab 주소및 브렌치 체크아웃
    stage('checkout'){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId:'test',url: 'http://172.16.10.168:8999/startkit/project']]])
    }

    // jar파일명 정해주기
    stage('Initialize') {
        gradleHome=tool "G7"
        sh(gradleHome+"/bin/gradle properties -q")
        artifactId = sh(script:gradleHome+"/bin/gradle properties -q | grep 'archivesBaseName:' | awk '{print \$2}'", returnStdout: true).trim()
        echo artifactId
        version = sh(script:gradleHome+"/bin/gradle properties -q | grep 'version:' | awk '{print \$2}'", returnStdout: true).trim()
        echo version
        jar = artifactId + '-' + version + '.jar'
        echo jar
    }

    // 빌드
    stage('build'){
        sh "${gradleHome}/bin/gradle clean build"
    }

    // 프로세스 죽이기
    stage('kill_pid'){
        if (sh("ps -ef | grep " + artifactId + " | grep -v grep | grep -v jenkins_home | awk '{print\$2}'")!=null){
            sh("kill -9 `ps -ef | grep " + artifactId + " | grep -v grep | grep -v jenkins_home | awk '{print\$2}'`")
        }
    }

    // jar 파일 실행
    stage('deploy'){
            sh('JENKINS_NODE_COOKIE=dontKillMe && nohup java -jar build/libs/'+jar+' &')
    }
}
```

## 세부내용
```
gradle properties -q | grep 'archivesBaseName:' | awk '{print \$2}'
```
- gradle일 때 Properties 출력하는 방법

```
sh('JENKINS_NODE_COOKIE=dontKillMe && nohup java -jar build/libs/'+jar+' &')
```
  - JENKINS_NODE_COOKIE=dontKillMe
  - 이렇게설정하지 않으면 Job 종료시 jenkins에 의해 프로세스가 kill 됨

<br/>

### 참고
- https://stackoverflow.com/questions/24936781/gradle-plugin-project-version-number
