
## Download Terraform

curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

sudo apt-get update && sudo apt-get install terraform

  

![](https://lh6.googleusercontent.com/GrH03QY6JJklhrpX_TpmP3Kf4PbF_dHWfUYmKDcNxPPlSiwDuN33WA_2PVFxal-fTBo20ae-0s4CyvbDuRQHQQpdwfZBSslTMDrzqb5pvTHUCyYvd7keVmkys6e-9CA3hjaGOPNWj5af_dKEZB0)

  

Download Provider Aws

git clone [https://github.com/hashicorp/terraform-provider-aws.git](https://github.com/hashicorp/terraform-provider-aws.git)

  

Initialize the terraform

cd terraform-provider-aws/

terraform init

  

Aws cli login

aws configure

  

![](https://lh5.googleusercontent.com/ui6g8zYPl4sLOoGlvJByEoc3QE21kBnaazZDebPQ0gEBN7yan8BP71S3Qy5vBDIWcPdEJAKJLMmiIwHc5D05E_pBtFqt-LV-kPM50x2PlIke79f0sGtAn9D8avEURJEGO5wslUGzdIh_Lx25v2U)

  
  
  

terraform output kubeconfig > ~/.kube/config

kubectl cluster-info

![](https://lh6.googleusercontent.com/klsEg1CXx7e78AiYi4D2FxJ2TGR2NJtF0RY4j9nqGDKdDZA3tSY9DVqj7vrO1WANMSOdVqfSCqxmBF4EIq5fu6DLzokOGBIWeGPV5Sagiqd3VqIruSbItgFiwMt6pBad_C7mh_eOyu5WO--7TSM)

  

Download the IAM Authenticator

curl -o aws-iam-authenticator [https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator](https://s3.us-west-2.amazonaws.com/amazon-eks/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator)

  

chmod +x aws-iam-authenticator

sudo mv aws-iam-authenticator /usr/local/bin/

aws-iam-authenticator version

  

Error

kubectl cluster-info

![](https://lh5.googleusercontent.com/27FJeZYcLguHbKnCtJWYKQ4c7aLMlVAFkhqwX8gm-r9YlCP86ciD2poZlACLvcBt7LHnzmEwa0yrj5yHl_EKSq_hbc_5uSi0JmV_vTchDk9xys76FhCwRUQdVD_GuKGFbtXqZuOCGy2ed8L3Gto)

Solution

aws eks update-kubeconfig --name terraform-eks-demo --region=us-west-2

![](https://lh6.googleusercontent.com/BM9XCn3uZv0409MkBRRB9zqKSrc2wCy3iQV4x6LPr-M0Jn-DF_s8ENOr_FOh8Chi-j_abRoDkZ0dMUISWWQrMRbvipd7hfnHNkaHlR-yEcWaFme11ciD_Z5uk7V2I9SW3lly7x4Jr4TnIl1SEDE)
