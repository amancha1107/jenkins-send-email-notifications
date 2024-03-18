# jenkins-send-email-notifications

Add below port to jenkins server

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/8bf16b3c-0335-4c7a-a98f-512affcf563f)

- we need to add app password inside our gmail, so that jenkins will use this app password and communicates with our gmail
- goto gmail -> gmail security

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/6ea42b71-16cc-4307-a6b9-7faf53e0f3b0)

- verify yourself

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/7df14c6c-82b6-4266-bd87-d86e25275b99)

- add one app password for jenkins server

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/4a03498b-5e0c-493a-9f6f-9a7c499e6d29)

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/a2877df9-128a-496e-9938-0a4344d5f84b)

- you will get a app password, save it and configure this password in jenkins credential manager

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/62a73468-4f96-4377-962c-8d36903822a0)

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/ec5b8f11-ae6d-4bb9-b48c-dc7df5305e0e)


### configure E-mail Notification and Extended E-mail Notification 

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/7482d1a8-4d1a-41c0-bcca-7fa0f6d980ba)

- for both, add port 465

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/e372dd12-4202-4357-bb34-fc6812e06407)

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/79fd7e88-f182-4264-9a7f-3afe5942f663)

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/817519ff-a816-4486-b961-9327ee47f203)

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/c39f03c4-718a-439c-9146-eb2f90512b3d)


- here use gmail app password, not your actual gmail password

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/58630a63-c2c5-4214-a66d-b0ee2a09bc74)

- if everything is configured correctly, then you can test email

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/51a223c2-1114-4cbd-90e8-b9091672e7a0)


- Failure

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/3c8c7160-c784-4af7-a386-c7c7d757fdf7)

- Success

![image](https://github.com/vijay2181/jenkins-send-email-notifications/assets/66196388/5cad92df-ae3d-478b-a7af-7c1c8d1d8370)


## Pipeline

```
pipeline {
    agent any

    environment {
        MAVEN = "${tool 'MAVEN3.9.5'}/bin/mvn"
    }

    stages {
       stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/vijay2181/java-maven-SampleWarApp.git'
            }
        }
        stage('build maven project'){
            steps{
                sh '$MAVEN clean package'
            }
        }
    }

    post {
        always {
            script {
                def color
                switch (currentBuild.result) {
                    case 'SUCCESS':
                        color = '#008000' // Green
                        break
                    case 'FAILURE':
                        color = '#FF0000' // Red
                        break
                    default:
                        color = '#808080' // Grey
                }
                emailext (
                    subject: "Pipeline ${currentBuild.result}",
                    body: "<html><body><h2 style='color: ${color};'>Pipeline ${currentBuild.result}</h2><p>Some content about pipeline.</p><img src='https://example.com/image.png' alt='Image'></body></html>",
                    to: "dvijay11@gmail.com",
                    from: 'jenkins@gmail.com',
                    mimeType: 'text/html',
                    attachmentsPattern: '.log'
                )
            }
        }
    }
}

```
















