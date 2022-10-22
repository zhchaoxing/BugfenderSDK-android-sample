pipeline {
  agent any
  stages {
    stage('Checkout Scm') {
      steps {
        git 'https://github.com/bugfender/BugfenderSDK-android-sample.git'
      }
    }


    stage('Shell script 0') {
      steps {
        sh '''ANDROID_SERIAL=emulator-5556

        # wait for emulator to be up and fully booted, unlock screen

        $ANDROID_HOME/platform-tools/adb wait-for-device shell \'while [[ -z $(getprop sys.boot_completed) ]]; do sleep 1; done; input keyevent 82\'

        '''
      }
    }


    stage('Gradle Build') {
      steps {
        sh 'gradle clean build'
        sh './gradlew connectedAndroidTest assembleDebug '
      }
    }

    

  }
}