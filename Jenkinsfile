pipeline {
    agent any
    environment {
       vpn   = "http://blog.ttk.loc"
       tor   = "http://q6ft5vxol7xt4az7ibmub3xsyyqqsbmm35g7xcnhoijpulez33zupwad.onion/"
       clear = "https://gzttk.org/"
    } 
    stages {
        steps("Build") {
            script {
                sh("hugo --config config.toml --baseURL $clear -d $clear")
                sh("hugo --config config.toml --baseURL $vpn   -d $vpn")
                sh("hugo --config config.toml --baseURL $tor   -d $tor")
            }
        }
        steps("Deploy") {
            script {
                echo("Deployment") 
                sh("ls -a")
                sh("pwd")
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


