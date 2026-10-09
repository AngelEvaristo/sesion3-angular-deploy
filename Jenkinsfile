pipeline {
  agent any

  environment {
      PATH = "C:\\Users\\aevar\\AppData\\Roaming\\npm;C:\\Windows\\System32\\WindowsPowerShell\\v1.0;${env.PATH}"
      CLOUDSDK_CORE_PROJECT='credenciales-364703'
      GCLOUD_PATH='C:\\Users\\aevar\\AppData\\Local\\Google\\Cloud SDK\\google-cloud-sdk\\bin'     
  }

  stages {
      stage('validar gcloud'){
          steps {
              bat '"%GCLOUD_PATH%\\gcloud.cmd" --version'
          }
      }

    
      stage('Clone from github'){
          steps {
              checkout scmGit(branches: [[name: "*/main"]], extensions: [], userRemoteConfigs: [[credentialsId: 'AngelEvaristo', url: 'https://github.com/AngelEvaristo/sesion3-angular-deploy.git']])
          }
      }

    //   stage('Login azure'){
    //       steps {
    //           withCredentials([azureServicePrincipal('azure_SP')]) {
    //              sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
    //           } 
    //       }
    //   }      
    
    //   stage('validar SP Azure'){
    //       steps {
    //         azureCLI commands: [[exportVariablesString: '', script: 'az account show']], principalCredentialId: 'azure_SP'    
    //       }  
    //   }    
    
    //   stage('Install angular cli'){
    //       steps {
    //           script {
    //             echo "Instalando angular cli"
    //             bat """
    //                 npm install -g @angular/cli                    
    //             """                
    //           }
    //       }
    //   }
    //   stage('validar ng'){
    //       steps {
    //         bat 'ng version'
    //       }  
    //   }
    //   stage('instalar dependencias'){
    //       steps {
    //         bat 'npm install'
    //       }
    //   }
    
    //   stage('Compilacion'){
    //       steps {
    //         bat 'ng build --configuration production'
    //       }
    //   }  
    
    //   stage('chequeo y Empaquetado'){
    //       steps {
    //         bat 'powershell -Command "Compress-Archive -Path dist\\my-first-angular-app\\browser\\* -DestinationPath angular_app.zip -Force"'
    //         bat 'dir'
    //       }
    //   }     

      stage('auth gcloud'){
          steps {

            withCredentials([file(credentialsId: 'GCP_CRED', variable: 'GCP_CRED')]) {
                bat '"%GCLOUD_PATH%\\gcloud.cmd" auth activate-service-account --key-file="%GCP_CRED%" --project=%CLOUDSDK_CORE_PROJECT%'
            }              
          }
      }   
    
      stage('install service'){
          steps {
              bat '"%GCLOUD_PATH%\\gcloud.cmd" run services replace service.yaml --plaform managed --region us-east1'
          }
      }


      stage('Allow user service'){
          steps {
              bat '"%GCLOUD_PATH%\\gcloud.cmd" run services add-iam-policy-binding firstservice --region us-east1 --member allusers -role "roles/run.invoker" '
          }
      }
    //   stage('Deploy Azur WebAPP'){
    //       steps {
    //           withCredentials([azureServicePrincipal('azure_SP')]) {
    //               bat '''
    //                   az webapp deploy \
    //                   --resource-group sesion3-tecylab \
    //                   --name test-sesion3-tecylab \
    //                   --restart true \
    //                   --src-path angular_app.zip \
    //                   --type zip                                                          
    //               '''
    //           } 
    //       }
    //   }    
    //   stage('Deploy to S3'){
    //       steps {
    //         withAWS(credentials: 'AWS_CREDS', region: 'us-east-1') {
    //             bat '''
    //                 cd dist\\my-first-angular-app\\browser\\
    //                 dir
    //                 aws s3 rm s3://amzn-s3-s4-tecylab --recursive
    //                 aws s3 sync . s3://amzn-s3-s4-tecylab
    //                 aws s3 ls s3://amzn-s3-s4-tecylab
    //             '''

    //         } 
    //       }
    //   }          
    
  }  
}
