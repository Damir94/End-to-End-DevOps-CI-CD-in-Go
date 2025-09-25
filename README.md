# End to End CI-CD Implementation of DevOps on a Go-lang web application

 We will implement End to End Implementation of DevOps on a Go-lang web application. We will implement the following things:
 - *Designed and implemented* a fully automated, end-to-end *CI/CD pipeline,* enabling seamless and faster deployment of application updates with minimal manual intervention.
 - *Containerized* the web application using a *multi-stage Docker build* to optimize and significantly reduce the container image size, improving efficiency in the CI pipeline.
 - *Created and managed Helm charts* to facilitate *Kubernetes* deployment across multiple environments within *AWS EKS,* ensuring scalable and consistent releases.
 - Developed a robust *GitHub Actions workflow,* incorporating *unit tests, integration tests,* and *Dockerization,* to ensure code quality and automate the build and deployment process.
 - Achieved *Continuous Delivery with Argo CD,* streamlining deployment management and enforcing GitOps practices for enhanced automation and monitoring of production changes.
 - Configured and deployed an *NGINX Ingress controller* to expose services and applications securely and efficiently, ensuring high availability and controlled access.
 - Orchestrated *DNS mapping* for the custom domain, ensuring smooth routing and accessibility to the deployed services.
 - Implemented *version tagging* and deployment tracking for continuous version control and traceability of releases across environments.

<img width="817" height="501" alt="Screenshot 2025-09-23 at 1 55 26 PM" src="https://github.com/user-attachments/assets/1e0ac17f-4f7b-442a-8212-d9bfe259fb73" />


Let’s Get Started !! 

1. Let’s copy first GitHub repository from below url:
   ```bash
   git clone https://github.com/Damir94/End-to-End-DevOps-CI-CD-in-Go
   cd End-to-End-DevOps-CI-CD-in-Go

  We will first run the go web app into local system, that is it running proper or not to get idea that, what exactly we need write in Docker file to run go web app.
  To build your go-web-app , run below command, it will download all dependencies to run go web-app.

    go build -o main .

  Now run below command to run your go web-app.

    go run main.go

 <img width="794" height="72" alt="Screenshot 2025-09-21 at 2 28 32 PM" src="https://github.com/user-attachments/assets/e6d1a4ee-c3dd-4aef-9e85-697b7fd3525d" />

Now you can access your application on below URL:
 - *URL : http://localhost:8080/courses*

 <img width="1131" height="795" alt="Screenshot 2025-09-21 at 2 29 02 PM" src="https://github.com/user-attachments/assets/19f36e5a-9034-4e94-8255-a7af14f26a1f" />

2. Now we need to write Docker file, and we are going to make multistage docker file for reducing image size and make sure, you have installed Docker desktop application inside your windows system.
So, you can find Docker file inside repository. So basically, what we are doing in this docker file that we are building the code binary in above step and copy that binary file of code and executing that inside distroless image which has no dependencies installed. This help to reduce dockerimage file size & provide high level of security.

 <img width="920" height="435" alt="Screenshot 2025-09-21 at 2 33 30 PM" src="https://github.com/user-attachments/assets/7a3658f7-0179-4731-9e19-8921b4e3454c" />

Run below commnad to build image:
```bash
 docker build -t abdurakhimovda522/go-web-app .
```
 <img width="1193" height="345" alt="Screenshot 2025-09-21 at 2 50 06 PM" src="https://github.com/user-attachments/assets/d2368c71-b04b-4b62-b946-a123476c82c4" />

You can see your image, inside Docker desktop.

 <img width="1262" height="444" alt="Screenshot 2025-09-21 at 2 50 31 PM" src="https://github.com/user-attachments/assets/73f61f18-2dc2-4ddb-bef3-539484f36e6c" />

Run below command to run container and make sure make container name as per your dockerhub username as we will push that image to dockerhub.
```bash
docker run -p 8080:8080 -dit abdurakhimovda522/go-web-app
```
 <img width="1208" height="78" alt="Screenshot 2025-09-21 at 2 51 57 PM" src="https://github.com/user-attachments/assets/95637e30-8f25-4cf7-95b8-3bf4794e4fc1" />

You can check your container running inside docker desktop.

 <img width="1261" height="464" alt="Screenshot 2025-09-21 at 2 52 37 PM" src="https://github.com/user-attachments/assets/bd765786-f0c2-49dc-84a8-cd625e668d94" />

Now you need to login inside your docker hub to push image:
```bash
docker login
```
 <img width="864" height="167" alt="Screenshot 2025-09-21 at 2 53 27 PM" src="https://github.com/user-attachments/assets/ac3d9d19-d706-425f-ab5b-64c2ccde7dee" />

Now run
 - *docker push "imagename"*
This will push image to dockerhub.

 <img width="1082" height="384" alt="Screenshot 2025-09-21 at 2 54 31 PM" src="https://github.com/user-attachments/assets/883959d1-ce6d-4f7d-bd10-a385a18ad25a" />

You can check your image on your docker hub.

 <img width="1901" height="745" alt="Screenshot 2025-09-21 at 2 55 19 PM" src="https://github.com/user-attachments/assets/c9c3f5d5-d138-44cd-9dae-0f2c5668b9db" />

3. So let’s create Kubernetes manifest file for deployment.
   Create “k8s” folder and inside that create “manifests” folder. Inside those create three manifests file name : deployment.yaml, ingress.yaml, service.yaml
   You can find all these file inside repo that you clone. The only thing which you need to change in deployment.yaml file that line no. 20, change docker image name with your docker image name.

<img width="686" height="424" alt="Screenshot 2025-09-21 at 3 13 40 PM" src="https://github.com/user-attachments/assets/3e5aa342-0ab8-42a8-83ee-9ca4fa578f15" />

<img width="618" height="329" alt="Screenshot 2025-09-21 at 3 13 25 PM" src="https://github.com/user-attachments/assets/dcf502f1-76f9-455e-a2dc-d4b7089d0e70" />

<img width="720" height="382" alt="Screenshot 2025-09-21 at 3 13 59 PM" src="https://github.com/user-attachments/assets/82ca0a95-0ed1-4866-baeb-fc581adb78b7" />

That’s it. Now to run this on your local system, you need to install “kubectl” command inside your cmd terminal. Run below commnad to install:
 - *winget install -e --id Kubernetes.kubectl*
   
 Now we have successfully installed kubectl, we need to install eksctl command to create cluster and making .konfig file to create inside system.

 you can download eksctl from here: https://eksctl.io/installation/

 Now after downloading, add that file into your system environment path to access any where in terminal.
 After that, run below command to create eks cluster. Remember you must have configured your system through “ AWS CLI “ command to authorize system.

 - *eksctl create cluster --name demo-cluster --region us-east-1*

<img width="716" height="138" alt="Screenshot 2025-09-21 at 3 17 15 PM" src="https://github.com/user-attachments/assets/d4f598b9-7fab-4584-82ca-4fd9feff758c" />

<img width="1190" height="533" alt="Screenshot 2025-09-21 at 3 28 54 PM" src="https://github.com/user-attachments/assets/19854cfd-4def-42f8-a522-6313da0215d5" />

<img width="1877" height="225" alt="Screenshot 2025-09-21 at 3 42 21 PM" src="https://github.com/user-attachments/assets/9a40d47f-2ac2-4259-a655-807bce5ca5fc" />

This will create a eks cluster inside your aws account, this process is going to take around 10–15 minutes.
Now run below commnad to deploy go-lang based web-app through kubernetes.
 - *kubectl apply -f k8s\manifests\deplyment.yaml*

<img width="1055" height="74" alt="Screenshot 2025-09-21 at 3 44 38 PM" src="https://github.com/user-attachments/assets/9470406c-98fc-44b1-97c5-9f30910c8a46" />

When you run this command, this will tell you all running pods in which your container is running.
 - *kubectl get pods*
   
<img width="784" height="95" alt="Screenshot 2025-09-21 at 3 44 59 PM" src="https://github.com/user-attachments/assets/36f61450-78bb-49c9-ab23-796d5b0ab5e8" />

Now run below command to run service file through kubernetes.
 - *kubectl apply -f k8s\manifests\service.yaml*

<img width="1023" height="174" alt="Screenshot 2025-09-21 at 3 46 08 PM" src="https://github.com/user-attachments/assets/4289bbfb-a039-40c7-9c15-8a16855d0ace" />

And this command will run ingress file:
 - *kubectl apply -f k8s\manifests\ingress.yaml*
   
<img width="1023" height="140" alt="Screenshot 2025-09-21 at 3 46 50 PM" src="https://github.com/user-attachments/assets/6143119a-92dd-4091-b789-c3485b7b16ff" />

Now you can check on which port or address your application is running but till now you won’t be able to found address as we have not installed ingress controller.
 - *kubectl get ing*

<img width="1023" height="140" alt="Screenshot 2025-09-21 at 3 46 50 PM" src="https://github.com/user-attachments/assets/cb74bece-9b1b-4c38-9b5e-c6a3f63f8ccc" />

Run below command to get ip for ingress:
 - *kubectl edit svc go-web-app*

<img width="853" height="48" alt="Screenshot 2025-09-21 at 3 48 44 PM" src="https://github.com/user-attachments/assets/389540c8-4780-4e78-a0ec-b1afc4f75c38" />

And change the type from: ClusterIP >> NodePort { to check all good }

<img width="243" height="134" alt="Screenshot 2025-09-21 at 3 48 18 PM" src="https://github.com/user-attachments/assets/8e6e40bc-b619-4652-bd0b-4bc81f5a0f77" />

Save file and close it.
When you run below command, you will get address of ingress.
 - *kubectl get svc*
 - *kubectl get nodes -o wide*

<img width="1197" height="232" alt="Screenshot 2025-09-21 at 3 49 43 PM" src="https://github.com/user-attachments/assets/7246c77d-73e6-415f-ac1c-5ca68c00b52d" />

So, everything is good, but if you try to access your file, you won’t be able to access as you need to update inboud & outbound rules for ec2:
Go to > ec2 instance > security group > add rule > all traffic > anywhere.
Now browse your ip as below and you will able to access your web-app.
Browser : externalipofcontainerruntime:portofsvc/courses

<img width="1629" height="780" alt="Screenshot 2025-09-21 at 3 53 50 PM" src="https://github.com/user-attachments/assets/765898d5-cbc5-45c1-b7bc-7790daa06f60" />

Now, let’s install NGINX ingress controller, for that run below command:
 - *kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.1/deploy/static/provider/aws/deploy.yaml*

<img width="1283" height="409" alt="Screenshot 2025-09-21 at 3 55 15 PM" src="https://github.com/user-attachments/assets/0359e8df-59cb-44c9-9663-6aeb3d014979" />

Now run below command to get pods:
 - *kubectl get pods -n ingress-nginx*
   
<img width="903" height="125" alt="Screenshot 2025-09-21 at 3 55 42 PM" src="https://github.com/user-attachments/assets/a000c7a5-5604-4819-98e8-7dd0bd4f9888" />

You can cross-check by open the file by below command:
 - *kubectl edit pods ingress-nginx-controller-f6dcd6c56-j4lhc -n ingress-nginx*

<img width="1281" height="93" alt="Screenshot 2025-09-21 at 3 57 27 PM" src="https://github.com/user-attachments/assets/db46c2fd-d35f-4c4a-826e-e291ce5a9886" />

Now run the below command and this time, your ingress will get address:
 - *kubectl get ing*
   
<img width="1283" height="80" alt="Screenshot 2025-09-21 at 3 57 45 PM" src="https://github.com/user-attachments/assets/2465304b-ca7a-4124-bfa6-f2bfd71f66be" />

This is the same address, you can verify in loadbalancer in aws account.

<img width="1600" height="374" alt="Screenshot 2025-09-21 at 3 58 31 PM" src="https://github.com/user-attachments/assets/fa560feb-37c0-4f07-82b0-59a6d9ba5605" />

You can check ip of your load balancer by running nslookup command.

<img width="1279" height="224" alt="Screenshot 2025-09-21 at 3 59 32 PM" src="https://github.com/user-attachments/assets/4d42d12e-12eb-4210-aa66-c27ba47ba3d5" />

Now if you are running on windows, go to the below address and edit hosts file as administrator.
Map the DNS in local system, so every time you try to access that web address, it will relocate to the load balancer ip.

<img width="693" height="330" alt="Screenshot 2025-09-22 at 12 54 34 PM" src="https://github.com/user-attachments/assets/465b4983-0e95-4db3-9270-1294711864d9" />

This is the same DNS name, which we have defined in ingress.yaml file.

<img width="684" height="361" alt="Screenshot 2025-09-22 at 12 56 58 PM" src="https://github.com/user-attachments/assets/98123a3d-a782-4e86-ba9e-8e3e5a00e3a0" />

Now if you try to access that website from below url , you can access web-app:
 -*go-web-app:local/courses*

<img width="1586" height="785" alt="Screenshot 2025-09-21 at 4 24 10 PM" src="https://github.com/user-attachments/assets/b174421e-d910-4e2a-9e31-d799e6398885" />

4. Now if you want to deploy in multiple environments, you can use helm chart. For that purpose, first install helm chart in local system:
   - *winget install Helm.Helm*

This will install helm command. Now run below command to create new directory name go-web-app-chart. You can find in repo.

 - *helm create go-web-app-chart*
   
<img width="1283" height="185" alt="Screenshot 2025-09-21 at 4 26 26 PM" src="https://github.com/user-attachments/assets/0525a7b6-3d06-4031-aa67-27b7465aeb89" />

Go to that directory by below command and remove all unnecessary folders.
 - *cd go-web-app-chart*
 - *rm -rf Charts*

<img width="976" height="113" alt="Screenshot 2025-09-21 at 4 29 37 PM" src="https://github.com/user-attachments/assets/0384a998-252f-4a4a-99d1-4e9edefc4a40" />

Now copy all three deployment.yaml , service.yaml and ingress.yaml files into template folder and delete everthing which was there previously.

<img width="676" height="75" alt="Screenshot 2025-09-21 at 4 33 56 PM" src="https://github.com/user-attachments/assets/23a3408e-38f6-487d-a8e0-126f340cbab6" />


And update the depluyment.yaml file with below variable. This will help developer or devops engineer to update whole docker image version just by writing in helm chart.
 - *abdurakhimov522/go-web-app:{{ .Values.image.tag }}*

<img width="886" height="576" alt="Screenshot 2025-09-21 at 4 36 07 PM" src="https://github.com/user-attachments/assets/4cfd4368-0c88-4a86-9e16-0470433fd225" />

Update values.yaml by replacing code from below:

<img width="677" height="455" alt="Screenshot 2025-09-21 at 4 39 09 PM" src="https://github.com/user-attachments/assets/5ce887e4-870b-4510-b77c-34e2cfa09928" />

Do not forgot to update repository name.
Now if you run below command, you will see your app, service, ingress.
 - *kubectl get all*
   
 <img width="784" height="276" alt="Screenshot 2025-09-21 at 4 41 46 PM" src="https://github.com/user-attachments/assets/535855cf-17de-4f95-a6ff-0b167dcf6fdd" />

5. So, let’s delete everything and deploy through helm chart.

   - *kubectl delete deploy go-web-app*

<img width="846" height="124" alt="Screenshot 2025-09-21 at 4 42 19 PM" src="https://github.com/user-attachments/assets/8d8ecfd6-33e2-43a2-8eba-bb6e15999bbe" />

Run below command to delete service.

 - *kubectl delete svc go-web-app*

<img width="792" height="88" alt="Screenshot 2025-09-21 at 4 42 39 PM" src="https://github.com/user-attachments/assets/f60e9e38-7dc5-45b3-9f80-3aee45327345" />

Run below command to delete ingress.

 - *kubectl delete ing go-web-app*
   
<img width="770" height="83" alt="Screenshot 2025-09-21 at 4 42 53 PM" src="https://github.com/user-attachments/assets/76cfabf9-7e33-448c-982d-a28d04b7c390" />

Now after this, if you will run below command, you will see nothing.
 - *kubectl get all*

<img width="690" height="85" alt="Screenshot 2025-09-21 at 4 43 12 PM" src="https://github.com/user-attachments/assets/1f9b2a90-4d3f-4bed-99e3-0008952d7440" />

Now we will install these from heml chart using below command:
 - *helm install go-web-app ./go-web-app-chart*
   
<img width="977" height="160" alt="Screenshot 2025-09-21 at 4 44 05 PM" src="https://github.com/user-attachments/assets/3b0ae6b0-c61e-4cba-8a34-9fc0d5ea9fcb" />

This will deploy my app, service and ingress. You can check by running below commands:
 - *kubectl get deployment*
 - *kubectl get svc*
 - *kubectl get ing*

<img width="1218" height="224" alt="Screenshot 2025-09-21 at 4 44 49 PM" src="https://github.com/user-attachments/assets/1c51c14b-8847-4d49-8ced-a087151ce4bc" />

You can cross check, by going to go-web-app file from below command:
 - *kubectl edit deploy go-web-app*

<img width="454" height="160" alt="Screenshot 2025-09-21 at 4 45 51 PM" src="https://github.com/user-attachments/assets/b0a2c1af-ac35-4d0b-8b5b-b8d66f0133bd" />

Now un-install everything and now we will create a proper CI-CD part of project so for uninstalling everything, run below command and verify.
 - *helm uninstall go-web-app*
 - *kubectl get all*
   
<img width="843" height="119" alt="Screenshot 2025-09-21 at 4 46 39 PM" src="https://github.com/user-attachments/assets/410e2003-193d-4694-92ae-abb329463e36" />

6. Now, first we will create CI part of our deployment on Github Action:
Create a .github folder and inside that create workflows folder and inside that, create ci.yaml file for CI part. You can find in repo that you clone above. Whenever you push the code this yaml code will run and create a pipeline and build our application and push new docker image inside dockerhub with new version tag.

<img width="846" height="616" alt="Screenshot 2025-09-23 at 7 43 12 AM" src="https://github.com/user-attachments/assets/91f76eab-4b3f-4048-92ea-0318f7cf47b3" />

Now create a repository inside your GitHub with any name you want and go to Settings > Secrets and variables > click on new repository secret.

Now here you need to create three secrets, as DOCKERHUB_TOKEN, DOCKERHUB_USERNAME, TOKEN.

<img width="1415" height="354" alt="Screenshot 2025-09-23 at 7 45 14 AM" src="https://github.com/user-attachments/assets/25b33f7a-89c1-46e1-b78b-be2af0ca1aa1" />

In DOCKERHUB_TOKEN = your dockerhub password

DOCKERHUB_USERNAME = your dockerhub username

For token, you need to provide GitHub Api as your code need to run in GitHub runner, so it needs access of your repository.

go to profile > settings > developer settings.

<img width="344" height="570" alt="Screenshot 2025-09-23 at 7 46 18 AM" src="https://github.com/user-attachments/assets/4e68923a-6440-46e3-9157-942ff7c0cd86" />

Click on Tokens classic and click on generate new token
Copy your key by allowing all permission inside Token secrets.

<img width="1298" height="419" alt="Screenshot 2025-09-23 at 7 47 12 AM" src="https://github.com/user-attachments/assets/12c38d9a-da4f-4e67-923c-945886888f98" />

Now when you will push this code, workflow that you have write in .github folder will start CI pipeline and build our docker image with new tag and push that image to dockerhub.

<img width="1903" height="813" alt="Screenshot 2025-09-23 at 10 22 10 AM" src="https://github.com/user-attachments/assets/0c409de9-481f-47b7-b568-68ea3a912b06" />


<img width="904" height="180" alt="Screenshot 2025-09-23 at 10 23 08 AM" src="https://github.com/user-attachments/assets/63ba86ac-c3de-4985-9f4b-86534bf51514" />


You can verify this here on Dockerhub.

7. Now as CI part is done, so let’s start CD part with help of ArgoCD. As for ArgoCD , the only source of truth is github so that’s why we use github action for CI part.

Run below command into your local terminal, to create namespace for argocd :

 - *kubectl create namespace argocd*

<img width="895" height="78" alt="Screenshot 2025-09-23 at 10 23 33 AM" src="https://github.com/user-attachments/assets/578daba3-3252-4471-9f63-743355b0b5ce" />

Run this command for install argocd or create pod of argocd:
 - *kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml*

<img width="1243" height="222" alt="Screenshot 2025-09-23 at 10 24 11 AM" src="https://github.com/user-attachments/assets/c8d84eab-49dc-42cb-90bb-96b079304b32" />

Run this command to edit service file of argocd and change ClusterIP to Loadbalanacer so it will take loadbalancer ip.
 - *kubectl patch svc argocd-server -n argocd -p "{\"spec\": {\"type\": \"LoadBalancer\"}}"*
   
<img width="1236" height="79" alt="Screenshot 2025-09-23 at 10 24 58 AM" src="https://github.com/user-attachments/assets/50a7e557-f4eb-40ed-b283-760a421005b7" />

Now run below command to get ip of argocd pod :
 - *kubectl get svc argocd-server -n argocd*

<img width="1239" height="135" alt="Screenshot 2025-09-23 at 10 25 26 AM" src="https://github.com/user-attachments/assets/fc0d08ef-6025-4a2f-8d41-d5135ee3cb82" />

Paste that ExternalIP inside your browser , and it will open argocd UI.

<img width="1149" height="912" alt="Screenshot 2025-09-23 at 10 26 14 AM" src="https://github.com/user-attachments/assets/71fd1e32-ef85-4e28-8c6f-1ce9a93ed083" />

So, as username is fixed as Admin, you need password, for that run below command :
 - *kubectl get secrets -n argocd*

<img width="916" height="136" alt="Screenshot 2025-09-23 at 10 26 38 AM" src="https://github.com/user-attachments/assets/148f7cf6-de40-4a30-aecd-40ceeed2e907" />

Run below command for getting password :
 - *kubectl edit secrets argocd-initial-admin-secret -n argocd*

<img width="1158" height="83" alt="Screenshot 2025-09-23 at 10 27 27 AM" src="https://github.com/user-attachments/assets/ab64e8c6-4244-49ef-b35f-f6a86e05f17a" />

Copy the password and decrypt that code as it is in base64 format. For decode that, run below command :
 - *echo Ung4eFh1Nk05T1RRUUstYg== | base64 --decode*

<img width="928" height="342" alt="Screenshot 2025-09-23 at 11 25 55 AM" src="https://github.com/user-attachments/assets/31547692-acbb-4a0f-8164-2cf1dcb6db25" />

<img width="1145" height="90" alt="Screenshot 2025-09-23 at 10 28 01 AM" src="https://github.com/user-attachments/assets/95f2f2b6-5c19-4692-863f-dd92fa3d2c97" />

Copy the password before the base64 only.
Copy paste that password inside ArgoCD, it will open such UI :

<img width="1153" height="603" alt="Screenshot 2025-09-23 at 10 28 34 AM" src="https://github.com/user-attachments/assets/7c09d628-67ac-45ad-be26-219ddea113ea" />

Click on New App.

 - Give Application name : go-web-app { you can give whatever you want }
 - Select project name > default
 - Select SYNC POLICY > self-heal

<img width="908" height="513" alt="Screenshot 2025-09-23 at 10 29 30 AM" src="https://github.com/user-attachments/assets/02885138-2ec8-41d8-bc89-22a471e6d9df" />

Now come to SOURCE > give your repository URL, it will detect automatically helm chart.

<img width="910" height="516" alt="Screenshot 2025-09-23 at 10 30 37 AM" src="https://github.com/user-attachments/assets/5ca81a64-acdd-477b-8aa4-347b2f42e3ba" />

In path section > select default cluster url and in Namespace , write default.

<img width="873" height="269" alt="Screenshot 2025-09-23 at 10 31 12 AM" src="https://github.com/user-attachments/assets/61ec7688-e1ac-4417-bb87-378f47756294" />

Before this, you can check that, there is no deployment and svc is running right now.

 - *kubectl get all*

<img width="787" height="100" alt="Screenshot 2025-09-23 at 10 31 35 AM" src="https://github.com/user-attachments/assets/0414e5b1-99ba-4fe9-90d7-f8acba0901c1" />

In HELM, select values.yaml file and then click on create.

<img width="881" height="322" alt="Screenshot 2025-09-23 at 10 32 18 AM" src="https://github.com/user-attachments/assets/743d824a-1c7a-4a7a-bad8-bd6219924c24" />

After click on create , it will sysnc application and tell you the status. If everthing is good, you will see status as Healthy and Synced.

<img width="1068" height="546" alt="Screenshot 2025-09-23 at 10 34 00 AM" src="https://github.com/user-attachments/assets/377f72bb-bdfe-48ec-9740-4da15e343345" />

<img width="1903" height="628" alt="Screenshot 2025-09-23 at 10 34 14 AM" src="https://github.com/user-attachments/assets/86228f0f-a83b-49e0-a20c-6910fd188d4e" />

That’s it. You have successfully devopsified a go language-based web-app.

Now whenever you update your github repo, whole CI-CD will run on github action and argocd.

For destroying all resources , run below command :

 - *eksctl delete cluster --name demo-cluster --region us-east-1*

<img width="1237" height="191" alt="Screenshot 2025-09-23 at 10 35 26 AM" src="https://github.com/user-attachments/assets/f8d0faff-a23b-4cc7-9d19-0e1a5ea98970" />

