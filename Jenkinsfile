pipeline{
    agent any
     // tools ---> in jenkins server use ui and install plugin
     tools {
        maven 'Maven'      //this point to the name in jenkins ui tool configured under maven
     }
    stages{
        stage("Test"){
            steps{
                     // mvn test
                slackSend channel: 'sample-projects', message: 'Job Started'     
                sh "mvn test"     
            }
        }
        stage("Build"){
            steps{
                    // maven package/build
                sh "mvn package" 
            }
        }
        stage("Deploy on Test"){
            steps{
                    // deploy on container ---plugin should be installed in jenkins ui..
                deploy adapters: [tomcat9(credentialsId: 'tomcatdetails', path: '', url: 'http://20.235.136.234:8080/')], contextPath: '/app', war: '**/*.war'  
            }
        }
        stage("Deploy on Prod"){
            input {
                message "Should we continue"
                ok "Yes you should"
            }
            steps{
                    // deploy on container ---plugin should be installed in jenkins ui..
                deploy adapters: [tomcat9(credentialsId: 'tomcatdetails', path: '', url: 'http://20.244.37.145:8080/')], contextPath: '/app', war: '**/*.war'    
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
            slackSend channel: 'sample-projects', message: 'Job Success'
        }
        failure{
            echo "========pipeline execution failed========"
            slackSend channel: 'sample-projects', message: 'Job Failed'
        }
    }
}