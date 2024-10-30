# Blue-Green Deployment in Kubernetes

## Overview

This repository shows how to use **Blue-Green Deployment** with **Kubernetes**. This method helps you update applications smoothly, keeping downtime to a minimum. In this demo, we have two environments: Blue and Green, plus a service to manage traffic between them.

## What is Blue-Green Deployment?

Blue-Green Deployment means having two identical environments:

- **Blue Environment**: This is the current version of your application that users are accessing.
- **Green Environment**: This is where the new version is deployed and tested without affecting users.

### Benefits

- **No Downtime** ⏳: Users can continue using the application while updates happen.
- **Easy Rollback** 🔄: If there are problems, you can quickly switch back to the previous version.
- **Safe Testing** 🛡️: Test new features in a real-like environment without risks.

## Getting Started

### Prerequisites

- **Kubernetes Cluster**: Set up a local or cloud-based Kubernetes environment (like Minikube or EKS).
- **kubectl**: This is the command-line tool to interact with your Kubernetes cluster.
- **Docker (optional)**: if you want to build custom images, have Docker installed.

### Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/prasanth624/blue-green-deployment-k8s.git
   cd blue-green-deployment-k8s
   ```

2. **Deploy to Kubernetes**:

   Apply the Kubernetes files:

   ```bash
   kubectl apply -f blue-deployment.yaml
   kubectl apply -f green-deployment.yaml
   kubectl apply -f service.yaml
   ```

3. **Access the Application**:

   Forward the service port to access the application:

   ```bash
   kubectl port-forward service/my-app 8080:80
   ```

   Visit [http://localhost:8080](http://localhost:8080) to see the Blue version in action! 🎉

### Switching Environments

To switch from the Blue environment to the Green environment:

1. **Open the Service YAML** ✏️:
   Find your `service.yaml` file where the service configuration is defined.

2. **Modify the Selector** 🔄:
   Change the `version` field from `blue` to `green` in the selector section.

   **Before**:
   ```yaml
   selector:
     app: my-app
     version: blue  # Current version
   ```

   **After**:
   ```yaml
   selector:
     app: my-app
     version: green  # Updated to Green
   ```

3. **Apply the Changes** 🚀:
   Save the file and run the following command to apply the updates:

   ```bash
   kubectl apply -f service.yaml
   ```

4. **Verify the Update** ✅:
   Check that the service is now routing to the Green version:

   ```bash
   kubectl get svc my-app
   ```

5. **Test the Application** 🖥️:
   Port-forward the service and visit [http://localhost:8080](http://localhost:8080) to see "This is the green version."   

### Rollback

If something goes wrong after switching to Green, just change the service back to Blue and reapply the configuration.

## Example Outputs

- **Blue Version**: Shows "This is the blue version."
- **Green Version**: Shows "This is the green version."

## Contributing

If you have ideas for improvements or new features, feel free to submit a pull request. 🤝

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. 📜
