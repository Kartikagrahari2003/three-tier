pipeline {
    agent { label 'electronix' }

    stages{
        stage("Provision node.js runtime"){
            steps{
                '''
                if ! command -v node &> /dev/null;then
                sudo apt-get update
                sudo apt-get install -y curl
                curl -fsSL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh
                sudo bash nodesource_setup.sh
                sudo apt-get install -y node.js
                rm -f nodesource_setup.sh
                fi
                node -v

                '''
            }
        }
    }
}
