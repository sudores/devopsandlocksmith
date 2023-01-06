pipeline {
    //agent { docker { image "alpine:latest"}}
    agent none
    environment {
       vpn   = "http://blog.ttk.loc"
       tor   = "http://q6ft5vxol7xt4az7ibmub3xsyyqqsbmm35g7xcnhoijpulez33zupwad.onion/"
       clear = "https://gzttk.org/"
    } 
    stages {
        stage("Build") {
            agent { docker { image "alpine:latest"}}
            steps {
                script {
                    sh("apk add --no-cache hugo")
                    sh("hugo --config config.toml --baseURL $clear -d $clear")
                    sh("hugo --config config.toml --baseURL $vpn   -d $vpn")
                    sh("hugo --config config.toml --baseURL $tor   -d $tor")
                }
            }
        }
        stage("Deploy"){
            agent { docker { image "alpine:latest"}}
            steps("Deploy") {
                script {
                    echo("Deployment") 
                    sh("ls -a")
                    sh("pwd")
                }
            }
        }
    }

    post {
        success {
            echo("success")
        }
        failure {
            echo("fail")
        }
    }
}


