# Azure DevOps Terraform

1. Goto Azure DevOps portal


2. Create a New project 

	Give Project name -> Click on Create


3. Goto Repos -> Files -> Click on Import -> Give the Terraform Files Repo URL in Clone URL feild -> Click on Import 

![STATE-FILE](https://github.com/Pavan-1997/Azure_DevOps_Terraform/assets/32020205/dc5f70be-692c-4d9b-8e69-e857154766ee)


4. For BUILD Pipleline - Now goto Pipelines -> Click on Create Pipeline -> Select the Azure Repos Git -> Select the repository created from the cloned URL -> Click on Started pipeline -> Give a YAML file from the repo snd click on Save and Run 

	For permission error -> Click on Authorize

![TF-CI](https://github.com/Pavan-1997/Azure_DevOps_Terraform/assets/32020205/ba6dbe6c-37c6-4d60-ad71-41ecbde30681)


5. For RELEASE Pipleline - Now goto Pipelines -> Click on Releases -> Click on New pipeline -> Select the Empty job
	
	Click on Add an artifact -> Source type - Build -> Select source as the repo name -> Click on Add
	
	Click on the Continous deployment trigger for the Artifact -> Enable it -> Build branch filters - The build pipeline's default branch 
	
	Goto Deployment task -> Select your self-hosted Agent pool created 

	- Click on + to add another task -> Search and add Install Extract files -> Give below feilds
	
	Archive file patterns - _$(Build.DefinitionName)/$(Build.BuildId)-
							build/$(Build.BuildId).zip
	
	Destination folder - '$(System.DefaultWorkingDirectory)/'
	
	
	- Click on + to add another task -> Search and add Install Terraform -> Give below feilds
	
	Display name - Terraform - init
	
	Select the Azure subscription
	
	Select the previously created Resource-group, Storage account, Container, Key - prod.terraform.tfstate
	
	- Click on + to add another task -> Search and add Install Terraform -> Give below feilds

	Command - apply 
	
	Additional command arguments - --auto-approve
	
	Select the Azure subscription
	
	Atlast rename the pipeline -> Click on Save

![RELEASE-DEPLOY](https://github.com/Pavan-1997/Azure_DevOps_Terraform/assets/32020205/85b3b9bb-5e25-4e2a-a0d4-ff97cd64032c)

![RELEASE-DEPLOY-TASKS](https://github.com/Pavan-1997/Azure_DevOps_Terraform/assets/32020205/6067b2c3-0df9-41e7-ac9e-c0034faee649)

![INFRA-VM](https://github.com/Pavan-1997/Azure_DevOps_Terraform/assets/32020205/4734defa-cc95-427d-859e-311c5a301d13)


6. Now add another stage for destruction in same pipeline created earlier by cloning the previous stage

	Now click on the tasks
	
	Just chnage the last task Command - destroy 
	
	Click on Save


7. Click on the Pre-deployment conditions for Destroy stage -> Pre-deployment approvals -> Approvers - Self -> Timeout - 2

	Save the pipeline


````
Build.SourcesDirectory = NA
Build.ArtifactStagingDirectory = NA

Build.BuildId = 44
Build.DefinitionName = Day8_Terraform
System.DefaultWorkingDirectory = /home/ubuntu/_work/r1/a



ARTIFACT - _$(Build.DefinitionName)/$(Build.BuildId)-build/$(Build.BuildId).zip
	   '$(System.DefaultWorkingDirectory)/'


	   _Day8_Terraform/44-build/44.zip
           /home/ubuntu/_work/r1/a/
	   


INIT - '$(System.DefaultWorkingDirectory)/'

	/home/ubuntu/_work/r1/a/



PLAN - '$(System.DefaultWorkingDirectory)/'

 	/home/ubuntu/_work/r1/a/
```
