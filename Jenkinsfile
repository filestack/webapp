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
	    
	    stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }   

 
	 
	    
	stage ('Upload Reports to Defect Dojo') {
		    steps {
			sh 'pip install requests'
			sh 'wget https://raw.githubusercontent.com/filestack/webapp/master/upload-results.py'
			sh 'chmod +x upload-results.py'
			sh 'python3 upload-results.py --host localhost:8000 --username admin --api_key b4e1b9a5f80cc8d96363b515f039170c2aa222db --scanner "SSL Labs Scan" --result_file trufflehog --engagement_id 4'
			
		    }
	    }

    }
}
