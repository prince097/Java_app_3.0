@Library('my-shared-library') _

pipeline{

    agent any
    //agent { label 'Demo' }

    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'ImageName', description: "name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'prince009977')
    }

    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/prince097/Java_app_3.0.git"
            )
            }
        }
         stage('Unit Test maven'){
         
         when { expression {  params.action == 'create' } }

            steps{
               script{
                   mvnTest()
               }
            }
        }
         stage('Integration Test maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnIntegrationTest()
               }
            }
        }
       //  stage('Static code analysis: Sonarqube'){
       //   when { expression {  params.action == 'create' } }
       //      steps{
       //         script{
                   
       //             def SonarQubecredentialsId = 'sonarqube-api'
       //             statiCodeAnalysis(SonarQubecredentialsId)
       //         }
       //      }
       // }
       // stage('Quality Gate Status Check : Sonarqube'){
       //   when { expression {  params.action == 'create' } }
       //      steps{
       //         script{
                   
       //             def SonarQubecredentialsId = 'sonarqube-api'
       //             QualityGateStatus(SonarQubecredentialsId)
       //         }
       //      }
       // }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
stage('Jfrog') {
    when { expression { params.action == 'create' } }
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'jfrog', passwordVariable: 'ARTIFACTORY_PASSWORD', usernameVariable: 'ARTIFACTORY_USERNAME')]) {
                sh '''
                    jfrog rt u kubernetes-configmap-reload-0.0.1-SNAPSHOT.jar prince-java-app --url http://65.2.11.4:8082/artifactory/ --user $ARTIFACTORY_USERNAME --password $ARTIFACTORY_PASSWORD
                '''
            }
        }
    }
}
//         stage('Docker Image Build'){
//          when { expression {  params.action == 'create' } }
//             steps{
//                script{
//                    // sh "sudo usermod -aG docker $praveensingam1994"
//                    // sh "setfacl -m user:root:rw /var/run/docker.sock"
//                    // sh "systemctl restart docker"
//                    dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
//                }
//             }
//         }
//          stage('Docker Image Scan: trivy '){
//          when { expression {  params.action == 'create' } }
//             steps{
//                script{
                   
//                    dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
//                }
//             }
//         }
//         stage('Docker Image Push : DockerHub '){
//          when { expression {  params.action == 'create' } }
//             steps{
//                script{
                   
//                    dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
//                }
//             }
//         }   
//         stage('Docker Image Cleanup : DockerHub '){
//          when { expression {  params.action == 'create' } }
//             steps{
//                script{
                   
//                    dockerImageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
//                }
//             }
//         }      
//     }
// }
