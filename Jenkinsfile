pipeline {
  agent {
	label {
		label "remote"
		customWorkspace "/mnt/weblight"
	}
  }
  environment {
		work = "/mnt/weblight/project"
  }
  stages {
      stage ("clone") {
		  steps {
		    sh "sudo rm -rf *"
		    sh "sudo git clone https://github.com/brownmundey/project.git"
		  }
		}
		stage ("database-tomcat") {
			steps {
				dir ("/mnt/weblight/project") {
				sh "sudo docker system prune -a -f"
				sh "sudo docker-compose up -d"
				}
			}
		}
		stage ("build") {
		  steps {
			dir ("/mnt/weblight/project") {
		    sh "sudo mvn clean install"
			}
		  }
		}
	    stage ("deploy") {
			steps {
				sh "sudo docker cp /mnt/weblight/project/target/LoginWebApp.war project-server-1:/usr/local/tomcat/webapps"
			    sh "sudo docker stop project-server-1"
				sh "sudo docker start project-server-1"
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
