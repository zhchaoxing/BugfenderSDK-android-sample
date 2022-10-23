pipeline {
  agent { label 'linux && android' } 
  stages {
    stage('Dependencies') {
		/*
                sh 'sudo npm install -g react-native-cli'
                sh 'npm install'
                sh 'react-native link'
                sh 'export JAVA_HOME=/opt/jdk1.8.0_201'
                sh 'export JRE_HOME=/opt/jdk1.8.0_201/jre'
                sh 'export PATH=$PATH:/opt/jdk1.8.0_201/bin:/opt/jdk1.8.0_201/jre/bin'
        */
                //sh 'export ANDROID_HOME=/home/newhab/android_sdk'
				sh 'echo ANDROID_HOME: $ANDROID_HOME'
				sh 'echo JRE_HOME: $JRE_HOME'
				sh 'echo JAVA_HOME：$JAVA_HOME'
        
    }

    stage('Checkout Scm') {
      steps {
        git 'https://github.com/bugfender/BugfenderSDK-android-sample.git'
      }
    }

    stage('Wait for android emulator') {
      steps {
        sh '''ANDROID_SERIAL=emulator-5556

        # wait for emulator to be up and fully booted, unlock screen

        $ANDROID_HOME/platform-tools/adb kill-server

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