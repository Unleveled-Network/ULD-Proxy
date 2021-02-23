pipeline {
    agent any
    tools {
        maven 'Maven 3'
        jdk 'Java 8'
    }
    options {
        buildDiscarder(logRotator(artifactNumToKeepStr: '5'))
    }
    stages {
        stage ('Build') {
            steps {
                sh 'git config --global user.email "gizmo0320@unleveledgaming.com" \
                && git config --global user.name "Gizmo0320"'
                sh "chmod +x ./scripts/jenkinsBuild.sh && ./scripts/jenkinsBuild.sh ${BUILD_ID}"
                withMaven {
                    sh "mvn -s /root/.m2/settings.xml -version"
                    sh 'mvn clean -Pupstream -B -DSNYK_API_ENDPOINT="https://snyk.io/" -Dbuild.number=${BUILD_NUMBER} install'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: "Waterfall-Proxy/bootstrap/target/Waterdog.jar,Waterfall-Proxy/module/cmd-server/target/cmd_server.jar,Waterfall-Proxy/module/cmd-list/target/cmd_list.jar,Waterfall-Proxy/module/reconnect-yaml/target/reconnect_yaml.jar,Waterfall-Proxy/module/cmd-find/target/cmd_find.jar,Waterfall-Proxy/module/cmd-send/target/cmd_send.jar,Waterfall-Proxy/module/cmd-alert/target/cmd_alert.jar", fingerprint: true
                }
            }
        }

        stage('Snapshot') {
            when {
                branch "master-zlib"
            }
            steps {
                sh 'mvn source:jar deploy -DskipTests'
            }
        }
    }
}
