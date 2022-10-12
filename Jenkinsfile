pipeline {
    agent any
    tools { 
        maven 'Maven' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
	    
	    stage ('Check-Git-Secrets') {
		    steps {
	        sh 'rm trufflehog || true'
		sh 'docker pull gesellix/trufflehog'
		sh 'docker run -t gesellix/trufflehog --json https://github.com/filestack/webapp.git > trufflehog'
		sh 'cat trufflehog'
	    }
	    }
	    
	     stage ('SSL Checks') {
		    steps {
			sh 'pip install sslyze==1.4.2'
			sh 'python -m sslyze --regular 54.86.226.84:8080 --json_out sslyze-output.json'
			sh 'cat sslyze-output.json'
		    }
	    }
	    
stage ('Upload Reports to Defect Dojo') {
		    steps {
			sh 'pip install requests'
			sh 'wget https://raw.githubusercontent.com/filestack/webapp/master/upload-results.py'
			sh 'chmod +x upload-results.py'
			sh 'python upload-results.py --host http://localhost:8000 --api_key b4e1b9a5f80cc8d96363b515f039170c2aa222db --engagement_id 1 --result_file trufflehog --username admin --scanner "SSL Labs Scan"'
			
		    }
	    }

    }
}
