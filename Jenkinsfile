pipeline {
    agent any

    parameters {
        choice(name: 'BRANCH', choices: getGitBranches(), description: 'Select the branch to build')
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Selected branch is : ${params.BRANCH}"
                    checkout([
                        $class: 'GitSCM', 
                        branches: [[name: "*/${params.BRANCH}"]],
                        userRemoteConfigs: [[url: 'https://github.com/santoshPagire/test_jenkin.git']]
                    ])
                }
            }
        }
        
    }
}

def getGitBranches() {
    node {
        def branches = []
        try {
            branches = sh(script: 'git ls-remote --heads origin', returnStdout: true)
                .readLines()
                .collect { it.split()[1].replaceAll('refs/heads/', '') }
                .sort()
        } catch (Exception e) {
            echo "Error fetching branches: ${e.message}"
        }
        return branches.join('\n')
    }
}
