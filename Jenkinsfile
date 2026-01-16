pipeline {
  agent any

  environment {
    ARM_CLIENT_ID       = credentials('AZURE_CLIENT_ID')
    ARM_CLIENT_SECRET   = credentials('AZURE_CLIENT_SECRET')
    ARM_TENANT_ID       = credentials('AZURE_TENANT_ID')
    ARM_SUBSCRIPTION_ID = credentials('AZURE_SUBSCRIPTION_ID')
  }

  stages {

    stage('Checkout') {
    steps {
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/main']],
            userRemoteConfigs: [[
                url: 'git@github.com:vnaidu84-creator/azure-vm-terraform.git',
                credentialsId: 'github-ssh'
            ]]
        ])
    }
}

    stage('Terraform Init') {
      steps {
        sh 'terraform init'
      }
    }

    stage('Terraform Validate') {
      steps {
        sh 'terraform validate'
      }
    }

    stage('Terraform Plan') {
      steps {
        sh 'terraform plan'
      }
    }

    stage('Terraform Apply') {
      when {
        branch 'main'
      }
      steps {
        sh 'terraform apply -auto-approve'
      }
    }
  }

  post {
    success {
      echo 'VM created successfully using Terraform'
    }
    failure {
      echo 'Terraform pipeline failed'
    }
  }
}


