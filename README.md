## Assignment 1: Set up a Jenkins pipeline that automates the testing and deployment of a simple Python web application.

### 1. Setup
Install Jenkins:

#### On a Virtual Machine:
Install Jenkins on your VM by following the official Jenkins installation guide.

#### Cloud-based Jenkins:
You can use services like Jenkins on AWS or Jenkins on Azure.
Configure Jenkins with Python:

Install Python on your Jenkins server.
Install necessary libraries using pip:
pip install pytest

### 2. Source Code
Fork and Clone Repository:

Fork a sample Python web application repository. Here’s a sample repository you can use: Sample Python Web App.
Clone the forked repository into your Jenkins server:
git clone https://github.com/YOUR_USERNAME/python-getting-started.git

### 3. Jenkins Pipeline
Create a Jenkinsfile:

In the root of your Python application repository, create a file named Jenkinsfile with the following content:
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                sh 'pytest'
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                // Add your deployment steps here
                echo 'Deploying to staging environment...'
            }
        }
    }

    post {
        success {
            mail to: 'you@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Good news! The build succeeded."
        }
        failure {
            mail to: 'you@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Unfortunately, the build failed. Please check the Jenkins logs for more details."
        }
    }
}

### 4. Triggers
Configure Build Triggers:

In your Jenkins job configuration, under the “Build Triggers” section, select “GitHub hook trigger for GITScm polling” to trigger builds on changes to the main branch.

### 5. Notifications
Set Up Email Notifications:

Configure the email notification system in Jenkins:
Go to “Manage Jenkins” > “Configure System”.
In the “E-mail Notification” section, set up your SMTP server details.
Ensure your Jenkins job has the correct email addresses configured in the post section of the Jenkinsfile.
