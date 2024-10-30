# build-image-without-docker


---

1. Create an EC2 Ubuntu Machine on AWS

1. Log in to AWS Console: Go to AWS Management Console.


2. Launch EC2 Instance:

Navigate to EC2 service from the AWS dashboard.

Click Launch Instance.

Choose an Ubuntu Server 22.04 LTS (HVM), SSD Volume Type AMI.

Select an Instance Type (t2.micro is free tier eligible).

Configure instance details, storage, and security group (allow SSH (port 22) for connecting to the instance).

Launch the instance and download the private key (.pem file).



3. Connect to Your EC2 Instance:

Open your terminal and use the .pem file to connect:

ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>





---

2. Update and Install Required Tools on EC2

1. Update Your EC2 Instance:

Once connected, update the package manager:

'sudo apt update && sudo apt upgrade -y'



2. Install Docker (since Buildpacks create container images, Docker is needed):

Install Docker:

sudo apt install docker.io -y

Start Docker:

sudo systemctl start docker
sudo systemctl enable docker



3. Install pack CLI (the tool used to build images using Buildpacks):

Download and install pack:

curl -sSL "https://github.com/buildpacks/pack/releases/download/v0.31.0/pack-v0.31.0-linux.tgz" | sudo tar -C /usr/local/bin/ -xzv pack

Verify installation:

pack --version





---

3. Clone or Upload Your Project to EC2

1. Clone Your Project:

If your project is on GitHub, you can clone it directly into your EC2 instance:

git clone https://github.com/yourusername/yourproject.git
cd yourproject



2. Upload Your Project (If not on GitHub):

Use an FTP tool or SCP command to upload your local project files to the EC2 instance.





---

4. Build the Container Image Using Buildpacks

1. Navigate to Your Project Directory:

Go to the project folder where your code is located:

cd /path/to/your/project



2. Build the Image Using Buildpacks:

Use the pack CLI to build the container image for your project:

pack build my-app --builder paketobuildpacks/builder:base

This command tells Buildpacks to analyze your project, automatically select the right build environment, and create a Docker container image called my-app.





---

5. Test Your Image Locally

1. Run the Docker Image:

Once the image is created, you can test it locally to make sure everything is working.

docker run -p 8080:8080 my-app

This will start your application inside a container and expose it on port 8080.



2. Verify the Application:

In your browser, navigate to http://<your-ec2-public-ip>:8080 to see if your app is running correctly.





---

6. Push Your Image to a Container Registry (Optional)

If you want to deploy your image elsewhere (like on a cloud service), you’ll need to push the image to a container registry like Docker Hub.

1. Login to Docker Hub:

docker login


2. Tag and Push Your Image:

docker tag my-app <your-dockerhub-username>/my-app
docker push <your-dockerhub-username>/my-app



Now, your image is available on Docker Hub, and you can deploy it to cloud platforms like AWS ECS, Kubernetes, or Google Cloud Run.


---

7. Deploy the Image (Optional)

If you're using a service like Google Cloud Run or AWS Elastic Container Service, you can deploy your image directly from Docker Hub.

For example, using Google Cloud Run:

1. Go to Google Cloud Run.


2. Create a new service and provide the Docker Hub image URL (<your-dockerhub-username>/my-app).


3. Deploy your image.




---

Summary:

1. Create an EC2 instance using AWS.


2. Install Docker and the Pack CLI on the EC2 instance.


3. Clone or upload your project to the EC2 instance.


4. Use Buildpacks to create a container image automatically.


5. Run and test the container locally.


6. (Optional) Push the image to Docker Hub or another registry.


7. (Optional) Deploy the image to a cloud platform.
