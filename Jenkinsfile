pipeline {
    agent {
        docker
        { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    parameters {
        choice(name: 'environment', choices: ['e2e', 'preprod', 'test1'], description: 'Chosissez une environment')
        booleanParam(name: 'selection', defaultValue: true, description: 'Lancer avec chaque test')
        booleanParam(name: 'tous', defaultValue: true, description: 'Lancer tous les test')
    }
    stages {
        stage('lancer le test') {
            steps {
                script {
                if(params.tous==true ){
                sh 'newman run collection.json'
                sh 'newman run collec1.json -e ./env/test1.json'
                sh 'newman run collec2.json -e ./env/e2e.json'
                sh 'newman run collec2.json -e ./env/preprod.json'
                }else if (params.selection){
                    sh 'newman run collection.json'
                }
                else{
                    switch(params.environment) {
                        case prod_env:
                            sh "newman run collec1.json -e ./env/${params.environment}.json"
                        break
                        default :
                            sh "newman run collec2.json -e./env/${params.environment}.json"
                            sh "newman run collec2.json -e./env/${params.environment}.json"
                        }
                    }        
                }
            }    
        }
    }
}