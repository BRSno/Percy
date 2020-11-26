pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        git(url: 'https://github.com/BRSno/Percy', branch: 'main', poll: true)
      }
    }

    stage('Test') {
      steps {
        SWEAGLEUpload(actionName: 'uploadProp', fileLocation: 'Components/Files/*.properties', format: 'properties', nodePath: 'Icarus', tag: '${BUILD_ID}', withSnapshot: true, filenameNodes: true)
        SWEAGLEUpload(actionName: 'uploadJSON', fileLocation: 'Components/Microservices/*.json', format: 'JSON', nodePath: 'Icarus', filenameNodes: true, tag: '${BUILD_ID}', withSnapshot: true)
      }
    }

    stage('Deploy') {
      steps {
        sleep 3
        echo 'Deploy complete'
      }
    }

  }
}