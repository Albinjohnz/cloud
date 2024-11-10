Here's a paraphrased version of your instructions:

---

### **Inventory and Playbook Setup**

1. **Inventory File**: 
   Create an `inventory.ini` file to specify the EC2 instance for Ansible to connect to. This file should define the EC2 instance's public IP address and the corresponding SSH credentials.

2. **Playbook File (`ansible.yml`)**:
   The `ansible.yml` playbook contains the following tasks:
   - **Target Host Group**: Specifies the EC2 group for the playbook to target.
   - **Update APT Repository**: Updates the APT package repository cache.
   - **Install Prerequisites**: Installs required packages like `apt-transport-https`, `ca-certificates`, `curl`, and `software-properties-common`.
   - **Add Docker GPG Key**: Adds Docker's official GPG key for security.
   - **Add Docker Repository**: Adds Docker's official APT repository to the system.
   - **Install Docker Engine**: Installs Docker from the official repository.
   - **Start Docker Service**: Ensures Docker is running and enabled to start on boot.
   - **Add User to Docker Group**: Adds the Ansible user to the Docker group for permissions.
   - **Run Nginx Docker Container**: Pulls the Nginx image from Docker Hub and starts the container, mapping port 80 for HTTP traffic.

### **Running the Playbook**

1. **Test Connection**:
   Before running the playbook, test the connection to the EC2 instance using Ansible’s `ping` module:

   ```bash
   ansible -i inventory.ini ec2 -m ping
   ```

   This checks if Ansible can communicate with the EC2 instance.

2. **Execute the Playbook**:
   Run the `ansible.yml` playbook to install Docker, start the service, and launch the Nginx container:

   ```bash
   ansible-playbook -i inventory.ini ansible.yml
   ```

   This will install Docker, configure it to run on boot, and set up an Nginx container on port 80.

3. **Access the Application**:
   After successfully running the playbook, open your web browser and navigate to the EC2 instance's public IP to access the Nginx application:

   ```plaintext
   http://<ec2_public_ip>:80
   ```

### **Troubleshooting**

- **Security Group Configuration**: Ensure that the security group attached to the EC2 instance permits inbound HTTP traffic on port 80. You may need to modify the inbound rules to allow traffic from all IP addresses (`0.0.0.0/0`) or specific IP ranges.
  
- **Verify Docker**: If the container is not working correctly:
  - Check if Docker is running with:

    ```bash
    sudo systemctl status docker
    ```

  - List the running containers with:

    ```bash
    docker ps
    ```

If the Docker container isn't accessible, ensure the EC2 instance's security group is correctly configured to allow inbound traffic on port 80.