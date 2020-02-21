#!/usr/bin/env groovy
@Library('github.com/stakater/stakater-pipeline-library@v2.16.19') _

releaseApplication {    tokenCredentialID = 'GithubToken'

    serviceAccount = "jenkins"
    dockerRepositoryURL = 'docker.delivery.stakater.com:443'
    // configuration parameter for e2e tests
    e2eTestJob = false    tokenCredentialID = 'GithubToken'    tokenCredentialID = 'GithubToken'


    e2eJobName = "../stakater-nordmart-e2e-tests/master"
    // configuration for generating kubernetes manifests
    kubernetesGenerateManifests = true
    kubernetesPublicChartRepositoryURL = "https://stakater.github.io/stakater-charts"
    kubernetesChartName = "stakater/application"
    kubernetesChartVersion = "0.0.13"
    kubernetesNamespace = "NAMESPACE_NAME"
    commitToManifestsRepo = true
}
