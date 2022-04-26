<div align="center" padding=25px>
    <img src="images/confluent.png" width=50% height=50%>
</div>

# <div align="center">Microservices with C# and .NET</div>
## <div align="center">Lab Guide</div>
<br>

## **Agenda**

***

## **Architecture**

<div align="center">
    <img src="images/architecture.png" width=75% height=75%>
</div>

*** 

## **Prerequisites**
<br>

1. .NET 5 or above - [Download .NET 5.0 (Linux, macOS, and Windows)](https://dotnet.microsoft.com/download/dotnet/5.0)

1. Visual Studio Code (Optional) - [Download Visual Studio Code (Mac, Linux, Windows)](https://code.visualstudio.com/Download)

1. Confluent Cloud Account
    - Sign-up for a Confluent Cloud account [here](https://www.confluent.io/confluent-cloud/tryfree/)
    - Once you have signed up and logged in, click on the menu icon at the upper right hand corner, click on "Billing & payment", then enter payment details under “Payment details & contacts”. A screenshot of the billing UI is included below.

    > **Note:** You will create resources during this workshop that will incur costs. When you sign up for a Confluent Cloud account, you will get free credits to use in Confluent Cloud. This will cover the cost of resources created during the workshop. More details on the specifics can be found [here](https://www.confluent.io/confluent-cloud/tryfree/).

1. Ports 443 and 9092 need to be open to the public internet for outbound traffic. To check, try accessing the following from your web browser:
    - portquiz.net:443
    - portquiz.net:9092

1. This workshop requires access to a command line interface.
    * **Mac users:** The standard Terminal application or iTerm2 are recommended.
    * **Windows users:** The built-in Command Prompt or Git BASH are recommended.  

1. Git access, see [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) for installation instructions. After installation, verify that the installation was successful with the following command:
    ```bash
    # Check the git version
    git --version
    ```
1. Clone Confluent's Commercial SE workshop repository to your machine to access useful files. 
    > **Note:** This repository contains **all** of the workshops and workshop series Confluent's Commercial SE team has created. Be sure to navigate to the correct sub-folder to use the right content.
    ```bash
    # clone the Commercial SE workshop repository
    git clone https://github.com/confluentinc/commercial-workshops.git
    ```
    Navigate to the correct sub-folder to access this labs content. This should act as your working directory for the remainder of the lab. 
    ```bash 
    # navigate to the correct sub-folder
    cd commercial-workshops/series-getting-started-with-cc/workshop-connectors/
    ```

***

## **Objective:**

## <a name="step-1"></a>**Step 1: Log in to Confluent Cloud**
1. Log in to [Confluent Cloud](https://confluent.cloud) and enter your email and password.

<div align="center" padding=25px>
    <img src="images/login.png" width=50% height=50%>
</div>

2. If you are logging in for the first time, you will see a self-guided wizard that walks you through spinning up a cluster. Please minimize this as you will walk through those steps in this workshop. 

*** 

## <a name="step-2"></a>**Step 2: Create an Environment and Cluster**

An environment contains clusters and its deployed components such as Connectors, ksqlDB, and Schema Registry. You have the ability to create different environments based on your company's requirements. Confluent has seen companies use environments to separate Development/Testing, Pre-Production, and Production clusters.

1. Click **+ Add Environment**. Specify an **Environment Name** and Click **Create**. 

    >**Note:** There is a *default* environment ready in your account upon account creation. You can use this *default* environment for the purpose of this workshop if you do not wish to create an additional environment.

<div align="center" padding=25px>
    <img src="images/environment.png" width=50% height=50%>
</div>

2. Now that you have an environment, click **Create Cluster**. 

    > **Note:** Confluent Cloud clusters are available in 3 types: Basic, Standard, and Dedicated. Basic is intended for development use cases so you will use that for the workshop. Basic clusters only support single zone availability. Standard and Dedicated clusters are intended for production use and support Multi-zone deployments. If you are interested in learning more about the different types of clusters and their associated features and limits, refer to this [documentation](https://docs.confluent.io/current/cloud/clusters/cluster-types.html).

3. Choose the **Basic** Cluster Type. 

<div align="center" padding=25px>
    <img src="images/cluster-type.png" width=50% height=50%>
</div>

4. Click **Begin Configuration**.
5. Choose your preferred Cloud Provider (AWS, GCP, or Azure), Region, and Availability Zone.
6. Specify a **Cluster Name** - any name will work here. 

<div align="center" padding=25px>
    <img src="images/create-cluster.png" width=50% height=50%>
</div>

7. View the associated Configuration & Cost, Usage Limits, and Uptime SLA information before launching.

8. Click **Launch Cluster.**

<div align="center" padding=25px>
    <img src="images/launch-cluster.png" width=50% height=50%>
</div>

## <a name="step-3"></a>**Step 3: Create a ksqlDB Application**

1. On the navigation menu, select **ksqlDB** and click **Create Application Myself**. 
2. Select **Global Access** and then **Continue**.
3. Name you ksqlDB application and set the streaming units to **1**. Click **Launch Application!**

> **Note:** A Confluent Streaming Unit is the unit of pricing for Confluent Cloud ksqlDB. A CSU is an abstract unit that represents the size of your kSQL cluster and scales linearly. 

<div align="center" padding=25px>
    <img src="images/create-application.png" width=50% height=50%>
</div>

## <a name="step-4"></a>**Step 4: Create a Topic and Cloud Dashboard Walkthrough**

1. On the left hand side navigation menu, you will see **Cluster**.

    This section shows Cluster Metrics, such as Throughput and Storage. This page also shows the number of Topics, Partitions, Connectors, and ksqlDB Applications.  Below is an example of the metrics dashboard once you have data flowing through Confluent Cloud. 

<div align="center" padding=25px>
    <img src="images/cluster-overview.png" width=50% height=50%>
</div>

2. Click on **Settings**. This is an important tab that should be noted. This is where you can find your cluster ID, bootstrap server (also known as broker endpoint), cloud details, cluster type, and capacity limits. 
3. Copy and save the bootstrap server - you will use it later in the workshop.
4. On that same navigation menu, select **Topics** and click **Create Topic**. 
5. Enter **pageviews** as the Topic name and **1** as the Number of partitions, then click on **Create with defaults**.
    <div align="center" padding=25px>
       <img src="images/new-topic.png" width=50% height=50%>
    </div>

    > **Note:** Topics have many configurable parameters that dictate how messages are handled. A complete list of those configurations for Confluent Cloud can be found [here](https://docs.confluent.io/cloud/current/using/broker-config.html).  If you are interested in viewing the default configurations, you can view them in the Topic Summary on the right side. 

6. After creation, the **Topics UI** allows you to monitor production and consumption throughput metrics and the configuration parameters for your topics. When you begin sending messages to Confluent Cloud, you will be able to view those messages and message schemas. 

7. Below is a look at your topic, pageviews, but you need to send data to this topic before you see any metrics. 
    <div align="center" padding=25px>
       <img src="images/topic-overview.png" width=50% height=50%>
    </div>

## <a name="step-5"></a>**Step 5: Create an API Key Pair**

1. Select **API keys** on the navigation menu. 
2. If this is your first API key within your cluster, click **Create key**. If you have set up API keys in your cluster in the past and already have an existing API key, click **+ Add key**.
    <div align="center" padding=25px>
       <img src="images/create-cc-api-key.png" width=50% height=50%>
    </div>

3. Select **Global Access**, then click Next.
4. Save your API key and secret - you will need these during the workshop.
5. After creating and saving the API key, you will see this API key in the Confluent Cloud UI in the **API keys** tab. If you don’t see the API key populate right away, refresh the browser. 

## <a name="step-6"></a>**Step 6: Enable Schema Registry**

A topic contains messages, and each message is a key-value pair. The message key or the message value (or both) can be serialized as JSON, Avro, or Protobuf. A schema defines the structure of the data format. 

Confluent Cloud Schema Registry is used to manage schemas and it defines a scope in which schemas can evolve. It stores a versioned history of all schemas, provides multiple compatibility settings, and allows schemas to evolve according to these compatibility settings. It is also fully-managed.

1. Return to your environment by clicking on the Confluent icon at the top left corner and then clicking your environment tile.
  <div align="center">
      <img src="images/sr-cluster.png" width=75% height=75%>
  </div>

2. Click on **Schema Registry**. Select your cloud provider and region, and then click on **Enable Schema Registry**.
  <div align="center">
      <img src="images/sr-tab.png" width=75% height=75%>
  </div>

## <a name="step-7"></a>**Step 7: Connect C# Producer and Consumer to Confluent Cloud**

### Setup

1. To begin setting up **client**, you should have already cloned the repository during the prerequisites step. If you have not, start by cloning the [csharp client](https://github.com/hendrasutanto/csharp-clients) GitHub repository.
    > **Note:** you can skip this step if you have already cloned the repository during the prerequisites step.
    ```bash
    git clone https://github.com/hendrasutanto/csharp-clients
    ```
1. Change directory to the csharp-client directory.
    ```bash
    cd csharp-clients
    ```
1. The next step is to replace the placeholder values surrounded in angle brackets within `csharp.config` with configuration parameters to connect to your Kafka cluster. For reference, use the following table to fill out all the values completely.

    | property               | created in step                         |
    |------------------------|-----------------------------------------|
    | `BROKER_ENDPOINT`      | [*create an environment and cluster*](#step-2) |
    | `CLUSTER_API_KEY`      | [*create an api key pair*](#step-5) |
    | `CLUSTER_API_SECRET`   | [*create an api key pair*](#step-5) |
    
### Produce Records

1. Build the client example application
    ```bash
    dotnet build
    ```
1. Run the example application, passing in arguments for:
   - whether to produce or consume (produce)
   - the topic name (pageviews)
   - the local file with configuration parameters to connect to your Kafka cluster
   - Windows only: a local file with default trusted root CA certificates
    ```bash
    # Run the producer (Windows)
    dotnet run produce pageviews csharp.config /path/to/curl/cacert.pem
    
    # Run the producer (other)
    dotnet run produce pageviews csharp.config
    ```
1. Verify that the producer sent all the messages. You should see:
    ```bash
    Producing record: 10 {"viewtime":10,"userid":"User_3","pageid":"Page_44"}
    Producing record: 20 {"viewtime":20,"userid":"User_4","pageid":"Page_60"}
    Producing record: 30 {"viewtime":30,"userid":"User_2","pageid":"Page_63"}
    Producing record: 40 {"viewtime":40,"userid":"User_2","pageid":"Page_73"}
    Producing record: 50 {"viewtime":50,"userid":"User_6","pageid":"Page_53"}
    Producing record: 60 {"viewtime":60,"userid":"User_7","pageid":"Page_57"}
    Producing record: 70 {"viewtime":70,"userid":"User_4","pageid":"Page_15"}
    Producing record: 80 {"viewtime":80,"userid":"User_4","pageid":"Page_32"}
    Producing record: 90 {"viewtime":90,"userid":"User_8","pageid":"Page_28"}
    Producing record: 100 {"viewtime":100,"userid":"User_2","pageid":"Page_45"}
    Produced message to: pageviews [[0]] @0
    Produced message to: pageviews [[0]] @1
    Produced message to: pageviews [[0]] @2
    Produced message to: pageviews [[0]] @3
    Produced message to: pageviews [[0]] @4
    Produced message to: pageviews [[0]] @5
    Produced message to: pageviews [[0]] @6
    Produced message to: pageviews [[0]] @7
    Produced message to: pageviews [[0]] @8
    Produced message to: pageviews [[0]] @9
    10 messages were produced to topic pageviews
    ```
1. View the [producer code](https://github.com/hendrasutanto/csharp-clients/blob/main/Program.cs)

### Consume Records

1. Run the example application, passing in arguments for:
   - whether to produce or consume (produce)
   - the topic name: same topic name as used above
   - the local file with configuration parameters to connect to your Kafka cluster
   - Windows only: a local file with default trusted root CA certificates
    ```bash
    # Run the consumer (Windows)
    dotnet run consume pageviews csharp.config /path/to/curl/cacert.pem
    
    # Run the consumer (other)
    dotnet run consume pageviews csharp.config
    ```
1. Verify that the consumer sent all the messages. You should see:
    ```bash
    Consumed record with key 10 and value {"viewtime":10,"userid":"User_3","pageid":"Page_44"}
    Consumed record with key 20 and value {"viewtime":20,"userid":"User_4","pageid":"Page_60"}
    Consumed record with key 30 and value {"viewtime":30,"userid":"User_2","pageid":"Page_63"}
    Consumed record with key 40 and value {"viewtime":40,"userid":"User_2","pageid":"Page_73"}
    Consumed record with key 50 and value {"viewtime":50,"userid":"User_6","pageid":"Page_53"}
    Consumed record with key 60 and value {"viewtime":60,"userid":"User_7","pageid":"Page_57"}
    Consumed record with key 70 and value {"viewtime":70,"userid":"User_4","pageid":"Page_15"}
    Consumed record with key 80 and value {"viewtime":80,"userid":"User_4","pageid":"Page_32"}
    Consumed record with key 90 and value {"viewtime":90,"userid":"User_8","pageid":"Page_28"}
    Consumed record with key 100 and value {"viewtime":100,"userid":"User_2","pageid":"Page_45"}
    ```
1. When you are done, press `<ctrl>-c`.

1. View the [consumer code](https://github.com/hendrasutanto/csharp-clients/blob/main/Program.cs)

### Troubleshooting: Configure SSL trust store

Depending on your operating system, there may be complexity around the `SslCaLocation` config, which specifies the path to your SSL CA root certificates. If your system doesn’t have the SSL CA root certificates properly set up, you may receive a `SSL handshake failed` error message similar to the following:
```bash
%3|1605776788.619|FAIL|rdkafka#producer-1| [thrd:sasl_ssl://...confluent.cloud:9092/bootstr]: sasl_ssl://...confluent.cloud:9092/bootstrap: SSL handshake failed: error:14090086:SSL routines:ssl3_get_server_certificate:certificate verify failed: broker certificate could not be verified, verify that ssl.ca.location is correctly configured or root CA certificates are installed (brew install openssl) (after 258ms in state CONNECT)
```
For further troubleshooting information, see [Configure SSL trust store](https://docs.confluent.io/platform/current/tutorials/examples/clients/docs/csharp.html?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud#configure-ssl-trust-store).

## <a name="step-8"></a>**Step 8: Launch Fully-Managed Datagen Source Connector in Confluent Cloud**

## **Confluent Resources and Further Testing**

Here are some links to check out if you are interested in further testing:
- [ksqlDB Tutorials](https://kafka-tutorials.confluent.io/)
- [ksqlDB: The Event Streaming Database, Purpose-Build for Stream Processing](https://ksqldb.io/)
- [Streams and Tables in Apache Kafka: A Primer](https://www.confluent.io/blog/kafka-streams-tables-part-1-event-streaming/)
- [Confluent Cloud Documentation](https://docs.confluent.io/cloud/current/overview.html)
- [Best Practices for Developing Apache Kafka Applications on Confluent Cloud](https://assets.confluent.io/m/14397e757459a58d/original/20200205-WP-Best_Practices_for_Developing_Apache_Kafka_Applications_on_Confluent_Cloud.pdf)
- [Confluent Cloud Demos and Examples](https://docs.confluent.io/platform/current/tutorials/examples/ccloud/docs/ccloud-demos-overview.html)
- [Kafka Connect Deep Dive – Error Handling and Dead Letter Queues](https://www.confluent.io/blog/kafka-connect-deep-dive-error-handling-dead-letter-queues/)
