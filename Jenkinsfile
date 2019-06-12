pipeline {
    agent {
        docker {
            // downloads and runs the maven docker image as a separate container that acts as the agent that Jenkins uses to run the project
            // this container is terminated after pipeline execution
            image 'maven:3-alpine' 
            // creates a reciprocal mapping between the maven repo directories in the maven container and that of the host filesystem
            // ensures that artifacts are retained beyond the maven container execution without having to re-download the artifacts
            // however, the host filesystem will be cleared each time docker is restarted
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                // runs the maven command to clean build the Java app without running tests
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                // runs maven command to run unit test on java app
                // also generates a JUnit XML report saved to the target/surefire-reports directory in the /var/jenkins_home/workspace/simple-java-maven-app directory of the Jenkins container
                sh 'mvn test'
            }
            // the always condition of the post section ensures that the step is always executed after the completion of the test stage regardless of its outcome
            post {
                always {
                    // Archives the JUnit XML report and exposes results through Jenkins interface
                    // can be accessed in the Tests page of a pipeline run through Blue Ocean
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }       
    }  
}