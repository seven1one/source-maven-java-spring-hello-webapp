pipeline {
    // 어떤 젠킨스 에이전트에서도 실행 가능하도록 설정
    agent any

    // 1분마다 Git 저장소를 확인해서 변경 사항이 있으면 자동으로 빌드 시작
    triggers {
        pollSCM('* * * * *')
    }

    stages {
        // 1단계: GitHub에서 소스 코드 가져오기
        stage('Checkout') {
            steps {
                // <YOUR_REPOSITORY_URL> 부분을 본인의 GitHub 주소로 바꾸세요.
                git branch: 'main', url: '<YOUR_REPOSITORY_URL>'
            }
        }

        // 2단계: 테스트를 제외하고 빠르게 빌드하여 WAR 파일 생성
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        // 3단계: 소스 코드에 문제가 없는지 테스트 수행
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        // 4단계: 빌드된 파일을 톰캣(Tomcat) 서버로 전송 및 배포
        stage('Deploy') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-manger', path: '', url: 'http://192.168.56.102:8080')], contextPath: 'hello-world', war: 'target/hello-world.war'
            }
        }
    }

 
