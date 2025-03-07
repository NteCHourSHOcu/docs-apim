# Access the MI Analytics Portal

!!! note

    - The MI Analytics feature has been deprecated. This solution is recommended only for users who are using WSO2 EI 7.0.0 and want to migrate in to a newer version while retaining the already existing analytics data.
    
Let's use **MI Analytics** to view and monitor **statistics** and **message tracing**.

You can monitor the following statistics and more through the MI Analytics Portal:

- Request Count
- Overall TPS
- Overall Message Count
- Top Proxy Services by Request Count
- Top APIs by Request Count
- Top Endpoints by Request Count
- Top Inbound Endpoints by Request Count
- Top Sequences by Request Count

!!! Tip
    Monitoring the usage of the integration runtime using statistical information is very important for understanding the overall health of a system that runs in production. Statistical data helps to do proper capacity planning, to keep the runtimes in a healthy state, and for debugging and troubleshooting problems. When it comes to troubleshooting, the ability to trace messages that pass through the mediation flows of the Micro Integrator is very useful.

## Before you begin

-   Set up the [MI Analytics deployment]({{base_path}}/install-and-setup/setup/mi-setup/observability/setting-up-classic-observability-deployment).

-   Note the following server directory in your deployment.

    <table>
        <tr>
            <th>
                `<MI_ANALYTICS_HOME>`
            </th>
            <td>
                This is the root folder of your MI Analytics installation.
            </td>
        </tr>
    </table>

## Step 1 - Start the servers

Let's start the servers in the given order.

### Step 1.1 - Start the Analytics Server

!!! Note
    Be sure to start the **Analytics** server before [starting the Micro Integrator](#starting-the-micro-integrator).

1.  Open a terminal and navigate to the `<MI_ANALYTICS_HOME>/bin` directory.
2.  Start the Analytics server by executing the following command:

    ```bash tab='On MacOS/Linux/Centos'
    sh server.sh
    ```

    ```bash tab='On Windows'
    server.bat
    ```

### Step 1.2 - Start the Micro Integrator

Once you have [started the Analytics Server](#starting-the-analytics-server), you can [start the Micro Integrator]({{base_path}}/install-and-setup/install/installing-the-product/installing-mi/).

### Step 1.3 - Start the Analytics Portal

1.  Open a terminal and navigate to the `<MI_ANALYTICS_HOME>/bin` directory.
2.  Start the Analytics Portal's runtime by executing the following command:

    ```bash tab='On MacOS/Linux/Centos'
    sh portal.sh
    ```

    ```bash tab='On Windows'
    portal.bat
    ```

In a new browser window or tab, open the Analytics Portal using the following URL: https://localhost:9645/analytics-dashboard. 
Use `admin` for both the username and password.

<img src="{{base_path}}/assets/img/integrate/mi-analytics/dashboard-login.png" width="500">

## Step 2 - Publish statistics to the Portal

Let's **test this solution** by running the [service chaining]({{base_path}}/tutorials/integration-tutorials/exposing-several-services-as-a-single-service) tutorial. When the artifacts deployed in the Micro Integrator are invoked, the statistics will be available in the portal. 

Follow the steps given below.

??? note "Step 1: Deploy integration artifacts"
    
    If you have already started the Micro Integrator server, let's deploy the artifacts. Let's use the integration artifacts from the [service chaining]({{base_path}}/tutorials/integration-tutorials/exposing-several-services-as-a-single-service) tutorial.

    1. Download the [CAR file](https://github.com/wso2-docs/WSO2_EI/blob/master/Analytics/Integration-Artifacts/SampleServicesCompositeExporter_1.0.0.car).
    2. Copy the CAR file to the `<MI_HOME>/repository/deployment/server/carbonapps/` directory.

??? note "Step 2: Start the backend"
    
    Let's start the hospital service that serves as the backend to the [service chaining]({{base_path}}/tutorials/integration-tutorials/exposing-several-services-as-a-single-service) use case: 

    1. Download the JAR file of the back-end service from [here](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/Hospital-Service-JDK11-2.0.0.jar).
    2. Open a terminal, navigate to the location where your saved the back-end service.
    3. Execute the following command to start the service:
        ```bash
        java -jar Hospital-Service-JDK11-2.0.0.jar
        ```

??? note "Step 3: Sending messages"
    
    Let's send 8 requests to the Micro Integrator to invoke the integration artifacts:

    !!! Tip
        For the purpose of demonstrating how successful messages and message failures are illustrated in the portal, let's send 2 of the requests while the back-end service is not running. This should generate a success rate of 75%.

    1.  Create a JSON file called `request.json` with the following request payload.
        ```json
        {
            "name": "John Doe",
            "dob": "1940-03-19",
            "ssn": "234-23-525",
            "address": "California",
            "phone": "8770586755",
            "email": "johndoe@gmail.com",
            "doctor": "thomas collins",
            "hospital": "grand oak community hospital",
            "cardNo": "7844481124110331",
            "appointment_date": "2025-04-02"
        }
        ```

    2.  Open a command line terminal and execute the following command (**six times**) from the location where you save the
        `request.json` file:  
        ```bash
        curl -v -X POST --data @request.json http://localhost:8290/healthcare/categories/surgery/reserve --header "Content-Type:application/json"
        ```
        If the messages are sent successfully, you will receive the following response for each request.
        ```json
        {
            "appointmentNo": 1,
            "doctorName": "thomas collins",
            "patient": "John Doe",
            "actualFee": 7000.0,
            "discount": 20,
            "discounted": 5600.0,
            "paymentID": "e1a72a33-31f2-46dc-ae7d-a14a486efc00",
            "status": "Settled"
        }
        ```

    3.  Now, shut down the back-end service and send two more requests.

## Step 3 - View the Analytics Portal

!!! info
    - The Analytics Portal has been renamed to Micro Integrator Analytics as it contains MI related analytics.
    - You need to get the [latest product updates](https://updates.docs.wso2.com/en/latest/updates/overview/) for your product to view these changes in the current version of WSO2 API-M. This change is available as a product update in Integrator Analytics 7.1.0 from June 18, 2021 onwards.

    !!! note
        You can deploy updates in a production environment only if you have a valid subscription with WSO2. Read more about [WSO2 Updates](https://wso2.com/updates).

Once you have signed in to the Analytics Portal server, click the **Micro Integrator Analytics** icon shown below to open the portal.

[![Opening the Analytics dashboard for the integration component]({{base_path}}/assets/img/integrate/mi-analytics/119132315/mi-dashboard.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/mi-dashboard.png)
      
### Statistics overview

View the statistics overview for all the integration artifacts that have published statistics:  

[![ESB total request count]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132316.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132316.png)

### Transactions per second

The number of transactions handled by the Micro Integrator per second is mapped on a graph as follows.

[![ESB overall TPS]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132326.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132326.png)

### Overall message count
The success rate and the failure rate of the messages received by the Micro Integrator during the last hour are mapped in a graph as follows.  

[![ESB overall message count]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132325.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132325.png)

### Top APIs by request

The `HealthcareAPI` REST API is displayed under **TOP APIS BY REQUEST COUNT** as follows.  

[![Top APIs by request count]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132324.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132324.png)

### Endpoints by request

The three endpoints used for the message mediation are displayed under **Top Endpoints by Request Count** as shown below.  

[![Top endpoints by request count]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132318.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132318.png)

### Per API requests

In the Top APIS BY Request COUNT gadget, click `HealthcareAPI` to open the **OVERVIEW/API/HealthcareAPI** page. The following is displayed.

-   The **API Request Count** gadget shows the total number of
    requests handled by the `StockQuoteAPI`
    REST API during the last hour:  
    [![Total request per API]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132323.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132323.png)
-   The **API** **Message Count** gadget maps the number of
    successful messages as well as failed messages at different
    times within the last hour in a graph as shown below.  
    [![API message count]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132322.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132322.png)
-   The **API** **Message Latency** gadget shows the speed with
    which the messages are processed by mapping the amount of time
    taken per message at different times within the last hour as
    shown below.  
    [![API message latency]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132321.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132321.png) 
-   The **Messages** gadget lists all the the messages handled by
    the `StockQuoteAPI` REST API during the
    last hour with the following property details as follows.  
    [![Message per API]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132320.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132320.png)
-   The **Message Flow** gadget illustrates the order in which the
    messages handled by the `StockQuoteAPI`
    REST API within the last hour passed through all the mediation
    sequences, mediators and endpoints that were included in the
    message flow as shown below.  
    [![Message flow per API]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132319.png)]({{base_path}}/assets/img/integrate/mi-analytics/119132315/119132319.png)

### Per endpoint requests

In the **Top Endpoints by Request Count** gadget, click one of the endpoints to view similar statistics per endpoint.

-   `ChannelingFeeEP`
-   `SettlePaymentEP`
-   `GrandOaksEP`

You can also navigate to any of the artifacts by using the top-left menu as shown below. For example, to view the statistics of a specific endpoint, click **Endpoint** and search for the required endpoint.  

![Dashboard navigation menu]({{base_path}}/assets/img/integrate/mi-analytics/119132315/per-endpoint-requests.png "Dashboard navigation menu")

### Message tracing

When you go to the [Analytics Portal](#step-13-start-the-analytics-portal) the message details will be logged as follows:

![Message tracing per API]({{base_path}}/assets/img/integrate/mi-analytics/119132315/message-tracing.png "Message tracing per API")
