pipeline {
    agent none

    environment {
        PROJECT_ZONE = "${JENK_INT_IT_ZONE}"
        PROJECT_ID = "${JENK_INT_IT_PROJECT_ID}"
        STAGING_CLUSTER = "${JENK_INT_IT_STAGING}"
        PROD_CLUSTER = "${JENK_INT_IT_PROD}"
        BUILD_CONTEXT_BUCKET = "${JENK_INT_IT_BUCKET}"
        BUILD_CONTEXT = "build-context-${BUILD_ID}.tar.gz"
        APP_NAME = "drupal_deploy_lab"
        GCR_IMAGE = "gcr.io/${PROJECT_ID}/${APP_NAME}:${BUILD_ID}"
    }

    stages {
        stage("Build and test") {
	    agent {
    	    	kubernetes {
      		    cloud 'kubernetes'
      		    label 'drupal-pod'
      		    yamlFile 'jenkins/drupal-pod.yaml'
		}
	    }
	    steps {
	    	container('maven') {
                    dir("gke") {
                    }
		}
	    }
	}
    }
}
