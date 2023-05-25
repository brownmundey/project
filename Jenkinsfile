pipeline {
  agent {
	label {
		label "remote"
		customWorkspace "/mnt/weblight"
	}
  }
  environment {
		work = "/mnt/weblight"
		database = "/mnt/database"
  }
  stages {
  stage ("shutdown-1") {
	steps {
		sh "cd ${work} && sudo docker-compose down -v"
		sh "sudo docker system prune -a -f"
	}
	}
	stage ("shutdown-2") {
	      agent {
	        label {
	            label "database"
	            customWorkspace "/mnt/database"
	        }
	        }
			steps {
				sh "cd ${database} && sudo docker-compose down -v"
		        sh "sudo docker system prune -a -f"
			}
			}
		stage ("tomcat") {
			steps {
				
				sh "sudo docker system prune -a -f"
				sh "sudo docker-compose up -d"
				sh "sudo docker stop weblight-database-1"
				
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
			 sh "sudo docker system prune -a -f"
			 sh "sudo docker-compose up -d"
			 sh "sudo docker stop database-server-1"
			}
	      }
	      
}	
}
