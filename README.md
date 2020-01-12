
# Microservices
An application that takes advantage of micro-service architecture and contenarisation

## Index
[Brief](#brief)
   * [Solution](#brief)
   * [Architecture](#architecture)
   * [Micorservices](#microservices)
   * [Entity Relationship Diagrams](#erd)

[Risk Assessment](#risks)
   * [Risk Sssessment Table](#risks)
   * [Explanations](#risks_expl)

[Deployment](#depl)
   * [Technologies Used](#tech)
   * [Testing](#testing)

[Project planning and user stories](#user_sotry)
   * [Use case diagram](#use_case)
   * [UML diagram](#uml)

[Improvements for the Future](#improve)
[Authors](#auth)

<a name="brief"></a>
## The Brief
The aims of the project are as follows:

* Create a service ortiented web-application, that is comprised of at least four services. Each service must have two different implementation that can be swapped between them without disrupting the user experience.
* Build a CI pipeline using Jenkins that automatically updates and deploys the web-app when source code is updated.
* Create ansible playbook to automate configuation of resourese (VMs) required for this project

## Solution

For my web application I decided to create a quiz game. The app presents the user a randomly generated flag, where the user has to guess which country doesthe flag belong to based on a multiple choice question.
If the country that belongs to the flag is guessed correctly, the user is rewarded with a voucher for a plane ticket.
<a name="architecture"></a>
### Architecture
To create the app, the service were as follow:
<a name="microservices"></a>
<img src="/Documentation/microservice_architecture.png" alt="Microservice architecture" width="80%" height="80%" border="5"/>

#### Country generator service:
   * This service essentianlly genearates the question that will be shown on the frontend. To create the multiple choice question, it generates a random list of unique countires from a josn file. Once the selection has been done, it parses a json package with the countires and a flag from one of those countries.
#### Temperature api service:
  * This service selects a random city from a list of cities and makes an api call to openweathermap.com to get the current temperature. The repsponse is decoded and then parsed to the desired JSON format to be used by service 4.

#### Voucher generator service:
  * Combines the JSON objects recieved from service 2 and 3 with some logic to create a voucher. The current logic set is to genereate a prize based on number of options, set in the quiz, and the current weather.

#### Frontend service:
  * This is the service that the user will be intercating with. The frontend uses the country service to retieve the flag quiz. Upon submission of quiz, if the answer is correct, the forntend then proceeds to make a get request to the voucher service to reward the user. Addionally it also perist some data to an SQL database, such as the response of the user to the question and user's email address used to retrieve the voucher.
<a name="erd"></a>
### Entity Relationship Diagrams
![Entity RelationShip diagram](/Documentation/database_architecture.png)

Altough having an entity relationship diagram was not a requirement for this project, it was useful to revise some database concepts and practice SQLAlchemy's syntax.
The tables above are used by the frontend to store the answer to the quiz given by the user and email address given to retrieve voucher.

<a name="risks" ></a>
## Risk Assessment
| Risks                            | Likelihood    | Impact       |    Explanation          |
| -------------------------------- |:-------------:| :-----------:| -----------------------:|
| Uploading API key to GitHub      | High          | High         | [1 Click here](#api)
| External database manipulation   | Medium        | High/Moderate | [2 Click here](#sql)
| Automation causing issues        | Medium        | High         | [3 Click here](#automation)
| Container losing data when VM stopped | High     | High         | [4 Click here](#container)
| Website malfunctions because code is broken |  Medium     |    High       | [5 Click here](#borken_code)

automation debian and ubuntu vm
<a name="risks_expl" ></a>
a name="risks_expl" ></a>
<a name="api"></a>
#### Uploading API key to GitHub
Service three retrieves the temperature of a random city by making an api request to openweathermap.com.
The website let's me retieve the data required for this project for free, however there also paid services which requires my API key. When writing source code I might upload these key on GitHub my midateks:
Solution:
+ Set the API key as an enviorment variable
<a name="sql"></a>
#### External database manipulationb
During development it is likely that I will be working on different machines, hence there will be a public Git repo. It is very likely that I may upload some credentils by mistake
Solution:
* Set enviornmental variables so that crednetials can be accesed by one person only
* Delete credentials if you know someone else might use the same machine
<a name="automation"></a>
#### Automation causing issues
Automation can save a lot of time and hussle if done right, however if not done properly it can:
* Slow down development time, if the script witten is full of bug. e.g. Jenkins script not cloning down the correct repo, might take time to debug the problem
* When writing Ansible playbook, it is important to write them as versatile as possible, like not having the host name hardcoded but instead one that varies automatically.
Solution:
* Automate only repetitive tasks such as deployment
* Do not automate a task that is not repetitive, such as setting environmental variables
<a name="container"></a>
#### Container losing data when VM stopped
It is important to realise that when a VM is stopped, the docker containers exit. On restart of VM the containers can also be restarted, however they won't be the same as before.
Solution:
* Set container to the option "--restart=always"
* Set volumes to the container
<a name="borken_code"></a>
#### Running out of GCP credit
It is unlikely that I will run out of GCP credit for this project, however it is still a possibility if:
* Leaving multiple instance up and running
* Leaving multiple database open can eat credit very quickly
Solution:
* Run manual test in development enviornment
* Set Jenkins to run automated tests, before it builds and deployes
<a name="depl" ></a>
## Deployment
The test and deployment process for the web app was automated using Jenkins, a CI server. Jenkins run in a GCP instance that automatically deploys the webapp into deployment server, with a webhook to GitHub which is triggered with every push event.
Ansible was used to create VMs with the correct configuration, to automate the process of setting environemtns for production and CI server

Jenkins job:
* Once code is pushed to GitHub, Jenkins is tirggered
* Jenkins downloads executable Jenkinsfile
* It run an automated test on the code
* Build images form yaml file and pushes image to local repo
* Pulls image down on Deplyment server and makes a rolling update.
<img src="/Documentation/CI_pipeline.png" alt="CI Pipeline" width="80%" height="80%" border="5"/>

<a name="tech"></a>
### Technologies Used

* Database: GCP SQL Server
* Programming language: Python
* Framework: Flask
* Deployment: Gunicorn
* CI Server: Jenkins
* Test Reporting: Pytest
* VCS: [Git](https://github.com/hafizpatwary/microservices)
* Project Tracking: [Trello](https://trello.com/b/Edpyk0uq/solo-project-qa)
* Live Environment: GCP
* Containerization: Docker
* Configuration Management: Ansible
* Orchestration: Docker-compose
<a name="testing"></a>
### Testing

<a name="user_sotry"></a>
### Project planning and user stories
<a name="use_case"></a>
<a name="uml"></a>
### Use case diagram
### UML diagram

<a name="improve"></a>
## Improvements for the Future


service 2 accepts parameter of number of question
