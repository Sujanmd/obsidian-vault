**`[http://opentams-aws-use1-jammer-integration.integration.amaginow.tv/](http://opentams-aws-use1-jammer-integration.integration.amaginow.tv/)`**
This is the endpoint where i can access opentams

![[Attachments/Pasted image 20261012122804.png|634]]

![[Attachments/Pasted image 20261012122829.png|587]]

main.tf is the primary config files containing the main cloud resources being created and managed
variables.tf defines input variables allowing to customize deployment variables
outputs.tf used to display the values after infra changes
providers.tf config cloud providers and how terraform connects to their apis
versions.tf version constraints for terraform itself
terraform.lock.hcl locks plugin versions like tofu so that it runs accross differrent machines.
iam-identity and access management