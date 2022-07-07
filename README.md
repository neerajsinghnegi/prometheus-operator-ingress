
## Using Terraform Deploy Aws EKS and Configure Prometheus-Operator [Prometheus | Grafana | Alert Manager] in the Kubernetes
### Download Terraform
    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
    sudo apt-add-repository "deb [arch=amd64] 

    https://apt.releases.hashicorp.com $(lsb_release -cs) main"
    sudo apt-get update && sudo apt-get install terraform


![](https://lh6.googleusercontent.com/GrH03QY6JJklhrpX_TpmP3Kf4PbF_dHWfUYmKDcNxPPlSiwDuN33WA_2PVFxal-fTBo20ae-0s4CyvbDuRQHQQpdwfZBSslTMDrzqb5pvTHUCyYvd7keVmkys6e-9CA3hjaGOPNWj5af_dKEZB0)

#### Download Provider Aws
`git clone [https://github.com/hashicorp/terraform-provider-aws.git]`

#### Initialize the terraform

    cd terraform-provider-aws/
    terraform init

#### Aws cli login
    aws configure
![](https://lh5.googleusercontent.com/ui6g8zYPl4sLOoGlvJByEoc3QE21kBnaazZDebPQ0gEBN7yan8BP71S3Qy5vBDIWcPdEJAKJLMmiIwHc5D05E_pBtFqt-LV-kPM50x2PlIke79f0sGtAn9D8avEURJEGO5wslUGzdIh_Lx25v2U)

    kubectl cluster-info

![](https://lh6.googleusercontent.com/klsEg1CXx7e78AiYi4D2FxJ2TGR2NJtF0RY4j9nqGDKdDZA3tSY9DVqj7vrO1WANMSOdVqfSCqxmBF4EIq5fu6DLzokOGBIWeGPV5Sagiqd3VqIruSbItgFiwMt6pBad_C7mh_eOyu5WO--7TSM)

### Download the IAM Authenticator
curl -o aws-iam-authenticator [https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator](https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator)

    chmod +x aws-iam-authenticator
    sudo mv aws-iam-authenticator /usr/local/bin/
    aws-iam-authenticator version

  
##### Note- Not able to connect to EKS

    kubectl cluster-info

![](https://lh5.googleusercontent.com/27FJeZYcLguHbKnCtJWYKQ4c7aLMlVAFkhqwX8gm-r9YlCP86ciD2poZlACLvcBt7LHnzmEwa0yrj5yHl_EKSq_hbc_5uSi0JmV_vTchDk9xys76FhCwRUQdVD_GuKGFbtXqZuOCGy2ed8L3Gto)

##### Solution
    aws eks update-kubeconfig --name terraform-eks-demo --region=us-west-2

![](https://lh6.googleusercontent.com/BM9XCn3uZv0409MkBRRB9zqKSrc2wCy3iQV4x6LPr-M0Jn-DF_s8ENOr_FOh8Chi-j_abRoDkZ0dMUISWWQrMRbvipd7hfnHNkaHlR-yEcWaFme11ciD_Z5uk7V2I9SW3lly7x4Jr4TnIl1SEDE)

#### Nginx Controller

    helm repo add nginx-stable https://helm.nginx.com/stable
    helm repo update
    helm install my-release nginx-stable/nginx-ingress
#### Nginx 
    kubectl apply -f deployment.yaml -f nginx-svc.yaml

### Prometheus-Operator Deployment
    git clone [https://github.com/prometheus-operator/kube-prometheus.git](https://github.com/prometheus-operator/kube-prometheus.git)
    cd kube-prometheus/
    kubectl apply --server-side -f manifests/setup
    until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
    kubectl apply -f manifests/

#### Ingress
    cd ../ingress/
    kubectl apply -f ingress.yaml
    kubectl describe ingress prometheus-ingress -n monitoring
![](https://lh5.googleusercontent.com/eiUvNFkuOMOvpttdhnJ3ybB9vYR0vG5B_0I79ZXrg9z-XF8U87o0OE5CNUW0KwGkL6YZR1NI6FutCL5BVgWHnInrSxO8lUjrHXmPlMQFDMk0xe1VsZ2gcL6h7ITsZazpC1UkR3ONVxTBE8ONK08)

##### Note- Make Ingess on the same namespace where there is a service running.

### Aws Route53
Make sure you have created the hosted zone record for the loadbalancer. So that you can able to connect your services using domain and sub-domain names.

    kubectl get svc

![](https://lh6.googleusercontent.com/8UFBNDgb4u2qt3GZHlMqS4pgEfQMgMInycjhB8t5kWtTyPtSc88JZXFfURT8O337vXh1lGN1bFq7npPay8XJSlyzsKZdcEwFY2DjUrPUDiVQMB9Uc80QGjF_7INlVkpxq40hPSMEvk9Yt5PXfLs)

#### Hosted Zone Record 
![](https://lh5.googleusercontent.com/swvTRcjG4OHqRnKwIAKhld-ucbNyANjtMcyGPqo-aGIJdx4mcJlA_i_DeJ11zf-Kd9QXGByH2pnNsJxpkz6HmJ1fh1g8RohvSjL4-A5d4o4qh2TrV5-u9pWhn6q9keJORMYjXIXbu4_ZKrv72hY)

#### Nginx service running on port 80
![](https://lh4.googleusercontent.com/Sh3T8LeyO3AMp0F9r5BP3oK0gag1l08Q4iK7xDVNn_qMQ9pfmRl8JvFHP8XmqEY1roqbD9h9EN6YXTeI7S7XfO_Dd-frpV7whStEY5Opit7Y2EE8YtHkzKrrkrxHrU3SNqBGemNNVmAdNutiJA)

#### Prometheus service running on port 9090
![](https://lh4.googleusercontent.com/lBuhTgOYEZdkt8QFpoMHEZJtejzmtkNZepAfkO9j61mfWzqjUxHXgOhgXJQYpOcBOXJE-Xy5yCbgpGSc5WFgh4To5gVP6gGgS9En1_wSaRLen2Z3pkQ90XCi0HXhxV1F2PMY8mMnnnM5oT14fHg)
#### Grafana service running on port 3000
![](https://lh5.googleusercontent.com/c9q0BTrgKK02wJhl_0jouWTXJgMXdU6g3AXTI5ClDTS3M4YrjP9OggKFw9tog15D20_gNQcTDn-sLbMvNjK6P_atkE4SkGmtNO0_SKB_exqOHM5hxBfDth6c47SR0SrBYmu_32IzIjkaWPMhVHo)
#### Alert Manager service running on port 9093
![](https://lh5.googleusercontent.com/Ni5gkcjLBENribilzCp75lEICNMzwLSembeT69Bg0EDVAOKM20M-N_fRra-1XdeleVDAu3w2rLfvxf91ZOUmdb-KHbXjjNRWn9T409Y7t2uryPUHYOcZW3MrmxiXnzAQqZsRBklU2_rBjz9KFmo)

