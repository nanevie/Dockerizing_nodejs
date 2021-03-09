//https://github.com/nodejs/examples.git
/*
checkout poll: false, scm: [$class: 'GitSCM', 
branches: [[name: '* /master']], 
doGenerateSubmoduleConfigurations: false, 
extensions: [], submoduleCfg: [], 
userRemoteConfigs: [[credentialsId: 'GithubSSH2', 
url: 'https://github.com/ParthaK8/codesamples']]]
*/
//git changelog: false, credentialsId: 'githubkey', poll: false, url: 'https://github.com/nanevie/LearnJenkins'
def myCheckout(myGitUrl, myBranch, myLocalDir) {
    checkout changelog: false, poll: false, scm: [$class: 'GitSCM',
    branches: [[name: myBranch]],
    doGenerateSubmoduleConfigurations: false,
    extensions: [[$class: 'RelativeTargetDirectory',
    relativeTargetDir: myLocalDir]],
    submoduleCfg: [],
    userRemoteConfigs: [[credentialsId: 'GithubSSH',
    url: myGitUrl]]
    ]
}

node () {
    stage ('Clone Git') {
        echo "Cloning git"
        def trialGit = "https://github.com/nanevie/Dockerizing_nodejs.git"
        def trial_branch = "*/main"
        def local_dir = "nancygitjenkins"
        myCheckout(trialGit, trial_branch, local_dir)

        // set an environment variable, value of local_dir
        env.LOCAL_DIR = local_dir
        
        // Verify
        sh "ls -la"
        sh "ls -la ${LOCAL_DIR}"
        def CWDABSPATH
        CWDABSPATH = sh (

        script: "echo `pwd`",
                returnStdout: true
        ).trim()
        println "Current Working Directory: " + CWDABSPATH
        env.BASEPATH = CWDABSPATH  // set env var for BASEPATH
    }

    stage ('install and build') {
     sh '''#!/bin/bash -l  
     cd nancygitjenkins/addressbook/
     npm install
     cd ../
     docker build -t evie0320/app_addressbook:v2 . 
     docker push evie0320/app_addressbook
       
      '''  

    }
}
