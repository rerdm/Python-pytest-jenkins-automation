# Python-pytest-jenkins-automation

# Python-pytest-jenkins-automation (Creating Allure Report)

In this project in have created a jenkins-pipeline. In this pipeline a pytest ist executed in addition    
an allure Report is created.


## GitHub
1. Create a GitHub-Repository with a basic pytest program.<b><br>NOTE : </b>Private Repos need authentication from Jenkins side.
## Jenkins
2. Create a Jenkins-Job (Pipeline Job) <br>
   2.1 Add a Pipeline-Script :<br>You can create a Pipeline-Script in Jenkins or you can choose Pipeline-Script from SCM ( in ths case a jenkins file need to be present in your Git-Repository)<br>
   You can create the Pipline with Jenkins-Pipeline-Syntax.

Example
````
pipeline {
    agent any

    environment {
        ALLURE_HOME = tool 'allure-commandline' // Assuming you have configured Allure command-line tool as a Jenkins tool
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rerdm/Python-pytest-jenkins-automation.git']])
            }
        }

        stage('Build and Run Tests') {
            steps {
                script {
                    // Install required dependencies , have to be present in your repo.
                    bat 'pip install -r requirements.txt' // Adjust the path to your requirements.txt file if needed

                    // Run Pytest with creating pytest html report and Allure report generation 
                    bat 'pytest tests/test_calculate.py --html=reports/reports.html --alluredir=target/allure-results'
                }
            }
        }

        stage('Generate Allure Report') {
            steps {
                script {
                    // Generate Allure report 
                    allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
                    
                }
            }

        }

    }
}


````

## Run The Pipeline

3. Now you can build the programm and see the output in the console log from Jenkins.<br>You will see the allure symbol next to the associated execution.