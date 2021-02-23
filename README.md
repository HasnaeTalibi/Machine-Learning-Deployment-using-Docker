# Machine-Learning-Deployment-using-Docker


### An Overview of the process executed  :
provided by the author **Pramod Singh** in his book 'Deploy Machine Learning Models to Production'


1. Train the ML model ( we will train a binary classification 'predict whether the customer will buy the product online or not' ).
2. Save and export the ML model.
3. Create a Flask app including the UI layer.
4. Build a custom Docker image for the app.
5. Run the app using a Docker container.
6. Stop the container.

## 1. Train the ML model
Since the overall idea is to deploy the ML app, the focus is on the containerizing the app instead of improving the accuracy of the model. Hence, we will not cover too much of the feature engineering or complex ML model. Rather, a simple logistic regression or random forest model will suffice 

## 2. Save and export the ML model
Once The model is trained, we need to save it using pickle/joblib in order to reuse it later during predictions

## 3. Create a Flask app including the UI layer
we will build a Flask app along with Flasgger (to handle the UI part of the app to make it more intuitive to consume the results) to deploy an ML model easily

## 4. Build a custom Docker image for the app 
Now it is time to tackle docker,
- We start with providing the base image first to the Docker server that needs to be pulled from Docker Hub (if not present locally already)
   
   **FROM continuumio/anaconda3:4.4.0**

- In the next step, we copy all the files and content from the local directory (where we build our ML model and Flask app) to the Docker directory (we can create a new or current Docker directory as well). In this case, we create a new directory called usr/ML/app.
  
  **COPY . /usr/ML/app**

- The next command is to expose port 5000 of Docker to run this application. Basically, when the request comes from the user, the host system will route this request to port 5000 of Docker where the app will be running. We will also have to do explicit port mapping between the host system and the Docker container while running the container to route the requests properly and access the predictions.
  
  **EXPOSE 5000**

- The next command is to change Docker’s current working directory to the directory where we have copied all the files from the local system
  
  **WORKDIR /usr/ML/app**

- The next step is to install all the dependencies and required libraries in order to run the app successfully. We have a requirement.txt file that contains the required libraries along with versions
   
   **RUN pip install -r requirements.txt**

- The last command in the Dockerfile is always the startup command, and in this case, we want Docker to run the Flask app that we built in the previous step.
  
  **CMD python flask_api.py**
 
 
This list of commands completes our Dockerfile, and it’s now ready to be converted into a Docker image and run as a container.

## 5. Run the app using a Docker container
In this step of the process, we build the Docker custom image from the Dockerfile created in the previous step and run the container.
We have to go to the terminal in the same directory where all the files are present. In the terminal, we run the docker build command to build the Docker image from Dockerfile.
  
  **docker build -t ml_app_docker .**

Once all the commands in the Dockerfile get executed and we have the final image built from the Dockerfile, we can initiate the container to run our ML app.
 
 **docker container run -p 5000:5000 ml_app_docker**
  
--> _To access to the app, we simply have to go to http://127.0.0.1:5000/apidocs to load the Swagger UI page_

## 6. Stop the container.
The last step left after running the application is to stop the running container. This can be done using the docker stop or kill command on the running container. We can see the list of running containers using the docker ps command and can select the running container ID to stop it.
  
  **docker ps**
  
  **docker kill <Container_ID>**

 




