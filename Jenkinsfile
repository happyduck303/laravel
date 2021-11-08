pipeline {
  agent { label 'master' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  stages {
    stage('Build Image') {
        steps {
            script {
                if (env.BRANCH_NAME == 'development') {
            sh '''
                docker login -u happyduck -p Blank132!
                docker build . -t happyduck:1.$BUILD_NUMBER-development
'''
                }
                else if (env.BRANCH_NAME == 'staging') {
            sh '''
                docker login -u happyduck -p Blank132!
                docker build . -t happyduck:1.$BUILD_NUMBER-staging
                    '''

                }
                else if (env.BRANCH_NAME == 'production') {
                    sh ''
                }
                else {
                    sh 'echo Nothing to Build'
                }
            }
        }
    }
    stage('Push Image') {
        steps {
            script {
                if (env.BRANCH_NAME == 'development') {
            sh '''
                docker push happyduck/laravel:1.$BUILD_NUMBER-development
        '''
                }
                else if (env.BRANCH_NAME == 'staging') {
            sh '''
                docker push happyduck/laravel:1.$BUILD_NUMBER-staging
                    '''
                }
                else if (env.BRANCH_NAME == 'production') {
                    sh ''
                }
                else {
                    sh 'echo Nothing to Build'
                }
            }
        }
    }
    stage('Deploy Image') {
        steps {
            script {
                if (env.BRANCH_NAME == 'development') {
            sh '''
                docker pull happyduck/laravel:1.$BUILD_NUMBER-development
                docker rm -f laravel
                docker run -dit --name laravel -p 80:8000 happyduck/laravel:1.$BUILD_NUMBER-development
        '''
                }
                else if (env.BRANCH_NAME == 'staging') {
            sh '''
                docker pull happyduck/laravel:1.$BUILD_NUMBER-staging
                docker rm -f laravel
                docker run -dit --name laravel -p 80:8000 happyduck/laravel:1.$BUILD_NUMBER-staging
                    '''
                }
                else if (env.BRANCH_NAME == 'production') {
                    sh ''
                }
                else {
                    sh 'echo Nothing to Build'
                }
            }
        }
    }
}
}
