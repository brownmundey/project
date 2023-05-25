pipeline {
  agent {
	label {
		label "remote"
		customWorkspace "/mnt/weblight"
	}
  }
  environment {
		work = "/mnt/weblight"
  }
  stages {
		stage ("tomcat") {
			steps {
				
				sh "sudo docker system prune -a -f"
				sh "sudo docker-compose up -d"
				sh "sudo docker stop weblight-server-2"
				
			}
		}
		stage ("build") {
		  steps {
			
		    sh "sudo mvn clean install"
			
		  }
		}
	    stage ("deploy") {
			steps {
				sh "sudo docker cp /mnt/weblight/target/LoginWebApp.war weblight-server-1:/usr/local/tomcat/webapps"
			    sh "sudo docker stop weblight-server-1"
				sh "sudo docker start weblight-server-1"
			}	
    }
	stage ("database") {
	      agent {
	        label {
	            label "database"
	            customWorkspace "/mnt/database"
	        }
	        }
			steps {
			 sh "sudo docker-compose up -d"
			 sh "sudo docker stop weblight-server-1"
			}
	      }
	      stage ("shutdown") {
	steps {
		sh "cd ${work} && sudo docker-compose down -v"
		sh "sudo docker system prune -a -f"
	}
}	
}
}
