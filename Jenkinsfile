node {
    
    stage('code checkout'){
        
        git 'https://github.com/ShaktiHazaru/insurance-project-demo.git'
    }
    
    stage('code build'){
        
        sh 'mvn clean package'
    }
    
    stage('containerize'){
        sh 'docker build -t shaktiehazaru/insurance-project-demo:1.0 .'
        
    }
    
    stage('release'){
        withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
           sh "docker login -u shaktiehazaru -p ${dockerhubpwd}"
           sh 'docker push shaktiehazaru/insurance-project-demo:1.0'
        }
    }
    
    stage ('deploy in test server'){
       ansiblePlaybook become: true, credentialsId: 'ansible-new-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage ('checkout-selenium-project'){
        git 'https://github.com/ShaktiHazaru/insureme-selenium.git'
    }
    
    stage ('build') {
        
        sh 'mvn clean package assembly:single'
    }
    
    stage ('run the jar'){
        sh 'java -jar target/insureme-selenium-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
        
    }
    
    stage('code checkout'){
        
        git 'https://github.com/ShaktiHazaru/insurance-project-demo.git'
    }
    
    stage('code build'){
        
        sh 'mvn clean package'
    }
    
    stage ('deploy in prod server'){
       ansiblePlaybook become: true, credentialsId: 'ansible-new-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }   
    
}
