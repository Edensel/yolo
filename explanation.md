# Containerization Explanation

## 1. Choice of Base Image

For both the frontend and backend containers, we chose `node:18.19.1-alpine` as the base image. This decision was made because:

- Alpine-based images are significantly smaller in size compared to full Node.js images.
- The specific version (18.19.1) ensures consistency across development and production environments.
- Alpine images provide a good balance between size and functionality for Node.js applications.

## 2. Dockerfile Directives

Key directives used in our Dockerfiles include:

- `FROM`: Specifies the base image.
- `WORKDIR`: Sets the working directory inside the container.
- `COPY`: Copies files from the host to the container.
- `RUN`: Executes commands during the build process.
- `EXPOSE`: Informs Docker that the container listens on specified network ports at runtime.
- `CMD`: Provides defaults for an executing container.

Reasoning:
- We use multi-stage builds to separate the build environment from the production environment.
- `npm ci` is used instead of `npm install` in the client dockerfile for more reliable and reproducible builds.
- We clean caches and remove unnecessary files to reduce image size.

## 3. Docker-compose Networking

In our docker-compose.yml file:

- We define a custom bridge network named 'yolo-network'.
- Port mappings are specified for each service (3000 for frontend, 5000 for backend, 27017 for MongoDB).
- All services are connected to the 'yolo-network', allowing inter-container communication.

Reasoning:
- A custom bridge network provides isolation and allows containers to communicate using service names.
- Specific port mappings allow external access to the services while maintaining isolation.

## 4. Docker-compose Volume Definition

We define and use volumes for data persistence:

- `client-volume` for the frontend
- `backend-volume` for the backend
- `mongodb-data` for the MongoDB database

Reasoning:
- Volumes ensure data persistence across container restarts and updates.
- They allow for easier backup and restore processes.

## 5. Git Workflow

For this project, we utilized a simple workflow centered on the master branch:

- All development work was performed directly on the master branch.
- Commits were made frequently to track progress and changes.
- Each commit message clearly describes the changes made or features added.

Reasoning for this approach:
- As this is a smaller project with a single developer, using the master branch exclusively simplifies the workflow.
- Frequent commits on the master branch provide a clear, linear history of the project's development.
- This approach allows for quick iterations and immediate integration of new features or bug fixes.

## 6. Running and Debugging

- We use `docker-compose up` to start all services.
- For debugging, we use Docker logs: `docker logs <container_name>`.
- We've implemented healthchecks in our docker-compose file to ensure services are truly ready.
- We can also start the client with npm start and node server.js for the backend.
- We use mongodb database which is started by 'sudo service mongod start'.

## 7. Docker Image Tagging

We follow these tagging conventions:

- Use semantic versioning (e.g., v1.0.0, v1.1.0). In this case, we used v2.0.0
- Include the type of image (e.g., img-a:v2.0.0, img-b:v2.0.0).

Reasoning:
- Semantic versioning allows for clear understanding of version differences.
- Including image type in the tag improves clarity in multi-container applications.

## 8. DockerHub Deployment
![DockerHub Screenshot](images/image1.png)
![DockerHub Screenshot](images/image2.png)
![DockerHub Screenshot](images/image3.png)

As shown in the screenshot above, our images are properly tagged and available for pull.

Our images are publicly available on DockerHub under the repositories [edensel/img-a] and [edensel/img-b]

## 9. Ansible deployment
We use a YAML-formatted inventory file that defines a single host:

- The host is named 'myserver' and represents a Vagrant-managed virtual machine.
- We specify connection details including the SSH port, user, and private key file.

Reasoning:

- YAML format provides better readability and structure compared to INI format.
- Detailed host configuration ensures reliable connection to the Vagrant VM.

## 10. Playbook Structure
Our main playbook is organized as follows:

- It targets the 'myserver' host defined in the inventory.
- We use roles to organize and modularize our tasks.
- The playbook includes tasks for ensuring the Docker network exists and all containers are running.

Reasoning:

- Using roles improves code reusability and maintainability.
- The structure allows for easy addition of new components or modifications to existing ones.

## 11. Role-based Configuration
We've defined three main roles:

- MongoDB: For deploying and configuring the MongoDB container.
- Backend: For deploying the backend service container.
- client: For deploying the client service container.

- Each role has its own variables defined in defaults/main.yml, allowing for easy customization.

Reasoning:

- Role-based structure allows for better organization and potential reuse in other projects.
- Default variables provide flexibility while maintaining sensible defaults.

## 12. Variable Management
We use a combination of:

- Global variables defined in vars/main.yml
- Role-specific variables in each role's defaults/main.yml
- The default() filter in role variables

Reasoning:

- This approach provides a good balance between global configuration and role-specific defaults.
- It allows for easy overriding of variables when needed, without modifying role files.

## 13. Docker Network Configuration
We create a custom Docker network named 'yolo-network':

- All containers are connected to this network.
- This allows for inter-container communication using service names.

Reasoning:

- Custom network provides isolation from other Docker networks on the host.
- Using service names for communication simplifies container orchestration.

## 14. Container Deployment Strategy
For each service (MongoDB, backend, client):

- We pull the specified Docker image.
- We start a container with appropriate configuration (network, ports, environment variables).
- We implement a wait condition to ensure the service is ready before proceeding.

Reasoning:

- Pulling images ensures we have the latest version specified.
- Wait conditions improve reliability by ensuring services are fully operational before dependent services start.

## 15. Security Considerations

- We've removed password authentication for MongoDB to simplify the local development setup.
- The playbook uses `become: true` to ensure necessary permissions on the Vagrant VM.

## 16. Debugging and Maintenance

- Ansible's verbose mode can be used for debugging: ansible-playbook playbook.yml -vvv
- Container logs can be viewed using docker logs <container_name>
- The playbook can be re-run to ensure configuration or to update services.

## 17. Deployment Process
To deploy the application:

- Ensure the Vagrant VM is up and running.
- Run the Ansible playbook: ansible-playbook playbook.yml
- Access the application via port forwarding set up in the Vagrantfile.

This Ansible configuration provides a repeatable, automated process for deploying the e-commerce application, ensuring consistency across deployments and simplifying the setup process.

## Kubertnetes Deployment Explnanation
 1. Choice of kubernetes objects
 We used the following Kubernetes objects:

- **Deployments** for frontend and backend:
  - Provides declarative updates and easy scaling.
  - Suitable for stateless applications.

- **StatefulSet** for MongoDB:
  - Provides stable network identities and persistent storage.
  - Crucial for maintaining database consistency.

- **Services** for networking:
  - LoadBalancer for frontend and backend to expose externally.
  - ClusterIP for MongoDB for internal communication.

2. Exposing Pods to Internet Traffic

We used a LoadBalancer Service for the frontend to expose it to the internet. This:
- Automatically creates an external load balancer.
- Provides an external IP address for access.

For internal communication, we used ClusterIP Services, which:
- Provide stable internal IPs for service discovery.
- Keep internal services isolated from external access.

3. Persistent Storage

We implemented persistent storage for MongoDB using:
- PersistentVolumeClaims (PVCs) in the StatefulSet.
- This ensures data persists even if the MongoDB pod is deleted or rescheduled.

4. Git Workflow

We followed a straightforward Git workflow:

1. All development was done directly on the master branch.
2. Each significant change or feature was committed with a descriptive commit message.
3. We ensured that the master branch always contained a stable, working version of the project.
4. Tagged significant versions (e.g., `v1.0.0`) for release management.

This approach allowed for rapid development while maintaining a clear history of changes. However, for larger teams or more complex projects, a feature branch workflow might be more appropriate.

5. Docker Image Tagging

We used semantic versioning for Docker images:
- Format: `username/image-name:vX.Y.Z`
- Example: `edensel/img-a:v2.0.0`
