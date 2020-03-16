#!groovy

@Library('jenkinslib') _

def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"

//Pipeline
pipeline {
    agent {
        node {
            label "master" //指定运行节点的标签或者名称
            customWorkspace "${workspace}" //指定运行工作目录（可选）
        }
    }
    parameters {
        string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '')
    }
    options {
        timestamps() //日志会有时间
        skipDefaultCheckout() //删除隐式checkout scm语句
        disableConcurrentBuilds() //禁止并行
        timeout(time:1, unit:"HOURS") //流水线超时设置1h
    }
    stages {
        //下载代码
        stage("GetCode") { //阶段名称
            when {
                environment name: 'test', value: 'abcd'
            }
            steps {
                timeout(time:5, unit:"MINUTES") { //步骤超时时间
                    script { //填写运行代码
                        println("获取代码")
                        println("${test}")
                    }
                }
            }
            input {
                message "Should we continue?"
                ok "Yes, we should"
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who shoud I say hello to?')
                }
            }
        }
        stage("01") {
            failFast true
            parallel {
                //构建
                stage("Build") {
                    steps {
                        timeout(time:20, unit:"MINUTES") {
                            script {
                                print("应用打包")
                                
                                mvnHome = tool "M2"
                                println(mvnHome)
                            }
                        }
                    }
                }
                //代码扫描
                stage("CodeScan") {
                    steps {
                        timeout(time:30, unit:"MINUTES") {
                            script {
                                print("代码扫描")

                                tools.PrintMes("this is my lib!")
                            }
                        }
                    }
                }
            }
        }

    }
    post {
        always {
            script {
                println("always")
            }
        }
    
        success {
            script {
                currentBuild.description = "\n 构建成功！"
            }
        }

        failure {
            script {
                currentBuild.description = "\n 构建失败！"
            }
        }    
    
        aborted {
            script {
                currentBuild.description = "\n 构建取消！"
            }
        }  
    
    }
}
