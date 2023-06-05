def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE':'danger', 
    ]

pipeline {
    agent any
    
    tools {
        //Que herramientas queremos utilizar 
        maven "jenkinsmaven"
        jdk "jenkinsjava"
        
    }

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
    
    stages {
        stage('Hello') {
            steps {
                echo "Database engine is ${DB_ENGINE}"
                echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "Job Name: ${env.JOB_NAME}"
                echo "Job Name: ${env.JAVA_HOME}"
            }
            
        }
        
        stage('Git Polling'){
            steps{
                git branch: 'master', url: 'https://github.com/MiguelAngelRamos/time-tracker.git'
                
            }
        }
        
        stage('BUILD CON MAVEN'){
            steps{
               // bat "mvn clean package -DskipTests" 
               
               sh "mvn -Dmaven.test.failure.ignore=true clean package " //Unix
            }
            
            post{
                success{
                    echo 'Archivar Artefactos'
                    archiveArtifacts "core/target/*.jar"
                    archiveArtifacts "web/target/*.war"
                }
            }
        }
        
        stage('test maven'){
            steps{
                // bat "mvn test"
                sh "mvn test"
            }
        }
        
        
    }
    post{
        always{
            echo 'Slack Notification'
            slackSend channel: '#integracion',
            color: COLOR_MAP[currentBuild.currentResult], 
            message: "*${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More Info at: ${env.BUILD_URL}"
        }
    }
}
