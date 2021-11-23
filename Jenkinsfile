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
        stage("Deploy to prod") {
            agent {
    	        kubernetes {
      		    cloud 'kubernetes'
      		    label 'gke-deploy'
		    yamlFile 'gke/jenkins/gke-deploy-pod.yaml'
		}
            }
	    steps{
		container('gke-deploy') {
		    sh "sed -i s#IMAGE#${GCR_IMAGE}#g gke/kubernetes/manifest.yaml"
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.STAGING_CLUSTER, location: env.PROJECT_ZONE, manifestPattern: 'deploy/manifest.yaml', credentialsId: env.JENK_INT_IT_CRED_ID, verifyDeployments: true])
		}
	     }
	}
    }
}
