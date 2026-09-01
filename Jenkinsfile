pipeline { 

    agent none 

 

    tools { 

        jdk 'JAVA_21' 

        maven 'MAVEN_3' 

    } 

 

    stages { 

        stage('1. Fetch Code') { 

            agent { label 'build' } 

            steps { 

                echo 'Pulling source code from GitHub...' 

                checkout scm 

            } 

        } 

 

        stage('2. Compile & Test') { 

            agent { label 'build' } 

            steps { 

                echo 'Building the application with Maven...' 

                sh 'mvn clean package' 

                stash name: 'compiled-app', includes: 'target/*.jar' 

            } 

        } 

 

        stage('3. Deploy to Production') { 

            agent { label 'deploy' } 

            steps { 

                echo 'Deploying to Docker on the Production Server...' 

                unstash 'compiled-app' 

 

                sh ''' 

                    docker stop my-web-app || true 

                    docker rm my-web-app || true 

 

                    docker run -d --name my-web-app -p 80:8080 \ 

                    -v $(pwd)/target:/app \

                    eclipse-temurin:21-jre-alpine java -jar /app/my-web-app-0.0.1-SNAPSHOT.jar 

                ''' 

            } 

        } 

    } 

}
