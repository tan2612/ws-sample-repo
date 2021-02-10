pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                //get code from repo
                git 'https://github.com/tan2612/ws-sample-repo.git'
                bat "mvn clean package"
            }
        }
        stage('Test'){
            steps {
                '
                bat "mvn clean test"
            }
        }
        stage('Deploy'){
            steps {
    			withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-token1', vaultUrl: 'http://localhost:8200'], vaultSecrets: [[path: 'kv/jenkins-prop', secretValues: [[vaultKey: 'HartifactId'], [vaultKey: 'Henv'], [vaultKey: 'Hpassword'], [vaultKey: 'Husername'], [vaultKey: 'Hversion'], [vaultKey: 'HworkerType']]]]) {
            bat "mvn -e clean package deploy -DmuleDeploy -Dmule.version=$Hversion -Danypoint.username=$Husername -Danypoint.password=$Hpassword -Dcloudhub.env=$Henv -Dproject.artifactId=$HartifactId -Dcloudhub.workerType=$HworkerType"
				}
            }
        }
    }
}