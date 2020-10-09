node {
    
    stage('Checkout from GitHub') {
        git url: 'https://github.com/Moopz123/sample-app-docker.git'
    }

    stage('Build and Push to Azure Container Registry') {
            app = docker.build("$env.ACR/$env.image")
            docker.withRegistry("https://$env.ACR", 'ACR-CRED') {
            app.push("${env.BUILD_NUMBER}")
            app.push('latest')
            }
    }
    
    stage('Deploy to Test AKS'){
        acsDeploy azureCredentialsId: env.azureCredentialsId, 
        configFilePaths: 'test.yml', 
        containerRegistryCredentials: [[credentialsId: 'ACR-CRED', url: "https://$env.ACR"]], 
        containerService: "$env.AKS_Name | AKS", 
        resourceGroupName: env.rg,
        enableConfigSubstitution: true
    }
}
