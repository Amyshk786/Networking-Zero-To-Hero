
# What is System Design?

We watch videos on YouTube, so YouTube has a User Interface (UI) where Users can watch videos. It also has a Video Upload service where videos can be uploaded by the Influencers. These videos are uploaded, then they are stored in a Database, and then they can only be viewed by the Users. [UI, Video Upload, Database Service]


If we open YT in Incognito, it asks us to provide our email ID, password, and authenticate. Once we authenticate, only then will we be able to access the UI and watch the videos, so we require an Authentication service as well. [Authentication Service]


When our favourite YouTuber uploads a video, we should be notified on our mobile phone that he has uploaded the video [Notification Service]


In the above example, we have seen that YouTube is not just an application, it's a System which has Multiple Services




# Why System Design?

- Cost is Involved
- Scale is Involved
- Data Flow Design is Involved
- Requirements are Clear

--------------------------


# What is System Design?

## 1. What is a System?

A **system** is a collection of different components that work together to achieve a common goal.

For example, YouTube is an application where users can watch videos and creators can upload videos.

Behind the scenes, multiple components work together:

- User Interface (UI)
- Authentication Service
- Video Upload Service
- Video Processing Service
- Video Service
- Database
- Object Storage
- Search Service
- Notification Service

All these components work together to provide one application experience.

**In simple words:** An application may look like a single product to users, but internally it can consist of many services and components.

---

## 2. What is System Design?

**System Design is the process of planning how different components of an application will work together to achieve a specific goal.**

It involves deciding:

- What components or services are required?
- How will users interact with the application?
- How will services communicate with each other?
- Where will application data be stored?
- How will the application handle increasing traffic?
- How will security be implemented?
- What happens if a service or server fails?
- How much infrastructure will be required?

In simple words:

System Design means planning the architecture of an application before or while building it.

---

## 3. Understanding System Design Using YouTube

Let's understand how YouTube works from a System Design perspective.

Imagine we want to build a basic version of YouTube.

### Step 1: Identify the Requirements

Before designing the application, we need to understand what it should do.

#### Functional Requirements

Functional requirements describe **what the system should do**.

For YouTube:

1. Users should be able to register and log in.
2. Users should be able to watch videos.
3. Creators should be able to upload videos.
4. Users should be able to search for videos.
5. Users should be able to subscribe to channels.
6. Users should receive notifications when subscribed channels upload videos.

#### Non-Functional Requirements

Non-functional requirements describe **how well the system should work**.

Examples:

- Performance: Videos should load quickly.
- Scalability: The application should support millions of users.
- Availability: Users should be able to access the application reliably.
- Security: User accounts and data should be protected.
- Reliability: The system should handle failures gracefully.

---

### Step 2: Identify the Components and Services

Now that we understand the requirements, let's identify the components required.

| Component | Responsibility |
|---|---|
| Frontend / UI | Displays the YouTube application |
| Authentication Service | Handles login and user authentication |
| Video Upload Service | Accepts uploaded videos |
| Video Processing Service | Converts videos into different formats and resolutions |
| Video Service | Manages video information and playback requests |
| Database | Stores user, video and subscription information |
| Object Storage | Stores actual video files |
| Search Service | Helps users find videos |
| Notification Service | Sends notifications to users |

**Important:** A database and object storage are not necessarily the same thing.

A database stores video metadata, such as:

- Video ID
- Video title
- Creator ID
- Upload date
- Video storage location

The actual video file can be stored in object storage such as Amazon S3.

---

### Step 3: Understand How Components Communicate

When a user opens YouTube, several things happen behind the scenes.

1. The user opens YouTube in a browser or mobile application.
2. The frontend loads.
3. The frontend sends requests to backend services.
4. Backend services process those requests.
5. Services communicate with databases or other services when required.
6. The backend sends responses.
7. The frontend displays the information.

#### Example: User Login

```text
              USER
                |
                v
           YOUTUBE UI
                |
                v
      AUTHENTICATION SERVICE
                |
                v
          USER DATABASE
                |
                v
       LOGIN SUCCESS / FAILURE
                |
                v
            YOUTUBE UI
```

The Authentication Service verifies the user's credentials and establishes an authenticated session or issues tokens.

Note: Incognito mode does not itself require login. It simply starts a separate browser session. YouTube may allow some videos to be watched without signing in.

---

### Step 4: Understand Video Uploading

Imagine your favourite YouTuber uploads a new video.

What happens behind the scenes?

```text
             CREATOR
                |
                v
           YOUTUBE UI
                |
                v
       VIDEO UPLOAD SERVICE
                |
                v
      VIDEO PROCESSING SERVICE
                |
                v
          OBJECT STORAGE
             (Amazon S3)
                |
                v
       VIDEO METADATA DATABASE
```

Let's understand the flow:

1. The creator selects a video and uploads it.
2. Video Upload Service receives the file.
3. Video Processing Service converts the video into different resolutions and formats.
4. The processed video files are stored in object storage.
5. Video information, such as title and creator ID, is stored in a database.
6. The video becomes available for playback after processing and publishing requirements are satisfied.

#### How do users watch videos?

A simplified video playback flow:

```text
              USER
                |
                v
           YOUTUBE UI
                |
                v
          VIDEO SERVICE
                |
                v
          VIDEO DATABASE
                |
                v
          VIDEO LOCATION
                |
                v
           CDN / STORAGE
                |
                v
          VIDEO PLAYBACK
```

A CDN (Content Delivery Network) helps deliver video content from geographically distributed locations, improving performance for users.

---

### Step 5: Understand Notifications

Suppose you subscribe to a YouTube channel.

Your favourite creator uploads a new video.

The system needs to identify subscribers and send notifications.

```text
       CREATOR UPLOADS VIDEO
                |
                v
           VIDEO SERVICE
                |
                v
           MESSAGE QUEUE
                |
                v
       NOTIFICATION SERVICE
                |
                v
       FIND CHANNEL SUBSCRIBERS
                |
                v
      PUSH NOTIFICATION PROVIDER
                |
                v
       USER'S MOBILE PHONE
```

The Notification Service may use a message queue to process notifications asynchronously.

Examples of messaging technologies:

- Amazon SQS
- Apache Kafka
- RabbitMQ

#### Why do we need a Message Queue?

Imagine a creator has 10 million subscribers.

Sending 10 million notifications directly during the video upload request could make the upload process slow.

Instead:

1. The video is published.
2. A notification event is placed into a queue.
3. Notification workers read messages from the queue.
4. Notifications are sent in the background.

This is called **Asynchronous Processing**.

---

## 4. Complete High-Level System Architecture

Let's combine all the components.

```text
                  USERS / CREATORS
                         |
                         v
                   FRONTEND / UI
                         |
                         v
                    API GATEWAY
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
   AUTHENTICATION   VIDEO SERVICE   SEARCH SERVICE
      SERVICE            |              |
          |              v              v
          v         VIDEO DATABASE   SEARCH INDEX
     USER DATABASE       |
                         v
                   OBJECT STORAGE
                         |
                         v
                        CDN


CREATOR UPLOAD
      |
      v
VIDEO UPLOAD SERVICE
      |
      v
VIDEO PROCESSING SERVICE
      |
      v
OBJECT STORAGE


VIDEO PUBLISHED
      |
      v
MESSAGE QUEUE
      |
      v
NOTIFICATION SERVICE
      |
      v
USERS' MOBILE PHONES
```

This is a simplified conceptual architecture, not YouTube's actual internal architecture.

---

## 5. Why is System Design Important?

There are several reasons why System Design is important.

### 1. Cost Optimization

Every component has an infrastructure cost.

For example, when hosting an application on AWS, we may pay for:

- EC2 instances
- EKS clusters
- RDS databases
- S3 storage
- Load Balancers
- CloudFront
- Data transfer
- Monitoring services

Poor architecture can result in unnecessary infrastructure costs.

**Example:**

If an application has very low traffic, running 20 large EC2 instances may be unnecessary.

We can start with fewer instances and scale when demand increases.

### 2. Scalability

Scalability means the ability of a system to handle increasing workloads.

Imagine an application initially has 100 users.

After six months, it has 100,000 users.

Will the existing infrastructure handle that traffic?

There are two major scaling approaches.

#### A. Vertical Scaling

Vertical scaling means increasing the capacity of an existing server.

Example:

- Before: 2 vCPUs and 4 GB RAM
- After: 8 vCPUs and 32 GB RAM

```text
       BEFORE                 AFTER

    +---------+            +-------------+
    | SERVER  |            |   SERVER    |
    | 2 CPU   |            |   8 CPU     |
    | 4 GB    |            |   32 GB     |
    +---------+            +-------------+
```

#### B. Horizontal Scaling

Horizontal scaling means adding more servers to distribute the workload.

Example:

Instead of relying on one application server, we run five application servers behind a Load Balancer.

```text
                 USERS
                   |
                   v
              LOAD BALANCER
                   |
          +--------+--------+
          |        |        |
          v        v        v
       SERVER 1 SERVER 2 SERVER 3
          |
       SERVER 4
          |
       SERVER 5
```

Horizontal scaling is commonly used in cloud-native and distributed applications.

### 3. Data Flow Design

Data Flow means understanding how information moves from one component to another.

Example: User Login

```text
USER
  |
  v
FRONTEND
  |
  v
API GATEWAY
  |
  v
AUTHENTICATION SERVICE
  |
  v
DATABASE
  |
  v
AUTHENTICATION RESPONSE
  |
  v
FRONTEND
```

We need to understand:

- Who sends the request?
- Which service receives it?
- Where is the data stored?
- Which service processes it?
- What response is returned?

Understanding data flow helps engineers identify bottlenecks and failures.

### 4. Clear Requirements

Before designing a system, engineers must understand what they are building.

Consider two applications.

**Application A: Personal Blog**

- 100 users per day.
- Users can read articles.
- Only an administrator can publish articles.

**Application B: YouTube-like Platform**

- Millions of users.
- Users can upload videos.
- Users can stream videos.
- Creators receive analytics.
- Users receive notifications.

Would both applications need the same architecture?

No.

Their requirements, infrastructure, storage, security and scalability needs are different.

That is why requirements must be understood before designing a system.

---

## 6. How Does System Design Connect With DevOps?

System Design is important for DevOps Engineers because they help deploy, operate, secure, scale and monitor applications.

Suppose developers create five microservices for a video application.

As a DevOps Engineer, you may perform the following tasks:

1. Create Dockerfiles for the services.
2. Build Docker images.
3. Push images to Amazon ECR.
4. Deploy services to Amazon EKS.
5. Configure Kubernetes Services and Ingress.
6. Configure an Application Load Balancer.
7. Provision infrastructure using Terraform.
8. Set up CI/CD using Jenkins or GitHub Actions.
9. Monitor services using Prometheus and Grafana.
10. Configure logging and alerting.

### System Design Components and DevOps Technologies

| System Design Component | DevOps Technology |
|---|---|
| Application Hosting | AWS EC2 |
| Containerization | Docker |
| Container Orchestration | Kubernetes / EKS |
| Infrastructure Provisioning | Terraform |
| Configuration Management | Ansible |
| CI/CD Automation | Jenkins / GitHub Actions |
| Load Balancing | AWS ALB |
| Object Storage | Amazon S3 |
| Database Hosting | Amazon RDS |
| Monitoring | Prometheus / Grafana |
| Logging | ELK Stack |
| GitOps Deployment | Argo CD |
| Networking | VPC, Subnets, Route Tables |

System Design helps DevOps Engineers understand the complete application architecture instead of focusing only on individual tools.

---

## 7. Important System Design Terminology

| Term | Meaning |
|---|---|
| Client | Application or device making a request |
| Server | Processes requests and provides responses |
| Frontend | User-facing part of an application |
| Backend | Server-side application logic |
| API | Interface through which systems communicate |
| API Gateway | Entry point that routes API requests |
| Database | Stores application data |
| Cache | Stores frequently accessed data for faster retrieval |
| Load Balancer | Distributes incoming traffic across servers |
| Microservices | Application divided into independently deployable services |
| Monolithic Architecture | Application built and deployed as one unit |
| CDN | Delivers content from distributed locations |
| Message Queue | Holds messages for asynchronous processing |
| High Availability | Keeps a system accessible despite failures |
| Fault Tolerance | Allows a system to continue operating when components fail |
| Scalability | Ability to handle increasing workloads |
| Latency | Time taken for a request or operation |
| Throughput | Amount of work processed in a given time |

---

## 8. System Design Learning Roadmap

Learn System Design step by step.

### Phase 1: Networking Fundamentals

- IP Addressing
- IPv4 vs IPv6
- DNS
- HTTP vs HTTPS
- TCP vs UDP
- OSI Model
- Ports and Protocols
- Firewalls
- Load Balancers

### Phase 2: Application Fundamentals

- Client-Server Architecture
- Frontend vs Backend
- APIs
- REST APIs
- Databases
- SQL vs NoSQL

### Phase 3: System Design Building Blocks

- Load Balancing
- Caching
- CDN
- Message Queues
- Reverse Proxy
- API Gateway
- Object Storage

### Phase 4: Architecture Patterns

- Monolithic Architecture
- Microservices Architecture
- Synchronous Communication
- Asynchronous Communication
- Event-Driven Architecture

### Phase 5: Scalability and Reliability

- Vertical Scaling
- Horizontal Scaling
- Database Replication
- Database Sharding
- High Availability
- Fault Tolerance
- Redundancy
- Disaster Recovery

### Phase 6: Cloud System Design

- AWS VPC
- EC2
- Application Load Balancer
- Auto Scaling
- RDS
- S3
- CloudFront
- EKS
- Route 53

### Phase 7: Practical Projects

Build and design:

1. URL Shortener
2. E-commerce Application
3. Three-Tier Web Application
4. Video Streaming Platform
5. Notification System

---

## 9. Key Takeaways

- A system is a collection of components working together.
- System Design is the process of planning how those components interact.
- Functional requirements describe what the system should do.
- Non-functional requirements describe how well the system should work.
- Scalability, availability, performance, security and cost are important design considerations.
- DevOps Engineers help implement and operate the designed architecture.
- Good System Design starts with understanding requirements, not selecting tools.

**Recommended next topic: Client-Server Architecture and How a Web Application Works.**
