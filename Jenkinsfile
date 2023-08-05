pipeline {
    agent any
    parameters {
            string(name: 'param_1', defaultValue: 'Jenkins', description: 'username')

            string(name: 'param_2', defaultValue: 'Jenkins', description: 'username')
    
            text(name: 'desc', defaultValue: '', description: 'description about the user. ')
    
            booleanParam(name: 'toggle', defaultValue: true, description: 'Enable/Disable')
    
            choice(name: 'choice', choices: [1,2,3,4,5], description: 'choose a value')
    
            password(name: 'passwd', defaultValue: 'secret', description: 'provide password')
        }
    environment {
        //DOCKER_ID = credentials('DOCKER_ID')
        //DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        AWS_DEFAULT_REGION="us-east-1"
        //THE_BUTLER_SAYS_SO=credentials('cmb-aws-cred')
        BUILD_ID = "${env.BUILD_ID}"
        
        PARAMETERS_DEV_FILE = "param.json"
    }

    stages {
        stage('Init') {
            steps {
                echo 'Initializing..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "Current branch: ${env.GIT_BRANCH}"
                echo "Current branch: ${env.GIT_TAG}"
                echo "Param1: ${params.param_1} ${env.param_1}"
                echo "Param2: ${params.param_2} ${env.param_2}"
                echo "Param2: ${params.param_2} ${env.param_2}"
                //sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_ID --password-stdin'
            }
        }
        stage('AWS') {           
            steps {
                echo 'AWS command..'
                sh '''
                  aws --version
                '''
            }
        }
        stage("Determine new version") {
            steps {
                script {
                   GIT_COMMIT_EMAIL = sh (
                        script: 'git --no-pager show -s --format=\'%ae\'',
                        returnStdout: true
                    ).trim()
                    echo "Git committer email: ${GIT_COMMIT_EMAIL}"
                    GIT_TAG = sh (script: 'git tag --sort version:refname | tail -1', returnStdout: true).trim()
                    echo "Git tag: ${GIT_TAG}"
                }                 
            }
        }
        stage('Show Files') {
            environment {
              MY_FILES = sh(script: 'cd .github/workflows && ls -l', returnStdout: true)
            }
            steps {
              sh '''
                echo "$MY_FILES"
              '''
            }
        }        
/*         
        stage('Teste') {           
            steps {                
                script {
                    MYTAG = sh (returnStdout: true, script: 'git tag --sort version:refname | tail -1').trim()
                    echo ${MYTAG}
                }
            }
        }             
        stage ('push artifact') {
            steps {
                    // sh 'zip -r index-${BUILD_ID}.zip src'
                    sh 'zip index-${BUILD_ID}.zip index.js'
                    sh 'aws s3 cp $WORKSPACE/index-${BUILD_ID}.zip s3://create-lambda-from-zip-file/deploy/'
                    // archiveArtifacts artifacts: 'index-${BUILD_ID}.zip', fingerprint: true
            }
        }
*/        
//        stage('new pull artifact new') {
//            steps {                
//                    copyArtifacts filter: 'test13.zip', fingerprintArtifacts: true, projectName: env.JOB_NAME, selector: specific(env.BUILD_NUMBER)
//                //  unzip zipFile: 'test11.zip', dir: './archive_new'
//                    sh 'unzip test13.zip -d ./archive_new'
//                    sh 'cat archive_new/test13.txt'                
//            }
//        }
/*      
        stage('Create Change') {
            steps {
                echo 'Removing unused docker containers and images..'
                // sh "aws cloudformation create-stack --stack-name s3bucket --template-body file://simplests3cft.json --region 'us-east-1'"
                // sh 'aws cloudformation create-stack --stack-name lambdafroms3bucket --region us-east-1 --template-body file://lambda.yml --parameters file://param.json --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM '
                // sh 'aws cloudformation create-stack --stack-name LambdaFromBucket --region us-east-1 --template-body file://lambda.yaml --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM '
                // sh 'aws cloudformation create-change-set --stack-name LambdaFromBucket --change-set-name LambdaFromBucket-${BUILD_ID} --region us-east-1 --template-body file://lambda.yaml --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM '
                
                echo "Updating parameters file..."
                sh (""" sed -i 's/index.zip/index-${BUILD_ID}.zip/' ${PARAMETERS_DEV_FILE} """)
                sh (""" cat ${PARAMETERS_DEV_FILE} """)
                
                sh '/bin/bash ./create-stack.sh LambdaFromBucket LambdaFromBucket-${BUILD_ID} lambda.yaml ${PARAMETERS_DEV_FILE}'
            }
        }
        stage('Execute Change') {
            steps {
                echo 'Removing unused docker containers and images..'
                sh 'sleep 30'

                sh '/bin/bash ./execute-change.sh LambdaFromBucket LambdaFromBucket-${BUILD_ID}'
            }
        }
*/        
    }
}
