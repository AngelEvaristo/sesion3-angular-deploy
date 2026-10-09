pipeline {
  agent any

  environment {
      PATH = "C:\\Users\\aevar\\AppData\\Roaming\\npm;C:\\Windows\\System32\\WindowsPowerShell\\v1.0;${env.PATH}"
  }

  stages {
    
      stage('Clone from github'){
          steps {
              checkout scmGit(branches: [[name: "*/main"]], extensions: [], userRemoteConfigs: [[credentialsId: 'AngelEvaristo', url: 'https://github.com/AngelEvaristo/sesion3-angular-deploy.git']])
          }
      }
      stage('Install angular cli'){
          steps {
              script {
                echo "Instalando angular cli"
                bat """
                    npm install -g @angular/cli                    
                """                
              }
          }
      }
      stage('validar ng'){
          steps {
            bat 'ng version'
          }  
      }
      stage('instalar dependencias'){
          steps {
            bat 'npm install'
          }
      }
    
      stage('Compilacion'){
          steps {
            bat 'ng build --configuration production'
          }
      }  
    
      stage('chequeo y Empaquetado'){
          steps {
            bat 'powershell -Command "Compress-Archive -Path dist\\my-first-angular-app\\browser\\* -DestinationPath angular_app.zip -Force"'
            bat 'dir'
          }
      }     

      stage('Deploy Azur WebAPP'){
          steps {
              withCredentials([azureServicePrincipal('azure_SP')]) {
                  bat '''
                      az webapp deploy \
                      --resource-group sesion3-tecylab \
                      --name test-sesion3-tecylab \
                      --restart true \
                      --src-path angular_app.zip \
                      --type zip                                                          
                  '''
              } 
          }
      }      
    
    
  }  
}
