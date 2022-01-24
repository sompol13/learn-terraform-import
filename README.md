## Import Terraform Configuration
In this tutorial, you will import an existing Docker container and image into an empty Terraform workspace. By doing so, you will learn strategies and considerations for importing real-world infrastructure into Terraform.

### Create a Docker container
- `docker run --name hashicorp-learn --detach --publish 8080:80 nginx:latest`
- Visit the address 0.0.0.0:8080 in your web browser to see the NGINX default index page.

### Import the container into Terraform
- `terraform init`
- `docker ps`
- `terraform import docker_container.web $(docker inspect --format="{{.ID}}" hashicorp-learn)`

### Create configuration (Current state)
1. Copy your Terraform state into a configuration file.
2. Run `terraform plan` to identify and remove read-only configuration arguments.
3. Re-run `terraform plan` to confirm the configuration is correct.
4. Run `terraform apply` to finish synchronizing your configuration, state, and infrastructure.
5. Use `terraform show -no-color > docker.tf` to copy your Terraform state into your docker.tf file.
- `terraform plan`
- Remove all six of these attributes from your `docker.tf`
- Now verify that the errors have been resolved by re-running `terraform plan`
- Notice that Terraform plans to destroy then recreate your Docker container because `env` is not defined.
- Add `env` to your `docker_container.web` resource.
- Now verify that the errors have been resolved by re-running `terraform plan`
- The plan should now execute successfully `Plan: 0 to add, 1 to change, 0 to destroy.`
- `terraform apply`

### Verify import
Regardless of which method you used, your Docker container is now managed by Terraform. Use the Docker CLI to inspect the container. Note the "Status" — the container has been up and running since it was created.
- Visit 0.0.0.0:8080 in your web browser to verify that the container is still working as intended.

### Manage the container with Terraform
Now that Terraform manages the Docker container, use Terraform to change the its configuration.
- In your `docker.tf` file, change the container's external port from `8080` to `8081`.
- Apply the change. This will cause Terraform to destroy and recreate the container with the new port configuration.
- Now verify that the container has been replaced with a new one with the new configuration by running `docker ps` or visiting `0.0.0.0:8081` in your web browser.



### Reference
https://learn.hashicorp.com/tutorials/terraform/state-import