podTemplate(containers: [
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk14',
        command: 'sleep',
        args: '30d'
        ),
    ]) {
    node(POD_LABEL) {
        stage('Run pipeline against a gradle project') {
            git 'https://github.com/karthikkrish84/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
            container('gradle') {
                stage('Build a gradle project') {
                    sh '''
                    cd Chapter08/sample1
                    chmod +x gradlew
                    ./gradlew test
                    '''
                    }
                stage("Code coverage") {
                    sh '''
                    cd Chapter08/sample1
                    ./gradlew jacocoTestCoverageVerification
                    ./gradlew jacocoTestReport
                    '''
                    publishHTML (target: [
                        reportDir: 'Chapter08/sample1/build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ])
                }
                stage("jacoco checkstyle") {
                    try {
                    sh '''
                    cd Chapter08/sample1
                    ./gradlew checkstyleMain
                    ./gradlew jacocoTestReport
                    '''
                    } 
                    catch (err) {
                    echo "checkstyle failed"
                    }
                            publishHTML (target: [
                                reportDir: 'Chapter08/sample1/build/reports/checkstyle',
                                reportFiles: 'main.html',
                                reportName: "JaCoCo CheckStyle"
                            ])
                        }
            }
        }
    }
}