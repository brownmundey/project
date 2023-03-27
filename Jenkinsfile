pipeline {
	agent {
		label {
			label "built-in"
			customWorkspace "/root"
		}
	}
	stages {
		stage ("build-project") {
		steps {
			sh "mvn clean install"
			sh "cp -r /mnt/work/project/target/LoginWebApp.war /mnt/server/apache-tomcat-9.0.73/webapps
			sh "cd /mnt/server/apache-tomcat-9.0.73/bin | ./shutdown.sh"
			sh "sleep 5"
			sh "cd /mnt/server/apache-tomcat-9.0.73/bin | ./startup.sh"
		}
	}
	stage ("connect-database") {
		environment {
			DB_HOST = 'database-1.cnyclmchztvw.ap-south-1.rds.amazonaws.com'
			DB_PORT = '3306'
			DB_USER = 'admin'
			DB_PASS = 'himanshu'
		}
		steps {
			sh "yum install mysql -y"
			sh "mysql -h $DB_HOST -u $DB_USER -p $DB_PASS
		}
	}
}
}
