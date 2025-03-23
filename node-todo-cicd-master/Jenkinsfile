@Library("Shared_lib@main") _

pipeline {
    agent any
    
    stages {

        stage("Clean WorkSpace"){
            steps{
                script{
                    cleanWs()
                }
            }
        }
        
        stage("Code checkout"){
            steps{
                script{
                    code_checkout("https://github.com/DevMadhup/node-todo-cicd.git","master")
                }
            }
        }

        stage("Code checkout"){
            steps{
                script{
                    owasp_dependency()
                }
            }
        }
        
        stage("Build Docker image"){
            steps{
                script{
                        docker_build("node-app","v1","madhupdevops")  
                }
            }
        }

        stage("Push Docker image"){
            steps{
                script{
                        docker_push("node-app","v1","madhupdevops")  
                }
            }
        }

        stage("Docker Artifacts Cleanup"){
            steps{
                script{
                        docker_cleanup("node-app","v1","madhupdevops")  
                }
            }
        }
    }
}
