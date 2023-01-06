pipeline {
    agent any
    environment {
       vpn   = "http://blog.ttk.loc"
       tor   = "http://q6ft5vxol7xt4az7ibmub3xsyyqqsbmm35g7xcnhoijpulez33zupwad.onion/"
       clear = "https://gzttk.org/"
    } 
    stages {
        stage("Build") {
            steps {
                script {
                    sh("git submodule init")
                    sh("git submodule update --recursive")
                    sh("hugo --config config.toml --baseURL $clear -d `echo $clear | cut -f '3' -d'/'`")
                    sh("hugo --config config.toml --baseURL $vpn   -d `echo $vpn   | cut -f '3' -d'/'`")
                    sh("hugo --config config.toml --baseURL $tor   -d `echo $tor   | cut -f '3' -d'/'`")
                }
            }
        }
        stage("Deploy"){
            steps("Deploy") {
                script {
                    echo("Deployment") 
                    // Removing old versions
                    sh("ssh    -i~/.ssh/id_ed25519 'rm -rf /opt/hugoClear/* /opt/hugoVpn/* /opt/hugoTor/*'")

                    // Deploing
                    sh("scp -r -i~/.ssh/id_ed25519 `echo $clear | cut -f '3' -d'/'`/* deploy@blog.ttk.loc:/opt/hugoClear")
                    sh("scp -r -i~/.ssh/id_ed25519 `echo $vpn   | cut -f '3' -d'/'`/* deploy@blog.ttk.loc:/opt/hugoVpn")
                    sh("scp -r -i~/.ssh/id_ed25519 `echo $tor   | cut -f '3' -d'/'`/* deploy@blog.ttk.loc:/opt/hugoTor")
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


