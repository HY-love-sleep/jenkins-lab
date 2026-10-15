pipeline {
    agent any
    triggers { pollSCM('H/1 * * * *') }

    stages {
        stage('证明源码来自 Git') {
            steps {
                sh 'git log -1 --format="本次构建的提交: %h  %s"'
                sh 'git rev-parse --short HEAD'
            }
        }
        stage('准备源码') {
            steps {
                writeFile file: 'src/Hello.java', text: '''
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, " + (args.length > 0 ? args[0] : "Jenkins") + "!  构建号=" + System.getenv().getOrDefault("BUILD_NUMBER", "?"));
    }
}
'''
            }
        }
        stage('编译') {
            steps {
                sh 'javac -d out src/Hello.java'
            }
        }
        stage('打包') {
            steps {
                sh 'jar --create --file hello.jar --main-class Hello -C out .'
            }
        }
        stage('验证') {
            steps {
                sh 'java -jar hello.jar HY-love-sleep'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'hello.jar', allowEmptyArchive: true
        }
    }
}
