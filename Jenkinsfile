/* Generated from templates/gitops-template/Jenkinsfile.njk. Do not edit directly. */

library identifier: 'RHTAP_Jenkins@main', retriever: modernSCM(
  [$class: 'GitSCMSource',
   remote: 'https://github.com/redhat-appstudio/tssc-sample-jenkins.git'])

pipeline {
    agent any
    environment {
        /* Not used but init.sh will fail if they're missing */
        COSIGN_SECRET_PASSWORD = 'dummy'
        COSIGN_SECRET_KEY = 'dummy'
        /* Used to verify the image signature and attestation */
        COSIGN_PUBLIC_KEY = credentials('COSIGN_PUBLIC_KEY')
        /* URL of the BOMbastic api host (e.g. https://sbom.trustification.dev) */
        TRUSTIFICATION_BOMBASTIC_API_URL = credentials('TRUSTIFICATION_BOMBASTIC_API_URL')
        /* URL of the OIDC token issuer (e.g. https://sso.trustification.dev/realms/chicken) */
        TRUSTIFICATION_OIDC_ISSUER_URL = credentials('TRUSTIFICATION_OIDC_ISSUER_URL')
        TRUSTIFICATION_OIDC_CLIENT_ID = credentials('TRUSTIFICATION_OIDC_CLIENT_ID')
        TRUSTIFICATION_OIDC_CLIENT_SECRET = credentials('TRUSTIFICATION_OIDC_CLIENT_SECRET')
        TRUSTIFICATION_SUPPORTED_CYCLONEDX_VERSION = credentials('TRUSTIFICATION_SUPPORTED_CYCLONEDX_VERSION')
    }
    stages {
        stage('Verify EC') {
            steps {
                script {
                    rhtap.info('gather_deploy_images')
                    rhtap.gather_deploy_images()
                    rhtap.info('verify_enterprise_contract')
                    rhtap.verify_enterprise_contract()
                }
            }
        }

        stage('Upload SBOM') {
            steps {
                script {
                    rhtap.info('gather_images_to_upload_sbom')
                    rhtap.gather_images_to_upload_sbom()
                    rhtap.info('download_sbom_from_url_in_attestation')
                    rhtap.download_sbom_from_url_in_attestation()
                    rhtap.info('upload_sbom_to_trustification')
                    rhtap.upload_sbom_to_trustification()
                }
            }
        }

    }
}
