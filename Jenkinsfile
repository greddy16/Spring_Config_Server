#!/usr/bin/env groovy

@Library("com.optum.jenkins.pipeline.library@v0.3.0") _
  
def app
def dyabalurcredentials = "dyabalurcredentials"

def getEnvName(RESOURCE_GROUP) {
    if("test".equals(ENVIRONMENT_NAME)) {
    return "oeo-tst-us-c-rg";
    } else if ("stage".equals(ENVIRONMENT_NAME)) {
    return "oeo-stg-us-c-rg";
    } else if ("stage-east".equals(ENVIRONMENT_NAME)) {
    return "oeo-stg-us2-e-rg";
    } else if ("perf".equals(ENVIRONMENT_NAME)) {
    return "oeo-prf-us-c-rg";
    } else if ("prod".equals(ENVIRONMENT_NAME)) {
    return "oeo-prd-us-c-rg";
    } else if ("prod-east".equals(ENVIRONMENT_NAME)) {
    return "oeo-prd-us2-e-rg";
    } else {
    return "please set the environment and get resource group";
    break;
    }
}

def getModuleName(MODULE_NAME) {
    if("test".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-test";
    } else if ("stage".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-stage";
    } else if ("stage-east".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-stage-east";
    } else if ("perf".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-perf";
    } else if ("prod".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-prod";
    } else if ("prod-east".equals(ENVIRONMENT_NAME)) {
    return "spring-config-server-api-prod-east";
    } else {
    return "please set the environment and get module name";
    break;
    }
}

def getACR(DOCKER_REG_URL) {
    if("test".equals(ENVIRONMENT_NAME)) {
    return "https://oeotstuscacr.azurecr.io";
    } else if ("stage".equals(ENVIRONMENT_NAME)) {
    return "https://oeotstuscacr.azurecr.io";
    } else if ("stage-east".equals(ENVIRONMENT_NAME)) {
    return "https://oeostgus2eacr.azurecr.io";
    } else if ("perf".equals(ENVIRONMENT_NAME)) {
    return "https://oeotstuscacr.azurecr.io";
    } else if ("prod".equals(ENVIRONMENT_NAME)) {
    return "https://oeoprduscacr.azurecr.io";
    } else if ("prod-east".equals(ENVIRONMENT_NAME)) {
    return "https://oeoprdus2eacr.azurecr.io";
    } else {
    return "please set the environment and get container registry";
    break;
    }
}

def getClusterName(AKS_CLUSTER_NAME) {
    if("test".equals(ENVIRONMENT_NAME)) {
    return "oeo-tst-us-c-aks";
    } else if ("stage".equals(ENVIRONMENT_NAME)) {
    return "oeo-stg-us-c-vm-aks";
    } else if ("stage-east".equals(ENVIRONMENT_NAME)) {
    return "oeo-stg-us2-e-aks";
    } else if ("perf".equals(ENVIRONMENT_NAME)) {
    return "oeo-prf-us-c-aks";
    } else if ("prod".equals(ENVIRONMENT_NAME)) {
    return "oeo-prd-us-c-aks";
    } else if ("prod-east".equals(ENVIRONMENT_NAME)) {
    return "oeo-prd-us2-e-aks";
    } else {
    return "please set the environment and get cluster";
    break;
    }
}

def getConnACR(ACR_REG_ID) {
    if("test".equals(ENVIRONMENT_NAME) || "stage".equals(ENVIRONMENT_NAME) || "perf".equals(ENVIRONMENT_NAME)) {
    return "oeotstuscacr";
    } else if ("stage-east".equals(ENVIRONMENT_NAME)) {
    return "oeostgus2eacr";
    } else if ("prod".equals(ENVIRONMENT_NAME)) {
    return "oeoprduscacr";
    } else if ("prod-east".equals(ENVIRONMENT_NAME)) {
    return "oeoprdus2ecacr";
    } 
    else {
    return "please set the environment and get container registry ID";
    break;
    }
}

def getConnSP(ENV_SERVICE_PRINCIPAL_NAME) {
    if("test".equals(ENVIRONMENT_NAME) || "stage".equals(ENVIRONMENT_NAME) || "stage-east".equals(ENVIRONMENT_NAME) || "perf".equals(ENVIRONMENT_NAME)) {
    return "onb-nonprod-sp";
    } else if ("prod".equals(ENVIRONMENT_NAME) || "prod-east".equals(ENVIRONMENT_NAME)) {
    return "onb-prod-sp";
    } 
    else {
    return "please set the environment and get service principal";
    break;
    }
}

pipeline{
               
        agent {
            label 'docker-azcli-kubectl-slave'
        }
      
        tools{
            maven 'Maven'
            jdk 'Java 1.8'
        }
        environment {
            RESOURCE_GROUP                  = getEnvName(env.ENVIRONMENT_NAME)
            MODULE_NAME                     = getModuleName(MODULE_NAME)
            CLUSTER_NAME                    = getClusterName(env.ENVIRONMENT_NAME)
            ENV_SERVICE_PRINCIPAL_NAME      = getConnSP(env.ENVIRONMENT_NAME)
            ACR_REG_ID                      = getConnACR(env.ENVIRONMENT_NAME)
            DOCKER_REG_URL                  = getACR(env.ENVIRONMENT_NAME)
        }

        parameters {
            choice(name: 'ENVIRONMENT_NAME', choices: ['test','stage','stage-east','perf','prod','prod-east'], description: 'Provide the branch name to build')
            //choice(name: 'BRANCH_NAME', choices: ['develop','release'], description: 'Provide the branch name to build')
            //string(name: 'VERSION', defaultValue: 'v1.0', description: 'Provide the version number to build', trim: false)
            //choice(name: 'FUNCTION_APP', choices: ['pn-tst-us-c-azfun','pn-stg-us-c-azfun','pn-prf-us-c-azfun','pn-prd-us-c-azfun'], description: 'Provide the branch name to build')
        }

        stages{
            stage('Build details') {
                steps {
                    echo '########################################################'
                    //echo 'BRANCH                : ' + env.BRANCH_NAME
                    echo 'ENVITONMENT           : ' + env.ENVIRONMENT_NAME
                    echo 'AKS CLUSTER           : ' + env.CLUSTER_NAME
                    echo 'RESOURCE GROUP NAME   : ' + env.RESOURCE_GROUP
                    echo 'ACR ID                : ' + env.ACR_REG_ID
                    echo '########################################################'
                }
            }
            stage('env configure') {
                steps{
                    sh '''
                    echo "PATH = ${PATH}"
                    echo "MAVEN_HOME = ${MAVEN_HOME}"
                    '''
                }
            }

            stage('checkout') {
              steps{
                script{
                  checkout scm
                    }
                }
            }

            stage('spring-config-server-api') {
                steps {
                    script {
                        sh "mvn clean package -DskipTests"
                    }
                }
            }
    
            stage ('Proceeding with SonarQube Scan ') {
                steps {
                    script {
                        if ( params.ENVIRONMENT_NAME == 'test') {
                        sh '''
                        mvn compile org.jacoco:jacoco-maven-plugin:0.8.5:prepare-agent install -Dmaven.test.failure.ignore=true
    				    mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.6.0.1398:sonar -Dsonar.host.url=https://sonar.optum.com -Dsonar.login=2807583897b7a398a92adc74becf170f3c2b3a16 -e
                        '''
                        }
                    }
                }
            }

            stage('Fortify') {
                steps {
                    script {
                        if ( params.ENVIRONMENT_NAME == 'test') {
                        glFortifyScan fortifyBuildName: "HDENROLLPREFERENCES",
                        scarProjectName: "HDENROLLPREFERENCES",
                        fortifyJdkVersion: "1.8",
                        scarProjectVersion: "23315",
                        sourceDirectory: "sourceDirectory",
                        fortifyTranslateExclusions:"-exclude '**/target/**/*', -exclude '**/resources/*.*'",
                        uploadToScar: false,
                        criticalThreshold: 50,
                        highThreshold: 50,
                        scarCredentialsId: "dyabalurcredentials",
                        wait: true,
                        downloadFromScar: false,
                        isVerbose: true
                        }
                    } 
                }
            }
    

            stage('Registring image and Docker image Build'){
               steps {
                    script {
                        app = docker.build("${MODULE_NAME}")
                    }
                }
            }

            stage('Push image to ACR with buildno tag'){
                steps {
                    script {
                        //You would need to first register with ACR before you can push images to your account
 
                        docker.withRegistry(env.DOCKER_REG_URL, env.ACR_REG_ID) {
                        app.push("latest")
                        app.push("${env.BUILD_NUMBER}")
                        }
                    }
                }
            }
 
            stage ('ACR deployment in AKS cluster') {
                steps {
                    script {
                        withCredentials([azureServicePrincipal(env.ENV_SERVICE_PRINCIPAL_NAME)]) {
                        sh '''
                        . /etc/profile.d/jenkins.sh
                        az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                        az account list --output table
                        az aks get-credentials --resource-group ${RESOURCE_GROUP} --name ${CLUSTER_NAME}

                        MODULE_NAME=${MODULE_NAME}
                        KUBERNETES_NAMESPACE=${ENVIRONMENT_NAME}
                        #ACR_HUB=${DOCKER_REG_URL}
                        #ACR_USER=${ACR_REG_ID}
                        SERVICE_NAME=${MODULE_NAME}-svc
                        DEPLOYMENT_NAME=${MODULE_NAME}-dep
                        # Create or update the deployment
                        echo $(kubectl --namespace=$KUBERNETES_NAMESPACE get deployments) > deployments_list.txt
                        if grep "$DEPLOYMENT_NAME" deployments_list.txt
                        then
                            echo "********************************************************************************************************"
                            echo "Deployment $DEPLOYMENT_NAME is available. Updating now."
                            echo "********************************************************************************************************"
                            #kubectl --namespace=$KUBERNETES_NAMESPACE patch deployment ${DEPLOYMENT_NAME} -p "{\"spec\":{\"template\":{\"spec\":{\"containers\":[{\"name\":\"$MODULE_NAME\",\"image\":\"orxhemiacrreg.azurecr.io/$MODULE_NAME:$BUILD_NUMBER\"}]}}}}"
                            sed "s/latest/$BUILD_NUMBER/g" $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy.yml > $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy-$BUILD_NUMBER.yml
                            cat $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy-$BUILD_NUMBER.yml
                            kubectl --namespace=$KUBERNETES_NAMESPACE apply -f $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy-$BUILD_NUMBER.yml

                            #Create hpa.yml  
                            #cat $WORKSPACE/deployment/$ENVIRONMENT_NAME/hpa.yml
                            #kubectl --namespace=$KUBERNETES_NAMESPACE --kubeconfig config apply -f $WORKSPACE/deployment/$ENVIRONMENT_NAME/hpa.yml
                    
                        else
                            echo "********************************************************************************************************"
                            echo "Deployment $DEPLOYMENT_NAME is NOT available. Deploying now."
                            echo "********************************************************************************************************"
                        
                            #sed "s/latest/$BUILD_NUMBER/g" $WORKSPACE/deploy.yml > $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy-$BUILD_NUMBER.yml
                            cat $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy.yml
                            kubectl --namespace=$KUBERNETES_NAMESPACE create -f $WORKSPACE/deployment/$ENVIRONMENT_NAME/deploy.yml

                            #Create hpa.yml  
                            #cat $WORKSPACE/deployment/$ENVIRONMENT_NAME/hpa.yml
                            #kubectl --namespace=$KUBERNETES_NAMESPACE --kubeconfig config apply -f $WORKSPACE/deployment/$ENVIRONMENT_NAME/hpa.yml
                       
                            docker rmi -f ${MODULE_NAME}:latest
                        fi

                        #========================================================================= 
                        # Create or update the service
                        echo $(kubectl --namespace=$KUBERNETES_NAMESPACE get services) > services_list.txt
                        if grep "$SERVICE_NAME" services_list.txt
                        then
                            echo "********************************************************************************************************"
                            echo "Service $SERVICE_NAME already exists. No need to create again."
                            echo "********************************************************************************************************"
                        else
                            echo "********************************************************************************************************"
                            echo "Service $SERVICE_NAME is NOT deployed. Deploying now."
                            echo "********************************************************************************************************"
                            cat $WORKSPACE/deployment/$ENVIRONMENT_NAME/service.yml
                            kubectl --namespace=$KUBERNETES_NAMESPACE create -f $WORKSPACE/deployment/$ENVIRONMENT_NAME/service.yml
                        fi
                        '''
                    }
                }
            }
        }
    }
}
