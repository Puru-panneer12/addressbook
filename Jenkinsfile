pipeline {
    agent none
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
      
    parameters { 
        string(name: 'Env', defaultValue: 'Test', description: 'version to deploy') 
        booleanParam(name: 'executeTests', defaultValue: true, description: 'decide to run tc')
        choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'])
    }
    stages {
        stage('Compile') {
            agent {
        // Specify the label or name of the Jenkins agent (slave)
        label 'linux_slave1'
    }
            steps {
                echo 'Compiling the code'
                echo "Compiling in ${params.Env}"
                sh 'mvn compile'
            }
        }
        stage('Test') {
            agent {
        // Specify the label or name of the Jenkins agent (slave)
        label 'linux_slave2'
    }
            when {
                expression{
                    params.executeTests == true
                }
            }
            steps {
                echo 'Testing the code'
                sh  'mvn test'
            }
        }
        stage('Package') {
            agent {
        // Specify the label or name of the Jenkins agent (slave)
        label 'linux_slave3'
    }
             input {
                message "Select the environment to deploy"
                ok "Deployed"
                parameters{
                    choice( name:'NEWAPP', choices:['SIT','UAT','PROD'])
                }
             }

            steps {
                echo 'Packaging the code'
                echo "Packaging version ${params.APPVERSION}"
                sh 'mvn package'
            }
        }
    }
}
