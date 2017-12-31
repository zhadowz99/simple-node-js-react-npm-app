node('jenkins-agent') {
  environment {
        CI = 'true'
    }

  try{

    stage('Cleaning Workspace') {
        echo 'Initializing for clean build...'
        deleteDir()
    }
    
    stage('Initialize') {
        echo 'Initializing...'
        def node = tool name: 'NodeJS-6.12', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        env.PATH = "${node}/bin:${env.PATH}"
    }

    stage('Checkout SCM') {
        echo 'Checkout SCM...'
        checkout scm
    }
  
    try{

        stage('Check Env') {
            echo 'Checking Environment...'
            sh 'node -v'
            sh 'npm -v'
        }

        stage('Build') {
            echo 'Building Dependency...'
            sh 'npm install'
        }
	
    }finally{
        
        stage('SonarQube analysis') {
        
        def scannerHome = tool 'SonarQube Scanner';
        withSonarQubeEnv('SonarQube') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
        stage('Deliver') {
                sh './scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './scripts/kill.sh'
        }
    }
    
  }

  }catch(e){

    throw e;
  }
}

