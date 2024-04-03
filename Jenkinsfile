pipeline {
    agent any
    
    parameters {
        password(name: 'GITHUB_TOKEN', defaultValue: '', description: 'Enter GITHUB_TOKEN')
        password(name: 'GITLAB_TOKEN', defaultValue: '', description: 'Enter GITLAB_TOKEN')
        string(name: 'GITLAB_LOG_PROJECT_PATH', defaultValue: '', description: 'Kindly paste in below format: \ngitlab_namespace/project_name')
    }
    
    
    stages {
        stage('Clone Repository') {
            steps {
                // git credentialsId: 'SCM_MIGRATOR_CRED', branch: 'main', url: '${SCM_MIGRATOR_SOURCE_CODE}'
                git branch: 'main', url: 'https://github.com/vinayhegde105/hub-lab.git'
            }
        }
        
        stage('SCM Migration') {
            steps {
                script {
        wrap([$class: 'MaskPasswordsBuildWrapper',
              varPasswordPairs: [
                  [password: "${GITLAB_TOKEN}", var: 'GITLAB_TOKEN'],
                  [password: "${GITHUB_TOKEN}", var: 'GITHUB_TOKEN']
              ]
        ]) {
            sh '''
                #!/bin/bash
                export gitlab_token=$GITLAB_TOKEN
                export github_token=$GITHUB_TOKEN
                export repo_path=$GITLAB_LOG_PROJECT_PATH
                pip3 install pandas
                pip3 install openpyxl
                python3 github-gitlab/github-gitlab.py
            '''
        }
    }
            }
        }
        // stage('Send Email') {
        //     steps {
        //         script {
        //             emailext attachmentsPattern: '*.csv', 
        //                         body: 'Please find the attached CSV files.', 
        //                         mimeType: 'text/csv', 
        //                         subject: 'CSV Files',
        //                         to: 'sssssabhahit@gmail.com'
        //             }
        //     }
        // }
    }
}
