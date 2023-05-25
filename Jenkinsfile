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
      stage ("clean") {
		  steps {
		    sh "sudo rm -rf *"
		  }
		}
		stage ("tomcat") {
			steps {
				
				sh "sudo docker system prune -a -f"
				sh "sudo docker-compose up -d"
				
			}
		}
		stage ("build") {
		  steps {
			
		    sh "sudo mvn clean install"
			
		  }
		}
	    stage ("deploy") {
			steps {
				sh "sudo docker cp /mnt/weblight/target/LoginWebApp.war project-server-1:/usr/local/tomcat/webapps"
			    sh "sudo docker stop project-server-1"
				sh "sudo docker start project-server-1"
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
		    sh "sudo rm -rf *"
		    sh "sudo git clone https://github.com/brownmundey/my-own.git"
		  }
	}
	stage ("container") {
	      agent {
	        label {
	            label "database"
	            customWorkspace "/mnt/database/my-own"
	        }
	        }
			steps {
			 sh "sudo docker-compose down"
			 sh "sudo docker system prune -a -f"
			 sh "sudo docker-compose up -d"
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
