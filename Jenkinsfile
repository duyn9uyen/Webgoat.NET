node {
   stage('Clone Repo') {
        echo 'Clone Repo'
        git branch: 'vs2017-with-vulnerable-packages',
        url: 'https://github.com/duyn9uyen/WebGoat.NET.git'
   }
   stage('Restore 3rd party dependencies') {
       bat label: '', script: '"c:\\nuget\\nuget.exe" restore "WebGoat.NET.sln"'
   }
   stage('Running WhiteSource scan') {
        echo 'Downloading WhiteSource Jar'
        bat 'powershell start-bitstransfer "https://github.com/whitesource/unified-agent-distribution/raw/master/standAlone/wss-unified-agent.jar" wss-unified-agent.jar'
	    echo 'Running WhiteSource scan'
	    withCredentials([string(credentialsId: 'ws-api-key', variable: 'APIKEY')]) {
		   bat "java -jar wss-unified-agent.jar -c whitesource-fs-agent.config -apiKey ${APIKEY} -product DN-Testing -project Webgoat.NET"
	    }
	   archiveArtifacts 'whitesource/*'
   }
}
