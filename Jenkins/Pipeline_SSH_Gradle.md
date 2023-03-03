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
        script {
            sshPublisher(publishers: [sshPublisherDesc(configName: 'rootdemo',
                    transfers: [sshTransfer(
                            execCommand: "kill -9 `ps -ef | grep " + artifactId + " | grep -v grep | awk '{print\$2}'`")
                    ])
            ])
        }

    }

    // jar 배포 및 실행
    stage('deploy'){
            script {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'rootdemo',
                        transfers: [sshTransfer(cleanRemote:true,
                                sourceFiles: "build/libs/"+jar,
                                execCommand: "java -jar /var/startkit/build/libs/"+jar)
                        ])
                ])
            }

    }
}
```

<br/>

### 참고
- https://stackoverflow.com/questions/69006002/jenkins-publish-over-ssh-pipeline-parameterized
