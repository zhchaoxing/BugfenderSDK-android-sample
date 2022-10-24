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

      steps{
        //sh 'export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64'
        //sh 'export JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre'
        //sh 'export PATH=$PATH:/opt/jdk1.8.0_201/bin:/opt/jdk1.8.0_201/jre/bin'
        sh '''
        $ANDROID_HOME/platform-tools/adb kill-server

        # Disable exit on non 0
        set +e

        kill $(ps aux | grep '[e]mulator' | awk '{print $2}')

        # Enable exit on non 0
        set -e

        ''' 
				sh 'echo ANDROID_HOME: $ANDROID_HOME'
				sh 'echo JRE_HOME: $JRE_HOME'
				sh 'echo JAVA_HOME：$JAVA_HOME'
        sh '$ANDROID_HOME/emulator/emulator -avd Android25 -noaudio -no-window -no-boot-anim -accel on -ports 5556,5557 &'
      }
    }

    stage('Checkout Scm') {
      steps {
        git 'https://github.com/zhchaoxing/BugfenderSDK-android-sample.git'
      }
    }

    stage('Wait for android emulator') {
      steps {
        sh '''
        ANDROID_SERIAL=emulator-5556

        # wait for emulator to be up and fully booted, unlock screen

        

        $ANDROID_HOME/platform-tools/adb wait-for-device shell \'while [[ -z $(getprop sys.boot_completed) ]]; do sleep 1; done; input keyevent 82\'

        '''
      }
    }


    stage('Gradle Build') {
      steps {
        //sh './gradlew clean build'
        sh './gradlew connectedAndroidTest assembleDebug '
      }
    }

    stage('Gradle Test') {
      steps {
        //sh './gradlew clean build'
        sh './gradlew connectedAndroidTest test '
      }
    }

    stage('Kill Emulator') {
      steps {
       sh '''
        # Disable exit on non 0
        set +e

        kill $(ps aux | grep '[e]mulator' | awk '{print $2}')

        # Enable exit on non 0
        set -e

        ''' 
      }
    }

  }
}