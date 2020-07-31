![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Innovate and Modernize Apps with Data & AI
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
July 2020
</div>


Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents** 

<!-- TOC -->

- [Innovate and Modernize Apps with Data & AI hands-on lab step-by-step](#innovate-and-modernize-apps-with-data-and-ai-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Exercise 1: Exercise name](#exercise-1-exercise-name)
        - [Task 1: Task name](#task-1-task-name)
        - [Task 2: Task name](#task-2-task-name)
    - [Exercise 2: Exercise name](#exercise-2-exercise-name)
        - [Task 1: Task name](#task-1-task-name-1)
        - [Task 2: Task name](#task-2-task-name-1)
    - [Exercise 3: Exercise name](#exercise-3-exercise-name)
        - [Task 1: Task name](#task-1-task-name-2)
        - [Task 2: Task name](#task-2-task-name-2)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete Lab Resources](#task-1-delete-lab-resources)

<!-- /TOC -->

# Innovate and Modernize Apps with Data and AI hands-on lab step-by-step 

## Abstract and learning objectives 

In this hands-on-lab, you will build a cloud processing and machine learning solution for IoT data. We will begin by deploying a factory load simulator using Azure IoT Edge to write into Azure IoT Hub, following the recommendations in the [Azure IoT reference architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/iot).  The data in this simulator represents sensor data collected from a stamping press machine, which cuts, shapes, and imprints sheet metal.  **TODO: exercise 1 website and exercise 2 explanation**

Once we have data in Azure IoT Hub, we will use Azure Stream Analytics to aggregate and transform the data, including using the Anomaly Detection service built into Stream Analytics to observe and report on abnormal machine temperature readings.  Stream Analytics will then send data to three different data stores:  Azure Database for PostgreSQL Hyperscale, Azure Cosmos DB, and Azure Data Lake Storage Gen2.  The data in each source will serve different purposes, from driving analytical microservices to performing predictive maintenance to forming the basis for Power BI dashboards.

You will learn how to apply historical machine temperature and stamping pressure values in the creation of a machine learning model to identify potential issues which might require machine adjustment.  You will deploy this predictive maintenance model and generate predictions on simulated stamp press data.

## Overview

The Innovate and Modernize Apps with Data & AI hands-on lab is an exercise that will challenge you to implement an end-to-end scenario using the supplied example that is based on Azure IoT Hub and other related Azure services. The hands-on lab can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

## Solution architecture

\[Insert your end-solution architecture here. . .\]

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN.

    a. Trial subscriptions will not work.

2. Install [Visual Studio Code](https://code.visualstudio.com/).

    a. Install the [Azure IoT Tools extension](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

    b. Install the [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).

    c. Install the [Azure Functions extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions).

3. Install [the Azure Machine Leraning SDK for Python](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/install?view=azure-ml-py).

4. Install [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio).

    a. Install the [PostgreSQL extension](https://docs.microsoft.com/en-us/sql/azure-data-studio/postgres-extension).

5. Install Docker. [Docker Desktop](https://www.docker.com/products/docker-desktop) will work for this hands-on lab and supports Windows and MacOS. For Linux, install the Docker engine through your distribution's package manager.

## Before the hands-on lab

Refer to the Before the hands-on lab setup guide manual before continuing to the lab exercises.

## Exercise 1: Deploy a factory load simulator and website

Duration: 40 minutes

[Azure IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/) is a Microsoft offering which provides secure and reliable communication between IoT devices and cloud services in Azure. The aim of this service is to provide bidirectional communication at scale. The core focus of many industrial companies is not on cloud computing; therefore, they do not necessarily have the personnel skilled to provide guidance and to stand up a reliable and scalable infrastructure for an IoT solution. It is imperative for these types of companies to enter the IoT space not only for the cost savings associated with remote monitoring, but also to improve safety for their workers and the environment.

Wide World Importers is one such company that could use a helping hand entering the IoT space.  They have an existing suite of sensors and on-premises data collection mechanisms at each factory but would like to centralize data in the cloud. To achieve this, we will stand up a IoT Hub and assist them with the process of integrating existing sensors with IoT Hub via [Azure IoT Edge](https://azure.microsoft.com/en-us/services/iot-edge/). A [predictable cost model](https://azure.microsoft.com/en-us/pricing/details/iot-hub/) also ensures that there are no financial surprises.

### Task 1: Add a new device in IoT Hub

The first task is to register a new IoT Edge device in IoT Hub.

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

    If you do not see the resource group in the Recent resources section, type in "resource groups" in the top search menu and then select **Resource groups** from the results.

    ![In the Services search result list, Resource groups is selected.](media/azure-resource-group-search.png 'Resource groups')

    From there, select the **modernize-app** resource group.

2.  Navigate to the IoT Hub you created. The name will start with **modernize-app-iot** and have a Type of **IoT Hub**.

3. In the Automatic Device Management menu, select **IoT Edge**. Then select the **+ Add an IoT Edge device** button to register a new device.

    ![In the IoT Hub menu, IoT Edge is selected, followed by Add an IoT Edge device.](media/azure-add-iot-edge-device.png 'Add an IoT Edge device')

4. In the Create a device menu, name the device `modernize-app-ubuntu1` and leave the remaining settings the same.

    ![In the Create a device menu, the device ID is filled in.](media/azure-add-iot-edge-device.png 'Create a device')

5. Click the **Save** button to complete device registration.

6. Click on the device named `modernize-app-ubuntu1`.

    ![In the IoT Edge devices section, the device named modernize-app-ubuntu1 is selected.](media/azure-modernize-app-ubuntu1.png 'The modernize-app-ubuntu1 IoT device')

7. On the modernize-app-ubuntu1 screen, copy the **Primary Connection String** and paste it into Notepad or another text editor. You will use this connection string in the next task, when configuring a Linux virtual machine to become `modernize-app-ubuntu1`.

    ![In the modernize-app-ubuntu1 settings, the copy action for the Primary Connection String is selected.](media/azure-modernize-app-ubuntu1-cs.png 'The primary connection string for the modernize-app-ubuntu1 IoT device')

### Task 2: Install and configure IoT Edge on a Linux virtual machine

The instructions in this task come from the guide on [how to install IoT Edge on Linux](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux). The instructions in this task are tailored specifically for Ubuntu Server 18.04, but if you wish to install IoT Edge on other variants of Linux, including Rasbian for the Raspberry Pi, the linked article will provide additional support.

1.  Use SSH to connect to the virtual machine you configured before the hands-on lab. If you do not have the IP address of the virtual machine, navigate to the **modernize-app** resource group and search for the name **modernize-app-vm** with a Type of **Virtual machine**.

    ![The virtual machine named modernize-app-vm is selected.](media/azure-modernize-app-vm.png 'The modernize-app-vm virtual machine')

    The public IP address will be visible on the Overview page for the virtual machine. Connect to this IP address using your private key and the built-in `ssh` command in Windows 10 version 1709 or later, Linux, and MacOS; or PuTTY on any version of Windows.

    ![Connection to the virtual machine over SSH was successful.](media/vm-ssh.png 'Connected to the Ubuntu-based virtual machine with SSH')

2. Run the following commands to register the Microsoft key and software repository feed, copy the generated list into `sources.list.d`, and install the Microsoft GPG public key.

    ```
    curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
    sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
    ```

3. Update the APT package list and install the Moby engine and command-line interface.

    ```
    sudo apt-get update
    sudo apt-get install moby-engine
    sudo apt-get install moby-cli
    ```

    ![The Moby engine and command-line interface are installed.](media/vm-install-moby.png 'Moby engine and command-line interface')

4. Install the latest version of Azure IoT Edge.

    ```
    sudo apt-get update
    sudo apt-get install iotedge
    ```

    ![Azure IoT Edge is installed.](media/vm-install-iot-edge.png 'Azure IoT Edge security daemon')

5. As the console message at the end of step 4 indicates, we will need to modify a configuration file. The configuration file is read-only by default, so you will need to use `chmod` to make it writable. Then, using a text editor like `vim` or `nano`, modify the configuration file.

    ```
    sudo chmod +w /etc/iotedge/config.yaml
    sudo vim /etc/iotedge/config.yaml
    ```

    Inside the configuration file, scroll down to the lines labeled:

    ```
    # Manual provisioning configuration
    provisioning:
        source: "manual"
        device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
    ```

    Replace the **<ADD DEVICE CONNECTION STRING HERE>** text with the connection string you copied in the prior task.  In vim, use the arrow keys to move the cursor to the point before the angle bracket `<` and then type `35x` to delete the next 35 characters, leaving you with an empty pair of quotation marks. Then, type `i` to insert, paste in your text by right-clicking the screen (if supported) or `Shift+Insert`. Once you have your connection string copied, press `Esc` to exit insert mode and type `ZZ` to save the file.

    ![Azure IoT Edge is configured.](media/vm-configure-iot-edge.png 'Azure IoT Edge device configuration')

6. Restart the IoT Edge service.

    ```
    sudo systemctl restart iotedge
    ```

    To confirm that this worked successfully, return to the modernize-app-ubuntu1 device in IoT Hub and you should see a runtime response of **417 -- The device's deployment configuration is not set**.

    ![Azure IoT Edge configuration succeeded.](media/vm-configure-iot-edge-success.png 'Azure IoT Edge device configuration succeeded')

    Although the runtime response reports an error message, this is a good result--the 417 error message indicates that we have not installed any IoT Edge modules on the device, and that is what we will do in the next task.

### Task 3: Build and deploy an IoT Edge module

1.  Open Visual Studio Code. Navigate to the **Command Palette** by selecting it from the View menu, or by pressing `Ctrl+Shift+P`. Select the option **Azure IoT Edge: New IoT Edge Solution**. If you do not see it, begin typing "Azure IoT Edge" and it should appear.

    > **NOTE**: If you do not see anything in the Command Palette for Azure IoT Edge, be sure to install the [Azure IoT Tools extension](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) for Visual Studio Code.

    ![Create a new IoT Edge Solution.](media/code-iot-edge-solution.png 'Azure IoT Edge: New IoT Edge Solution')

2.  Select a folder for your IoT Edge solution, such as `C:\Temp\IoT Edge`. When you have created or found a suitable folder, click the **Select Folder** button.

    ![The IoT Edge folder location is selected.](media/code-iot-edge-select-folder.png 'Select Folder')

3. Enter `WWIFactorySensors` as the name for your solution.

    ![Name a new IoT Edge Solution.](media/code-iot-edge-name.png 'Azure IoT Edge: Provide a Solution Name')

4. In the Select Module Template menu, choose **C# Module**.

    ![The C# module template is selected.](media/code-iot-edge-csharp.png 'Select Module Template')

5. In the Provide a Module Name menu, enter `WWIFactorySensorModule` for the name.

    ![Name a new IoT Edge module.](media/code-iot-edge-module-name.png 'Azure IoT Edge: Provide a Module Name')

6. Provide the location of your Azure Container Registry instance.

    ![The Azure Container Registry location is filled in, along with the module name.](media/code-iot-edge-registry.png 'Azure IoT Edge: Provide a Docker Image Repository')

    If you do not know the public address of your Azure Container Registry instance, navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com). From there, select the resource of type **Container registry**.

    ![Name a new IoT Edge module.](media/code-iot-edge-cr-location.png 'Azure IoT Edge: Provide a Module Name')

    Copy the **Login server** and replace `localhost:5000` with this address.

    ![Name a new IoT Edge module.](media/code-iot-edge-cr-name.png 'Azure IoT Edge: Provide a Module Name')

7. Install any required assets to build the factory sensor solution by selecting **Yes** to the following prompt.

    ![The Yes option is selected when prompted about adding required assets.](media/code-iot-edge-assets.png 'Required assets to build and debug are missing')

8. Open the `.env` file and ensure that the container registry username and password are correct.

    ![Ensure that the container username and password are correct for the selected registry.](media/code-iot-edge-env.png '.env')

    If you are not sure what the values should be, navigate to the container registry and then select **Access keys** from the Settings menu.

    ![The username and password are selected in the Access keys page for the container registry.](media/code-iot-edge-password.png 'Container registry access keys')

9. Replace the contents of **deployment.template.json** and **deployment.debug.template.json** with the following JSON. Replace the **modernizeapp** on lines 14, 15, and 16 with the name of your container registry.

    ```
    {
    "$schema-template": "2.0.0",
    "modulesContent": {
        "$edgeAgent": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "runtime": {
            "type": "docker",
            "settings": {
                "minDockerVersion": "v1.25",
                "loggingOptions": "",
                "registryCredentials": {
                "modernizeapp": {
                    "username": "$CONTAINER_REGISTRY_USERNAME_modernizeapp",
                    "password": "$CONTAINER_REGISTRY_PASSWORD_modernizeapp",
                    "address": "modernizeapp.azurecr.io"
                }
                }
            }
            },
            "systemModules": {
            "edgeAgent": {
                "type": "docker",
                "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": {}
                }
            },
            "edgeHub": {
                "type": "docker",
                "status": "running",
                "restartPolicy": "always",
                "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": {
                    "HostConfig": {
                    "PortBindings": {
                        "5671/tcp": [
                        {
                            "HostPort": "5671"
                        }
                        ],
                        "8883/tcp": [
                        {
                            "HostPort": "8883"
                        }
                        ],
                        "443/tcp": [
                        {
                            "HostPort": "443"
                        }
                        ]
                    }
                    }
                }
                }
            }
            },
            "modules": {
            "WWIFactorySensorModule": {
                "version": "1.0.1",
                "type": "docker",
                "status": "running",
                "restartPolicy": "always",
                "settings": {
                "image": "${MODULES.WWIFactorySensorModule}",
                "createOptions": {}
                }
            }
            }
        }
        },
        "$edgeHub": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "routes": {
            "WWIFactorySensorModuleToIoTHub": "FROM /messages/modules/WWIFactorySensorModule/outputs/* INTO $upstream"
            },
            "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
            }
        }
        }
    }
    }
    ```

10. Open the **modules\WWIFactorySensorModule** folder and open **module.json**. Ensure that the **repository** in line 5 is correct.

    ![The repository for module.json is selected.](media/code-iot-edge-module-json.png 'module.json')

11. Open the **Program.cs** file. Replace its contents with the following code.

    ```
    namespace WWIFactorySensorModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;

        using System.Collections.Generic;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;
        using Newtonsoft.Json.Linq;

        public class Program
        {
            // Each message sent to IoT Hub will have this shape
            class MessageBody
            {
                public int factoryId {get; set;}
                public Machine machine {get;set;}
                public Ambient ambient {get; set;}
                public string timeCreated {get; set;}
            }
            class Machine
            {
                public int machineId {get; set;}
                public double temperature {get; set;}
                public double pressure {get; set;}
                public double electricityUtilization {get; set;}
            }
            class Ambient
            {
                public double temperature {get; set;}
                public int humidity {get; set;}
            }

            public int Interval { get; private set; }
            public string OutputChannelName { get; private set; }

            static async Task Main(string[] args)
            {
                var module = new Program();
                await module.Run();
            }

            public async Task Run()
            {
                var cancellationTokenSource = new CancellationTokenSource();
                // Unloading assembly or Ctrl+C will trigger cancellation token
                AssemblyLoadContext.Default.Unloading += (ctx) => cancellationTokenSource.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cancellationTokenSource.Cancel();

                try
                {
                    // Use an environment variable or the defaults
                    Interval = 5000; // Generate a new message every 5 seconds.
                    int interval;
                    if (Int32.TryParse(Environment.GetEnvironmentVariable("Interval"), out interval))
                        Interval = interval;
                    OutputChannelName = Environment.GetEnvironmentVariable("OutputChannelName") ?? "output1";

                    MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                    ITransportSettings[] settings = { mqttSetting };

                    // Open a connection to the Edge runtime
                    using (var moduleClient = await ModuleClient.CreateFromEnvironmentAsync(settings))
                    {
                        await moduleClient.OpenAsync(cancellationTokenSource.Token);
                        Console.WriteLine("IoT Hub module client initialized.");

                        await moduleClient.SetDesiredPropertyUpdateCallbackAsync(DesiredPropertyUpdateHandler, moduleClient, cancellationTokenSource.Token);

                        // Fire the DesiredPropertyUpdateHandler manually to read initial values
                        var twin = await moduleClient.GetTwinAsync();
                        await DesiredPropertyUpdateHandler(twin.Properties.Desired, moduleClient);

                        Random rand = new Random();
                        while (!cancellationTokenSource.IsCancellationRequested)
                        {
                            // Generate new data to send as a message
                            var msg = GenerateNewMessage(rand);
                            var messageString = JsonConvert.SerializeObject(msg);
                            var messageBytes = Encoding.UTF8.GetBytes(messageString);

                            await moduleClient.SendEventAsync(OutputChannelName, new Message(messageBytes));
                            await Task.Delay(Interval, cancellationTokenSource.Token);
                        }
                    }
                }
                catch (TaskCanceledException)
                {
                    Console.WriteLine("Asynchronous operation cancelled.");
                }

                Console.WriteLine("IoT Hub module client exiting.");
            }

            // Simulate factory activity to create a new message
            private MessageBody GenerateNewMessage(Random rand)
            {
                // This sensor will be attached to machine 12345
                var machine = new Machine { machineId = 12345 };
                // Create values for temperature, stamp pressure, and electricity utilization
                machine.temperature = GenerateDoubleValue(rand, 55.0, 2.4);
                machine.pressure = GenerateDoubleValue(rand, 7539, 14);
                machine.electricityUtilization = GenerateDoubleValue(rand, 29.36, 1.1);

                // This represents conditions around the stamp press: the current temperature and humidity levels of the room itself
                var ambient = new Ambient();
                ambient.temperature = GenerateDoubleValue(rand, 22.6, 1.1);
                ambient.humidity = Convert.ToInt32(GenerateDoubleValue(rand, 20.0, 3.5));

                // The machine is in factory 1
                return new MessageBody
                {
                    factoryId = 1,
                    machine = machine,
                    ambient = ambient,
                    timeCreated = DateTime.UtcNow.ToString("yyyy-MM-ddTHH:mm:ssZ")
                };
            }

            // Returned values generally follow a normal distribution and we pass in the mean and standard deviation,
            // but occasionally we will get anomalies which report values well outside the expectations for this distribution
            private double GenerateDoubleValue(Random rand, double mean, double stdDev, double likelihoodOfAnomaly = 0.06)
            {
                double u1 = rand.NextDouble();

                double res = BoxMullerTransformation(rand, mean, stdDev);

                if (u1 <= likelihoodOfAnomaly / 2.0)
                {
                    // Generate a negative anomaly
                    res = res * 0.6;
                }
                else if (u1 > likelihoodOfAnomaly / 2.0 && u1 <= likelihoodOfAnomaly)
                {
                    // Generate a positive anomaly
                    res = res * 1.8;
                }

                return res;
            }

            // Reference: https://stackoverflow.com/questions/218060/random-gaussian-variables
            private double BoxMullerTransformation(Random rand, double mean, double stdDev)
            {
                double u1 = 1.0 - rand.NextDouble();
                double u2 = 1.0 - rand.NextDouble();
                double randStdNormal = Math.Sqrt(-2.0 * Math.Log(u1)) * Math.Sin(2.0 * Math.PI * u2);
                return mean + stdDev * randStdNormal;
            }

            private async Task DesiredPropertyUpdateHandler(TwinCollection desiredProperties, object userContext)
            {
                var moduleClient = userContext as ModuleClient;

                if (desiredProperties.Contains("OutputChannel"))
                {
                    OutputChannelName = desiredProperties["OutputChannel"];
                }
                if (desiredProperties.Contains("Interval"))
                {
                    Interval = desiredProperties["Interval"];
                }
                var reportedProperties = new TwinCollection(new JObject(), null);
                reportedProperties["OutputChannel"] = OutputChannelName;
                reportedProperties["Interval"] = Interval;
                await moduleClient.UpdateReportedPropertiesAsync(reportedProperties);

            }

            public async Task<byte[]> ReadAllBytesAsync(string filePath, CancellationToken cancellationToken)
            {
                const int bufferSize = 4096;
                using (FileStream sourceStream = new FileStream(filePath,
                    FileMode.Open, FileAccess.Read, FileShare.Read,
                    bufferSize: bufferSize, useAsync: true))
                {
                    byte[] data = new byte[sourceStream.Length];
                    int numRead = 0;
                    int totalRead = 0;
                
                    while ((numRead = await sourceStream.ReadAsync(data, totalRead, Math.Min(bufferSize,(int)sourceStream.Length-totalRead), cancellationToken)) != 0)
                    {
                    totalRead += numRead;
                    }
                    return data;
                }
            }
        }
    }
    ```

12. Navigate to the **Command Palette** by selecting it from the View menu, or by pressing `Ctrl+Shift+P`. Select the option **Azure IoT Edge: Set Default Target Platform for Edge Solution**.

    ![Set a target platform for the IoT Edge Solution.](media/code-iot-edge-target-platform.png 'Azure IoT Edge: Set Default Target Platform for Edge Solution')

13. Select **amd64** from the target platform menu. Be sure not to select windows-amd64, as the sensor will deploy to a Linux machine.

    ![Set a target platform of amd64 for the IoT Edge Solution.](media/code-iot-edge-amd64.png 'amd64')

14. Navigate to the Terminal and enter the following command:

    ```
    docker login -u <USERNAME> -p "<PASSWORD>" <CONTAINER_REGISTRY>
    ```

    Enter appropriate values for username, password, and container registry. The values for username and password are stored in the .env file.

    ![Log in to the container registry.](media/code-iot-edge-docker-login.png 'docker login')

15. Right-click on **deployment.template.json** and select **Build and Push IoT Edge Solution**. This may take several minutes depending upon the speed of your internet connection. The terminal will include the current status of each Docker image layer. When everything is **Pushed**, continue to the next step.

    ![Build and push the IoT Edge solution.](media/code-iot-edge-build-push.png 'Build and Push IoT Edge Solution')

16. Navigate to the Azure IoT Hub section and select your IoT Hub by selecting the **...** menu option and choosing **Select IoT Hub**. Choose your subscription and IoT Hub and it will appear in the Azure IoT Hub section.

    ![The Select IoT Hub option is selected.](media/code-iot-edge-select-hub.png 'Select IoT Hub')

17. Right-click on the `modernize-app-ubuntu1` device and select **Create Deployment for Single Device**.

    ![The Create Deployment for Single Device option is selected.](media/code-iot-edge-create-deployment.png 'Create Deployment for Single Device')

18. In the **Open** menu, navigate to  the **config** folder and select **deployment.amd64.json** and then click **Select Edge Deployment Manifest**.

    ![The amd64 configuration is selected.](media/code-iot-edge-amd64-deployment.png 'Select Edge Deployment Manifest')

    > **NOTE**: It is important that you not select the `deployment.template.json` or `deployment.debug.template.json` files. These are template JSON files which do not contain any sensitive details. These are the files that you can safely check into source control. Visual Studio Code uses these templates as the basis for the proper deployment files in the **config** folder. You should not check in files in the config folder.

19. Navigate back to the IoT Hub (referring back to Task 1 if needed), and then return to the IoT Edge entry in Automatic Device Management. Select the entry labeled **modernize-app-ubuntu1** and you should see an IoT Edge Runtime Response of **200 -- OK** and four deployed modules, including the **WWIFactorySensorModule**.

    ![The modernize-app-ubuntu1 device is returning a 200 response code and reports that the WWIFactorySensorModule is installed.](media/azure-modernize-app-ubuntu1-status.png 'Status for the modernize-app-ubuntu1 device')


## Exercise 2: Modernize services logic to use event sourcing and CQRS

Duration: X minutes

\[insert your custom Hands-on lab content here . . . \]

### Task 1: Task name

1.  Number and insert your custom workshop content here . . . 

    -  Insert content here

        -  

### Task 2: Task name

1.  Number and insert your custom workshop content here . . . 

    -  Insert content here

        -  


## Exercise 3: Set up ingest from IoT Hub to Cosmos DB

Duration: 30 minutes

Now that IoT Hub is storing data, we can begin to process the sensor data messages and insert the results into long-term storage. In this exercise, we will store messages in Cosmos DB as well as Azure Data Lake Storage Gen2.

### Task 1: Enable Azure Synapse Link for Cosmos DB

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

    If you do not see the resource group in the Recent resources section, type in "resource groups" in the top search menu and then select **Resource groups** from the results.

    ![In the Services search result list, Resource groups is selected.](media/azure-resource-group-search.png 'Resource groups')

    From there, select the **modernize-app** resource group.

2. Select the Cosmos DB account you created before the hands-on lab. This will have a Type of **Azure Cosmos DB account**.

    ![In the Services search result list, Resource groups is selected.](media/azure-cosmos-db-select.png 'Resource groups')

3. In the **Settings** section, navigate to the **Features** pane.

    ![In the Cosmos DB settings section, Features is selected.](media/azure-cosmos-db-features.png 'Features')

4. Select the **Azure Synapse Link** feature and then select **Enable**.  Note that this may take several minutes to complete. Please wait for this to complete before moving on to the next task.

    ![In the Features section, Azure Synapse Link is selected and enabled.](media/azure-cosmos-db-synapse-link.png 'Azure Synapse Link')

        
### Task 2: Create Cosmos DB containers

1.  In the **Containers** section for your Cosmos DB account,select **Browse**.

    ![In the Cosmos DB containers section, Browse is selected.](media/azure-cosmos-db-browse.png 'Browse')

2. Select **+ Add Collection** on the Browse pane to add a new collection.

    ![In the Browse pane, Add Collection is selected.](media/azure-cosmos-db-add-collection.png 'Add Collection')

3. In the **Add Container** tab, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Database id                    | _select `Create new` and enter `sensors`_          |
   | Throughput                     | _`400`_                                            |
   | Container id                   | _`temperatureanomalies`_                           |
   | Partition key                  | _`/machineid`_                                     |
   | Analytical store               | _select `On`_                                      |

   ![The form fields are completed with the previously described settings.](media/azure-create-cosmos-db-temp-container.png 'Create a container for temperature anomalies')

4. Select **OK** to create the container. This will take you to the Data Explorer pane for Cosmos DB.

5. In the Data Explorer pane, select **New Container** to add a new container.  In the **Add Container** tab, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Database id                    | _select `Use existing` and select `sensors`_       |
   | Container id                   | _`pressure`_                                       |
   | Partition key                  | _`/machineid`_                                     |
   | Analytical store               | _select `On`_                                      |

   ![In the Data Explorer pane, New Container is selected and details filled out in the Add Container fly-out pane.](media/azure-cosmos-db-add-collection-2.png 'Add New Container')

5. Select **OK** to create the container. This will take you back to the Data Explorer pane for Cosmos DB.

6. In the **Settings** menu, select **Keys** and navigate to the Keys page. Copy the primary key and store it in a text editor for a later task.

    ![In the Keys pane, the Primary Key is selected for copying.](media/azure-cosmos-db-key.png 'Copy primary key')

### Task 3: Create a sensor data directory in Azure Data Lake Storage Gen2

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

2. Select the **modernizeappstorage#SUFFIX#** storage account which you created before the hands-on lab. Note that there may be multiple storage accounts, so be sure to choose the one you created.

    ![The storage account named modernizeappstorage is selected.](media/azure-storage-account-select.png 'The modernizeappstorage storage account')

3. In the **Data Lake Storage** section, select **Containers**. Then, select the **synapse** container you created before the hands-on lab.

    ![The Container named synapse is selected.](media/azure-storage-account-synapse.png 'The synapse storage container')

4. In the synapse container, select **+ Add Directory**. Enter **sensordata** for the name and select **Save**.

    ![A new data lake storage directory named sensordata is created.](media/azure-storage-account-sensordata.png 'Creating a new directory named sensordata')

5. Close the synapse Container pane and return to the storage account. In the **Settings** section, select **Access keys** and copy the storage account name and key1's Key, storing them in a text editor for later use.

    ![The storage account name and key are selected and copied for future use.](media/azure-storage-account-key.png 'Copying the storage account name and key')

### Task 4: Create an Azure Stream Analytics job

1. In the [Azure portal](https://portal.azure.com), type in "stream analytics jobs" in the top search menu and then select **Stream Analytics jobs** from the results.

    ![In the Services search result list, Stream Analytics jobs is selected.](media/azure-stream-analytics-search.png 'Stream Analytics jobs')

2. In the Stream Analytics jobs page, select **+ Add** to add a new container.  In the **New Stream Analytics job** tab, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Job name                       | _`modernize-app-stream`_                           |
   | Subscription                   | _select the appropriate subscription_              |
   | Resource group                 | _select `modernize-app`_                           |
   | Location                       | _select the resource group's location_             |
   | Hosting environment            | _select `Cloud`_                                   |
   | Streaming units                | _select `3`_                                       |

   ![In the New Stream Analytics job tab, form details are filled in.](media/azure-stream-analytics-create.png 'New Stream Analytics job')

3. After your deployment is complete, select **Go to resource** to open up the new Stream Analytics job.

4. In the **Configure** menu, select **Storage account settings**. Then, on the Storage account settings page, select **Add storage account**.
    
    ![In the Configure menu, Storage account settings is selected, followed by the Add storage account option.](media/azure-stream-analytics-add-storage.png 'Add storage account')

5. Choose the storage account you created before the hands-on lab and then select **Save**.

    ![In the Storage account settings menu, the appropriate storage account is selected.](media/azure-stream-analytics-add-storage-2.png 'Select storage account')

6. In the **Job topology** menu, select **Inputs**. Then, select **+ Add stream input** and choose **IoT Hub**.

    ![In Stream Analytics job inputs, IoT Hub is selected.](media/azure-stream-analytics-input-iothub.png 'IoT Hub')

7. In the **New input** tab, name your input `modernize-app-iothub` and choose the appropriate IoT Hub that you created before the lab. Leave the other settings at their default values and select **Save**.

   ![In the New input tab, form details are filled in.](media/azure-stream-analytics-input-iothub-2.png 'IoT Hub')

8. In the **Job topology** menu, select **Outputs**. Then, select **+ Add** and choose **Cosmos DB**.

    ![In Stream Analytics job outputs, Cosmos DB is selected.](media/azure-stream-analytics-output-cosmos1.png 'Cosmos DB')

9. In the **New output** window, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Output alias                   | _`cosmos-temperatureanomalies`_                    |
   | Subscription                   | _select the appropriate subscription_              |
   | Account id                     | _select `modernize-app-#SUFFIX#`_                  |
   | Database                       | _select `sensors`                                  |
   | Container name                 | _`temperatureanomalies`_                           |

   ![In the Cosmos DB new output, form field entries are filled in.](media/azure-stream-analytics-output-cosmos1-2.png 'Cosmos DB output')

10. Select **Save** to add the new output. Then, add another Cosmos DB output with the following information:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Output alias                   | _`cosmos-pressure`_                                |
   | Subscription                   | _select the appropriate subscription_              |
   | Account id                     | _select `modernize-app-#SUFFIX#`_                  |
   | Database                       | _select `sensors`                                  |
   | Container name                 | _`pressure`_                                       |

11. Add a **Blob storage/Data Lake Storage Gen2** output with the following details:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Output alias                   | _`synapse-sensordata`_                             |
   | Subscription                   | _select the appropriate subscription_              |
   | Account id                     | _select `modernizeappstorage#SUFFIX#`_             |
   | Container                      | _select `synapse`                                  |
   | Path pattern                   | _`sensordata/{machineid}`_                         |
   | Minimum rows                   | _`100`_                                            |
   | Maximum time - Hours           | _`0`_                                              |
   | Maximum time - Minutes         | _`5`_                                              |
   | Authentication mode            | _select `Connection string`                        |

   > **NOTE**: The path pattern should  read `sensordata/{machineid}` with the braces included. Those indicate that `machineid` is an input parameter and so we will generate one folder per individual machine.

   ![In the Blob storage / Data Lake Storage new output, form field entries are filled in.](media/azure-stream-analytics-output-dls.png 'Data Lake Storage output')

12. Select **Save** to add the new output.

13. In the **Job topology** menu, select **Query**. You should see the input and three outputs that you created, as well as sample events from IoT Hub. If you do not see sample events, select the **modernize-app-iothub** input. If you still do not see records, ensure that the virtual machine is running and IoT Hub reports no errors.

    ![In the Stream Analytics query, inputs and ouptuts, as well as sample data, are visible.](media/azure-stream-analytics-query.png 'Stream Analytics query')

14. In the query window, replace the existing query text with the following code.

    ```
    -- Anomolous results -- write to Cosmos anomalies endpoint
    WITH AnomalyDetectionStep AS
    (
        SELECT
            FactoryId,
            machine.machineId AS machineid,
            EventProcessedUtcTime,
            CAST(machine.temperature AS float) AS Temperature,
            AnomalyDetection_SpikeAndDip(CAST(machine.temperature AS float), 80, 60, 'spikesanddips')
                OVER(LIMIT DURATION(minute, 5)) AS SpikeAndDipScore
        FROM [modernize-app-iothub] TIMESTAMP BY EventProcessedUtcTime
    )
    SELECT
        FactoryId,
        machineid,
        EventProcessedUtcTime,
        Temperature,
        CAST(GetRecordPropertyValue(SpikeAndDipScore, 'Score') AS float) AS SpikeAndDipScore,
        CAST(GetRecordPropertyValue(SpikeAndDipScore, 'IsAnomaly') AS bigint) AS IsSpikeAndDipAnomaly
    INTO [cosmos-temperatureanomalies]
    FROM AnomalyDetectionStep
    WHERE
        CAST(GetRecordPropertyValue(SpikeAndDipScore, 'IsAnomaly') AS float) = 1
        AND CAST(GetRecordPropertyValue(SpikeAndDipScore, 'Score') AS float) > 0.001;

    -- Machine pressure -- write to Cosmos system wear endpoint.  Group by TumblingWindow(second, 30)
    SELECT
        FactoryId,
        machine.machineId AS machineid,
        System.TimeStamp() AS ProcessingTime,
        AVG(machine.pressure) AS Pressure,
        AVG(machine.temperature) AS MachineTemperature
    INTO [cosmos-pressure]
    FROM [modernize-app-iothub] TIMESTAMP BY EventProcessedUtcTime
    GROUP BY
        FactoryId,
        machine.machineId,
        TumblingWindow(second, 30);

    -- All sensor data -- write to ADLS gen2.
    SELECT
        FactoryId,
        CAST(machine.machineId AS NVARCHAR(MAX)) AS machineid,
        machine.temperature AS MachineTemperature,
        machine.pressure AS MachinePressure,
        ambient.temperature AS AmbientTemperature,
        ambient.humidity AS AmbientHumidity,
        EventProcessedUtcTime
    INTO [synapse-sensordata]
    FROM [modernize-app-iothub] TIMESTAMP BY EventProcessedUtcTime;
    ```

15. Select **Test query** to ensure that the queries run. You will see only the results of the last query in the Test results window and only the inputs and outputs you created in the Inputs and Outputs sections, respectively. If you want to see the results of a different query, highlight that query and select **Test selected query**.

    ![Testing the queries in stream analytics.](media/azure-stream-analytics-test-query.png 'Test query')

16. Once you are satisfied with query results, select **Save query** to save your changes.

17. Return to the **Overview** page and select **Start** to begin processing. In the subsequent menu, select **Start** once more. It may take approximately 1-2 minutes for the Stream Analytics job to start.
    
    ![The Start option in the Overview page is selected.](media/azure-stream-analytics-start.png 'Start')


## Exercise 4: Import anomaly data into an Azure Synapse Analytics SQL Pool

Duration: 10 minutes

In the prior exercise, you used the Anomaly Detector functionality built into Stream Analytics to find anomalous data points and store them in Cosmos DB. Now you will use Synapse Link in Cosmos DB to load an Azure Synapse Analytics SQL Pool in order to analyze the resulting data.

### Task 1: Import anomaly data via a Spark notebook

1. In the [Azure portal](https://portal.azure.com), type in "azure synapse analytics" in the top search menu and then select **Azure Synapse Analytics (workspaces preview)** from the results.

    ![In the Services search result list, Azure Synapse Analytics (workspaces preview) is selected.](media/azure-create-synapse-search.png 'Azure Synapse Analytics (workspaces preview)')

2. Select the workspace you created before the hands-on lab.

    ![The Azure Synapse Analytics workspace for the lab is selected.](media/azure-synapse-select.png 'modernizeapp workspace')

3. Select **Launch Synapse Studio** from the Synapse workspace page.

    ![Launch Synapse Studio is selected.](media/azure-synapse-launch-studio.png 'Launch Synapse Studio')

4. In the Azure Synapse workspace, select the **>>** chevron to expand icons to include names and then select **Manage**.

    ![The Manage option is selected.](media/azure-synapse-workspace-manage.png 'Manage')

5. In the Analytics pools section, select **Apache Spark pools** and then select the **+ New** option.

    ![Create a new Apache Spark pool.](media/azure-synapse-manage-spark-pool-1.png 'New Apache Spark pool')

6. In the **Create Apache Spark pool** window, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Apache Spark pool name         | _`modernizeapp`_                                   |
   | Autoscale                      | _select `disabled`_                                |
   | Node size                      | _select `Small (4 vCPU / 32 GB)`_                  |
   | Number of nodes                | _select `3 - 3`                                    |
   | Container name                 | _`temperatureanomalies`_                           |

   ![In the Create Apache Spark pool output, form field entries are filled in.](media/azure-synapse-create-spark-pool.png 'Create Apache Spark pool output')

7. Select **Review + create**. On the review screen, select **Create**.  Provisioning may take several minutes, but you do not need to wait for the pool to be provisioned before moving to the next step.

8. In the **Manage** page, select **Linked services** from the External connections section. Then select **+ New**.

    ![The New option is selected for linked services.](media/azure-synapse-manage-linked-services.png 'New linked service')

9. In the search box, type in "cosmos" and select **Azure Cosmos DB (SQL API)**. Then select **Continue**.

    ![The Azure Cosmos DB SQL API linked service is selected.](media/azure-synapse-cosmos-linked-service.png 'Azure Cosmos DB (SQL API) linked service')

10. In the **New linked service** window, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Name                           | _`modernize_app_cosmos`_                           |
   | Account selection method       | _select `From Azure subscription`_                 |
   | Cosmos  DB account name        | _select the appropriate account_                   |
   | Database name                  | _select `sensors`                                  |

   ![In the New linked service output, form field entries are filled in.](media/azure-synapse-cosmos-linked-service-2.png 'Azure Cosmos DB (SQL API) linked service form details')

11. Select the **Test connection** option to ensure that your connection will work, and then select **Create** to create the linked service.

12. In the Azure Synapse Workspace, select **Develop**

    ![The Develop option is selected.](media/azure-synapse-workspace-develop.png 'Develop')

13. Select the **+** and then choose **Notebook** from the ensuing dropdown list.

    ![Add a new Notebook.](media/azure-synapse-develop-notebook.png 'Add a new Notebook')

14. In the **Properties** tab, change the **Name** to `Write Cosmos Changes to SQL Pool`.

    ![Rename the Notebook to Write Cosmos Changes to SQL Pool.](media/azure-synapse-develop-cosmos-name.png 'Write Cosmos Changes to SQL Pool')

15. In the notebook toolbar, change the language to **Spark (Scala)**. [The Azure Synapse Apache Spark connector to Synapse SQL connector currently only supports Scala](https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export), so the default language of PySpark will not work.

    ![Change the notebook language to Scala.](media/azure-synapse-develop-cosmos-language.png 'Spark (Scala)')

16. In the main section, select **{ } Add Code** and paste in the following code to retrieve data from the `temperatureanomalies` container and select only the necessary columns.

    ```
    val df = spark.read.format("cosmos.olap").
        option("spark.synapse.linkedService", "modernize_app_cosmos").
        option("spark.cosmos.container", "temperatureanomalies").
        load().
        select($"FactoryId", $"machineid", $"EventProcessedUtcTime",
            $"Temperature", $"SpikeAndDipScore", $"IsSpikeAndDipAnomaly")

    df.show(10)
    ```

    Then, run the code block by selecting **Run** (shaped like a play button) or by pressing `Shift+Enter` when inside the notebook cell. After the job completes, you should see a table with the first ten anomalous results, assuming there are at least ten anomalous results by the time you reach this step.

    ![Code to connect to Cosmos DB is entered and ready to run.](media/azure-synapse-develop-cosmos-connect.png 'Connect to Cosmos DB from a notebook')

    >**NOTE**: it may take several minutes for a Spark session to start. The notebook cell output text will read "Starting Spark session" during this time.

17. Below the first code block, select **{ } Add Code** and add a new code block. Enter and then run the following code to create a new SQL Pool table with machine anomaly data.

    ```
    // Ensure that this table does not already exist on your SQL Pool; otherwise, you will get an error!
    df.write.sqlanalytics("modernapp.dbo.MachineAnomaly", Constants.INTERNAL)
    ```

18. After the code segment completes, select **{ } Add Code** and add a new block with the following code to ensure that the SQL pool has loaded correctly.

    ```
    val sqldf = spark.read.sqlanalytics("modernapp.dbo.MachineAnomaly") 
    sqldf.show(10)
    ```

    ![The SQL Pool results are returned.](media/azure-synapse-develop-cosmos-success.png 'Retrieve data from a SQL Pool')

## Exercise 5: Configure Azure function to write events and aggregates to PostgreSQL

Duration: 30 minutes

This exercise will send Stream Analytics data into PostgreSQL, specifically Azure Database for PostgreSQL Hyperscale. Because there is no direct connection from Stream Analytics to PostgreSQL, you will need to create an intermediary, which is a key use case for Azure Functions.

### Task 1: Create a sensor data table

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

    If you do not see the resource group in the Recent resources section, type in "resource groups" in the top search menu and then select **Resource groups** from the results.

    ![In the Services search result list, Resource groups is selected.](media/azure-resource-group-search.png 'Resource groups')

    From there, select the **modernize-app** resource group.

2. Select the PostgreSQL server group you created before the hands-on lab. This will have a Type of **Azure Database for PostgreSQL server group**. There are other entries of type Azure Database for PostgreSQL server v2, but we will need to gather information from the server group itself.

    ![In the modernize-app resource group, the PostgreSQL server group is selected.](media/azure-postgres-server-group.png 'Azure Database for PostgreSQL server group')

3. On the **Overview** page, copy down the **Coordinator name** field.

    ![The coordinator name for the PostgreSQL Hyperscale server group is selected.](media/azure-postgres-coordinator.png 'Coordinator name')

4. Open Azure Data Studio and connect to the coordinator server by filling in the following in the Connection Details form:

   | Field                          | Value                                       |
   | ------------------------------ | ------------------------------------------  |
   | Connection type                | _select `PostgreSQL`_                       |
   | Server name                    | _enter the coordinator name_                |
   | Authentication type            | _select `Password`_                         |
   | User name                      | _enter `citus`_                             |
   | Password                       | _enter the password_                        |
   | Database name                  | _enter `citus`_                             |

   > **NOTE**: You might need to force SSL to connect to PostgreSQL on Azure. To do this in Azure Data Studio, select the **Advanced** button and change the value for SSL mode to **Require** from its default value of Prefer.

   ![The form fields are completed with the previously described settings.](media/azure-data-studio-citus-connection-details.png 'Azure Data Studio Settings')

5. After connecting to PostgreSQL in Azure Data Studio, select **New Query** from the top menu to open a new query window.

    ![The New Query option is selected.](media/azure-data-studio-citus-new-query.png 'New Query')

6. Enter the following query and then select **Run** to create a `sensordata` table.

    ```
    CREATE TABLE sensordata(
        id serial,
        event_time timestamptz default now(),
        factory_id int,
        machine_id int,
        machine_temperature float,
        machine_pressure float,
        ambient_temperature float,
        ambient_humidity float,
        electricity_utilization float
    )
    PARTITION BY RANGE (event_time);

    -- Create 10-minute partitions
    SELECT partman.create_parent('public.sensordata', 'event_time', 'native', '10 minutes');
    UPDATE partman.part_config SET infinite_time_partitions = true;

    -- Create a table up to August 2020
    -- This demonstrates the ability to create new tables for partitions of data
    CREATE TABLE sensordata_202007
        partition OF sensordata(event_time)
        FOR VALUES FROM ('1900-01-01') TO ('2020-08-01')

    -- Create a table from August 2020 forward
    CREATE TABLE sensordata_999912
        partition OF sensordata(event_time)
        FOR VALUES FROM ('2020-08-01') TO ('9999-12-31')
    ```
   
   ![The sensordata table is created.](media/azure-data-studio-sensordata.png 'The sensordata table')

7. In a new query window, run the following to ensure that you are able to insert a row into the sensordata partitioned table.

    ```
    INSERT INTO sensordata
    (
        event_time,
        factory_id,
        machine_id,
        machine_temperature,
        machine_pressure,
        ambient_temperature,
        ambient_humidity,
        electricity_utilization
    )
    VALUES
    (
        '2020-08-01T21:53:00.000000Z',
        2,
        16000,
        55.55,
        7500.55,
        22.06,
        36.60,
        10.65
    );

    select * from public.sensordata;
    ```

    You should see one row returned.
        
### Task 2: Create an Azure Function to write sensor data to PostgreSQL

1. Open Visual Studio Code. Select the **Azure** menu option and navigate to **Functions**. In the top-right corner, choose **Create New Project...**. Note that you may need to move your mouse to the top-right corner of the Functions pane to see these options.

    ![Crew New Azure Functions Project is selected.](media/code-create-function-project.png 'Create New Project...')

2.  Select a folder for your IoT Edge solution, such as `C:\Temp\Azure Functions`. When you have created or found a suitable folder, click the **Select Folder** button.

    ![The Azure Functions folder location is selected.](media/code-function-select-folder.png 'Select Folder')

3. Choose **C#** in the **Select a language** menu.

    ![The C# language is selected.](media/code-function-select-language.png 'Select a language')

4. Choose **HttpTrigger** in the **Select a template for your project's first function** menu.

    ![The HttpTrigger template is selected.](media/code-function-select-trigger.png 'Select a template')

5. Name the function **WriteSensorDataToPostgres**.

    ![The Azure Function name is entered.](media/code-function-select-name.png 'Provide a function name')

6. Name the namespace **ModernApp**.

    ![The Azure Functions namespace is entered.](media/code-function-select-namespace.png 'Provide a namespace')

7. Choose **Function** in the **AccessRights** menu.

    ![The Function access rights level is selected.](media/code-function-select-accessrights.png 'Select access rights')

8. Choose **Open in current window** in the **Select how you would like to open your project** menu.

    ![The Azure Function will open in the current window.](media/code-function-select-open.png 'Open in current window')

9. Replace the contents of **WriteSensorDataToPostgres.cs** with the following, and then save the file.

    ```
    using System;
    using System.IO;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Extensions.Logging;
    using Newtonsoft.Json;
    using Npgsql;

    namespace ModernApp
    {
        public class SensorEvent
        {
            public DateTime event_time {get; set;}
            public int factory_id {get; set;}
            public int machine_id {get; set;}
            public float machine_temperature {get; set;}
            public float machine_pressure {get; set;}
            public float ambient_temperature {get; set;}
            public float ambient_humidity {get; set;}
            public float electricity_utilization {get; set;}
        }
        public static class WriteSensorDataToPostgres
        {
            [FunctionName("WriteSensorDataToPostgres")]
            public static async Task<IActionResult> Run(
                [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)] HttpRequest req,
                ILogger log)
            {
                log.LogInformation("C# HTTP trigger function processed a request.");

                string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
                dynamic array = JsonConvert.DeserializeObject(requestBody);

                string insertCommand = @"INSERT INTO sensordata
                (
                    event_time,
                    factory_id,
                    machine_id,
                    machine_temperature,
                    machine_pressure,
                    ambient_temperature,
                    ambient_humidity,
                    electricity_utilization
                )
                VALUES
                (
                    @event_time,
                    @factory_id,
                    @machine_id,
                    @machine_temperature,
                    @machine_pressure,
                    @ambient_temperature,
                    @ambient_humidity,
                    @electricity_utilization
                );";

                // Connect to Postgres
                var connStr = Environment.GetEnvironmentVariable("pg_connection");
                using (var conn = new NpgsqlConnection(connStr))
                {
                    conn.Open();

                    foreach(var data in array)
                    {
                        SensorEvent se = new SensorEvent();
                        se.event_time = data?.event_time;
                        se.factory_id = data?.factory_id;
                        se.machine_id = data?.machine_id;
                        se.machine_temperature = data?.machine_temperature;
                        se.machine_pressure = data?.machine_pressure;
                        se.ambient_temperature = data?.ambient_temperature;
                        se.ambient_humidity = data?.ambient_humidity;
                        se.electricity_utilization = data?.electricity_utilization;

                        using (var cmd = new NpgsqlCommand(insertCommand, conn))
                        {
                            cmd.Parameters.AddWithValue("event_time", se.event_time);
                            cmd.Parameters.AddWithValue("factory_id", se.factory_id);
                            cmd.Parameters.AddWithValue("machine_id", se.machine_id);
                            cmd.Parameters.AddWithValue("machine_temperature", se.machine_temperature);
                            cmd.Parameters.AddWithValue("machine_pressure", se.machine_pressure);
                            cmd.Parameters.AddWithValue("ambient_temperature", se.ambient_temperature);
                            cmd.Parameters.AddWithValue("ambient_humidity", se.ambient_humidity);
                            cmd.Parameters.AddWithValue("electricity_utilization", se.electricity_utilization);

                            cmd.ExecuteNonQuery();
                        }
                    }
                }            

                string responseMessage = "This HTTP triggered function executed successfully.";

                return new OkObjectResult(responseMessage);
            }
        }
    }
    ```

10. In the Visual Studio Code terminal, enter the following commands.

    ```
    dotnet add package Npgsql
    dotnet restore
    dotnet build
    ```

    After running these commands, you should receive a message that the build succeeded.

    ![The Azure Function built successfully.](media/code-function-build-succeeded.png 'Build succeeded.')

### Task 3: Deploy and configure an Azure Function

1. In Visual Studio Code, select the **Azure** menu option and navigate to **Functions**. Drill down into **Local Project** to **Functions** until you see the **WriteSensorDataToPostgres** function.

    ![The Azure Function.](media/code-function-view.png 'WriteSensorDataToPostgres')

2. Select the **WriteSensorDataToPostgres** function and then select the **Deploy to Function App...** operation. You may need to move the mouse to the top-right corner of the Functions tab to view this option.

    ![Deploy to Function App is selected.](media/code-function-deploy.png 'Deploy to Function App...')

3. Choose the appropriate subscription from the list. Then, choose the Function App starting with **modernize-app** from the Function App list.

    ![The appropriate Function App is selected.](media/code-function-deploy-app.png 'Select Function App in Azure')

4. Select **Deploy** in the ensuing modal dialog.

    ![The option to deploy is selected.](media/code-function-deploy-check.png 'Deploy')

5. After deployment completes, navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com). Then, select the **modernize-app** entry with a Type of **App Service**.

    ![The modernize-app Function App is selected.](media/azure-function-select.png 'modernize-app')

6. In the Settings menu, select **Configuration**. Then, select **Application settings** and then **+ New application setting**.

    ![The New application setting is selected.](media/azure-function-new-appsetting.png 'New Application setting')

7. Add an application setting with the name of `pg_connection` and a value of the PostgreSQL connection string. Take the following connection string and replace the host and password values with your values.  Then select **OK** to create the application setting and **Save** and then **Continue** on the App Service menu to save changes.

    ```
    Server={modernize-app-c.postgres.database.azure.com}; Port=5432; Database=citus; Username=citus; Password={your_password}; SSL Mode=Require; Trust Server Certificate=true
    ```

    ![The New application setting is filled out.](media/azure-function-new-appsetting-configure.png 'New application setting details')

### Task 4: Update the Stream Analytics job

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com). Then, navigate to the **modernize-app-stream** Stream Analytics job. If the job is currently running, select **Stop** to stop the job temporarily, as you will not be able to modify a running job. Select **Yes** in the ensuing modal dialog to confirm that you wish to stop the job.

    ![Stop the current stream analytics job.](media/azure-stream-analytics-stop.png 'Stop')

    >**NOTE**: Stopping a Stream Analytics job may take one or two minutes to complete. Until the job has completely stopped, you will not be able to modify any inputs, functions, queries, or outputs and the pages will appear in a read-only state.

2. In the Job topology menu, select **Outputs**. Under **+ Add**, select **Azure function** to add a new Azure Function output.

3. In the **New output** window, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Output alias                   | _`fn-writesensordatatopostgres`_                   |
   | Subscription                   | _select the appropriate subscription_              |
   | Function app                   | _select `modernize-app-#SUFFIX#`_                  |
   | Function                       | _select `WriteSensorDataToPostgres`_               |
   | Max batch count                | _`100`_                                            |

   ![In the Azure function new output, form field entries are filled in.](media/azure-stream-analytics-output-function.png 'Azure function output')

4. Select **Save** to add the new output.

5. Select **Query** from the Job topology menu and add the following at the **end** of the existing query code. Highlight the query and then select **Test selected query** to check that you get results.

    ```
    -- Sensor data -- write to Postgres hyperscale via Azure function
    SELECT
        EventProcessedUtcTime AS event_time,
        FactoryId AS factory_id,
        machine.machineId AS machine_id,
        machine.temperature AS machine_temperature,
        machine.pressure AS machine_pressure,
        ambient.temperature AS ambient_temperature,
        ambient.humidity AS ambient_humidity,
        machine.electricityUtilization AS electricity_utilization
    INTO [fn-writesensordatatopostgres]
    FROM [modernize-app-iothub] TIMESTAMP BY EventProcessedUtcTime; 
    ```

    >**IMPORTANT**: The new query should be added at the end of the existing set of queries because the first query contains a common table expression and Stream Analytics requires that common table expressions not have any code above them.

6. Select **Save query** to save the query. Then, return to the **Overview** page and select **Start** to begin the Stream Analytics job once again.

7. After 5 to 10 minutes, return to Azure Data Studio and run the following query. You should now see data in PostgreSQL which originated from your sensor data generator.

    ```
    select * from public.sensordata;
    ```

    ![Data is flowing into PostgreSQL.](media/azure-data-studio-postgres-data.png 'Sensor data')

## Exercise 6: Use Azure Synapse Analytics to train and register a predictive maintenance model

Duration: 40 minutes

Now that your data is streaming to data sources like Cosmos DB and Azure Database for PostgreSQL, it is time to train and build a model to predict whether machine maintenance is required. 

### Task 1: Load historical maintenance data

1. Open the hands-on lab's Resources\Synapse directory and confirm that you have a file called **HistoricalMaintenanceRecord.csv**.

2.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

    If you do not see the resource group in the Recent resources section, type in "resource groups" in the top search menu and then select **Resource groups** from the results.

    ![In the Services search result list, Resource groups is selected.](media/azure-resource-group-search.png 'Resource groups')

    From there, select the **modernize-app** resource group.

3. Select the **modernizeappstorage#SUFFIX#** storage account which you created before the hands-on lab. Note that there may be multiple storage accounts, so be sure to choose the one you created.

    ![The storage account named modernizeappstorage is selected.](media/azure-storage-account-select.png 'The modernizeappstorage storage account')

4. In the **Data Lake Storage** section, select **Containers**. Then, select the **synapse** container you created before the hands-on lab.

    ![The Container named synapse is selected.](media/azure-storage-account-synapse.png 'The synapse storage container')

5. In the synapse container, select **+ Add Directory**. Enter **maintenancedata** for the name and select **Save**. Then, add another directory named **models**.

6. Navigate to the **maintenancedata** directory and select the **Upload** option. In the Files section, select the folder icon to upload files. Navigate to where you saved **HistoricalMaintenanceRecord.csv** and choose this file for upload. Then select **Upload** to finish uploading the file.

    ![The historical maintenance record data is uploaded.](media/azure-synapse-upload.png 'Historical maintenance record')

### Task 2: Create a new Azure Machine Learning Datastore

1. Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

2. Select the **modernize-app-#SUFFIX#** resource with a Type of **Machine Learning**.

    ![The Machine Learning resource is selected.](media/azure-ml-select.png 'Machine Learning')

3. Select **Launch now** to open the Azure Machine Learning studio.

    ![The option to launch Azure Machine Learning Studio is selected.](media/azure-ml-select.png 'Launch now')

4. In the Azure Machine Learning studio, select the **Datastores** option in the Manage tab. Then, select the **+ New datastore** option.

    ![The option to create a new datastore is selected.](media/azure-ml-new-datastore.png 'New datastore')

5. In the **New datastore** window, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Datastore name                 | _`modernizeappstorage`_                            |
   | Datastore type                 | _select `Azure Blob Storage`_                      |
   | Account selection method       | _select `From Azure subscription`_                 |
   | Subscription ID                | _select the appropriate subscription_              |
   | Storage account                | _select `modernizeappstorage#SUFFIX#`_             |
   | Blob container                 | _select `synapse`_                                 |
   | Allow Azure ML Service...      | _select `No`_                                      |
   | Authentication type            | _select `Account key`_                             |
   | Account key                    | _enter the account key_                            |

   ![In the Azure function new output, form field entries are filled in.](media/azure-stream-analytics-output-function.png 'Azure function output')

6. Select **Create** to add the new output.

### Task 3: Develop the predictive maintenance model

1. In the [Azure portal](https://portal.azure.com), type in "azure synapse analytics" in the top search menu and then select **Azure Synapse Analytics (workspaces preview)** from the results.

    ![In the Services search result list, Azure Synapse Analytics (workspaces preview) is selected.](media/azure-create-synapse-search.png 'Azure Synapse Analytics (workspaces preview)')

2. Select the workspace you created before the hands-on lab.

    ![The Azure Synapse Analytics workspace for the lab is selected.](media/azure-synapse-select.png 'modernizeapp workspace')

3. Select **Launch Synapse Studio** from the Synapse workspace page.

    ![Launch Synapse Studio is selected.](media/azure-synapse-launch-studio.png 'Launch Synapse Studio')

4. Navigate to the **Develop** section and create a new **Notebook**. Leave this notebook's language at its default of **PySpark (Python)**.

    ![Add a new Notebook.](media/azure-synapse-develop-notebook.png 'Add a new Notebook')

5. In the **Properties** section, name the notebook **Stamp Press Maintenance Model**. Add a code block to import libraries needed for the notebook and run the block. Note that this may take several minutes to start a Spark session.

    ```
    import numpy as np
    import pyspark
    import os
    import urllib
    import sys

    from pyspark.sql.functions import *
    from pyspark.ml.classification import *
    from pyspark.ml.evaluation import *
    from pyspark.ml.feature import *
    from pyspark.ml import Pipeline
    from pyspark.sql.types import StructType, StructField
    from pyspark.sql.types import DoubleType, IntegerType, StringType

    from azureml.core.run import Run
    from azureml.core import Workspace, Environment, Datastore
    from azureml.core.experiment import Experiment
    from azureml.core.model import Model
    ```

6. Add a new code block to load the historical maintenance record. Be sure to replace **modernizeappstorage** on line 7 with your storage account. Then run the code block.

    ```
    schema = StructType([
        StructField("Pressure", IntegerType(), True),
        StructField("MachineTemperature", IntegerType(), True),
        StructField("MaintenanceRequired", StringType(), True)
    ])

    data = spark.read.load('abfss://synapse@modernizeappstorage.dfs.core.windows.net/maintenancedata/HistoricalMaintenanceRecord.csv',
        format='csv', header=False, schema=schema)
    data.show(10)
    ```

    ![Results after loading the historical maintenance record data.](media/azure-synapse-develop-pred-notebook.png 'Historical maintenance record data')

7. Add a new code block to build an Azure Machine Learning experiment, train the predictive maintenance model on a Spark cluster, save the artifacts to Azure Data Lake Storage, and then transmit the model artifacts to Azure Machine Learning.  Be sure to change the value of **subscription_id** on line 3 with your subscription ID. Then run the code block.

    ```
    # load workspace and environment
    ws = Workspace(
        subscription_id = '{ Enter your subscription ID }',
        resource_group = 'modernize-app',
        workspace_name = 'modernize-app')

    # initialize logger and start experiment
    experiment = Experiment(ws, "Stamp_Press_Experiment")
    run = experiment.start_logging()

    # vectorize all numerical columns into a single feature column
    feature_cols = data.columns[:-1]
    assembler = pyspark.ml.feature.VectorAssembler(
        inputCols=feature_cols, outputCol='features')

    # convert text labels into indices
    label_indexer = pyspark.ml.feature.StringIndexer(
        inputCol='MaintenanceRequired', outputCol='label').fit(data)

    # use a Decision Tree Classifier to train on the training set
    classifier = DecisionTreeClassifier(
    featuresCol="features",
    labelCol="label",
    maxDepth=6,
    maxBins=50)

    pipeline = Pipeline(stages=[assembler, label_indexer, classifier])
    train, test = data.randomSplit([0.70, 0.30])
    model = pipeline.fit(train)


    # predict on the test set
    prediction = model.transform(test)
    print("Prediction")
    prediction.show(10)

    # evaluate the accuracy of the model using the test set
    evaluator = pyspark.ml.evaluation.MulticlassClassificationEvaluator(
        metricName='accuracy')
    accuracy = evaluator.evaluate(prediction)

    print()
    print('#####################################')
    print("Accuracy is {}".format(accuracy))
    print('#####################################')
    print()

    # log accuracy
    run.log('Accuracy', accuracy)

    # Save pipeline
    model.write().overwrite().save("/models/stamp_press_model")

    # Download model details using Azure ML Datastore
    ds = Datastore.get(ws, "modernizeappstorage")

    ds.download("/tmp/stamp_press_model", prefix="/models/stamp_press_model")

    # Push model to Azure ML experiment
    run.upload_folder(
        name = 'stamp_press_model',
        path = '/tmp/stamp_press_model/models/stamp_press_model'
    )

    # Register Azure ML model
    run.register_model(
        model_name = 'stamp_press_model',
        model_path = 'stamp_press_model'
    )

    run.complete()
    ```

    The output of this block will include ten predictions, the overall accuracy of the test data set, and notes from Azure Machine Learning concerning the downloaded files.

### Task 4: Deploy the predictive maintenance model

1.  Open a console on your local machine. Navigate to the folder containing downloaded hands-on lab  resources and from there into **Resources\Azure ML**. There should be a file in this directory named **score.py**.

2. Run **python** in this directory to open a Python console.

    ![Python is running in the Resources\Azure ML directory.](media/python-anaconda-prompt.png 'Python')

    >**NOTE**: the installation of Python you choose must have [the Azure Machine Leraning SDK for Python](https://docs.microsoft.com/en-us/python/api/overview/azure/ml/install?view=azure-ml-py) installed. In this example, I am using the Anaconda distribution of Python, but any Python installation with the appropriate libraries will work.

3. Run the following code in Python to deploy an instance of your Azure Machine Learning model using the `AzureML-PySpark-MmlSpark-0.15` environment. Be sure to change the value of **subscription_id** with your subscription.

    ```
    from azureml.core.webservice import AciWebservice
    from azureml.core import Workspace, Environment 
    from azureml.core.model import InferenceConfig, Model

    ws = Workspace(
        subscription_id = '<Your subscription>',
        resource_group = 'modernize-app',
        workspace_name = 'modernize-app')

    env = Environment.get(ws, "AzureML-PySpark-MmlSpark-0.15")
    inference_config = InferenceConfig(entry_script="score.py", environment=env)

    model = Model(ws, 'stamp_press_model') 
    models = [model] 
    aciconfig = AciWebservice.deploy_configuration(cpu_cores = 2, memory_gb = 4, auth_enabled = False) 
    service = Model.deploy(ws, "stamp-press-model", models, inference_config, aciconfig) 
    ```

    ![Python is running in the Resources\Azure ML directory.](media/python-deploy.png 'Python')

4. Although `Model.deploy()` returned a result, this is an asynchronous call. If you want to check on the status of deployment, navigate to the Azure Machine Learning studio, select **Endpoints**, and select the **stamp-press-model** endpoint.

    ![The stamp press model is selected.](media/azure-ml-endpoints.png 'stamp-press-model')

5. In the stamp-press-model endpoint, observe the current state. If the Deployment state is **Transitioning**, this means that Azure Machine Learning is still deploying the endpoint. If the Deployment state is **Unhealthy**, return to the Python console and run the following command.

    ```
    service.get_logs()
    ```

    If the deployment state is **Healthy**, deployment succeeded and your Azure Machine Learning deployment is ready to be consumed.  It may take take **up to 10 minutes** before the Deployment state is Healthy.

    ![The stamp press model is deployed.](media/azure-ml-deployed.png 'Deployed model')

### Task 5: Test the predictive maintenance model

1. On the **stamp-press-model** endpoint, copy the **REST endpoint** value to a text editor.

2. Open a command prompt and run the following command.

    ```
    curl -X POST -H "Content-Type: application/json" -d "{\"data\": [{\"Pressure\":7470, \"MachineTemperature\":69}, {\"Pressure\":7490, \"MachineTemperature\":55}, {\"Pressure\":7800, \"MachineTemperature\":30}]}" http://b2917240-e6bd-4fe1-9f09-ae4f356492ef.eastus.azurecontainer.io/score
    ```

    You should receive back a JSON array with the values `[2,0,4]`.

    >**NOTE**: If you do not have the curl application installed, you may alternatively wish to install [Postman](https://www.postman.com/), a free tool for making web requests.

3. Try modifying the input JSON and seeing what results you get. The following table represents the true maintenance requirements.

   | Pressure | Machine Temperature | ID | Maintenance Required  |
   | -------- | ------------------- | -- | -------------------------- |
   | < 7475   | < 70                | 1  | Tighten adjustment harness |
   | > 7600   | 50 <= x < 70        | 2  | Loosen adjustment harness  |
   | > 7600   | < 50                | 3  | Tighten restrictor plate   |
   | < 7475   | >= 70               | 4  | Loosen restrictor plate    |
   | > 7600   | >= 70               | 5  | Replace fabrication screws |
   | 7475 <= x <= 7600    | 50 <= x <= 70   | 0  | No maintenance required    |

    Like any realistic data set, the historical maintenance record is imperfect and so the data your model trained on will include some incorrect maintenance operations, leading to incorrect maintenance recommendations. Try a few values near the edges of pressure and machine temperature to see.

## Exercise 7: Apply predictive maintenance calculations and write to Azure Synapse Analytics

Duration: 20 minutes

Now that you have a predictive maintenance model deployed and available, you can take sensor data streaming into Cosmos DB and generate maintenance predictions. You will store those predictions in another Cosmos DB container and then use Synapse Link to load an Azure Synapse Analytics SQL pool table with the results.

### Task 1: Create a new container

1.  Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

    If you do not see the resource group in the Recent resources section, type in "resource groups" in the top search menu and then select **Resource groups** from the results.

    ![In the Services search result list, Resource groups is selected.](media/azure-resource-group-search.png 'Resource groups')

    From there, select the **modernize-app** resource group.

2. Select the Cosmos DB account you created before the hands-on lab. This will have a Type of **Azure Cosmos DB account**.

    ![In the Services search result list, Resource groups is selected.](media/azure-cosmos-db-select.png 'Resource groups')

3.  In the **Containers** section for your Cosmos DB account,select **Browse**.

    ![In the Cosmos DB containers section, Browse is selected.](media/azure-cosmos-db-browse.png 'Browse')

4. Select **+ Add Collection** on the Browse pane to add a new collection.

    ![In the Browse pane, Add Collection is selected.](media/azure-cosmos-db-add-collection.png 'Add Collection')

5. In the **Add Container** tab, complete the following:

   | Field                          | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | Database id                    | _select `Use existing` and then `sensors`_         |
   | Container id                   | _`maintenancepredictions`_                         |
   | Partition key                  | _`/machineid`_                                     |
   | Analytical store               | _select `On`_                                      |

   ![The form fields are completed with the previously described settings.](media/azure-create-cosmos-db-maintenance-container.png 'Create a container for maintenance predictions')

4. Select **OK** to create the container. This will take you to the Data Explorer pane for Cosmos DB.

### Task 2: Create Azure Function application settings

1. Navigate to the **modernize-app** resource group in the [Azure portal](https://portal.azure.com).

    ![The resource group named modernize-app is selected.](media/azure-modernize-app-rg.png 'The modernize-app resource group')

2. Select the **modernize-app** entry with a Type of **App Service**.

    ![The modernize-app Function App is selected.](media/azure-function-select.png 'modernize-app')

3. In the Settings menu, select **Configuration**. Then, select **Application settings** and then **+ New application setting**.

    ![The New application setting is selected.](media/azure-function-new-appsetting.png 'New Application setting')

4. Add the following application settings. For each one, select **OK** to save the setting and then **+ New application setting** to add the next.

   | Name                           | Value                                              |
   | ------------------------------ | ------------------------------------------         |
   | cosmosEndpointUrl              | _enter the Cosmos DB URL, something like `https://modernize-app-#SUFFIX#.documents.azure.com:443/`_ |
   | cosmosPrimaryKey               | _enter the primary key for your Cosmos DB account_ |
   | azureMLEndpointUrl             | _enter the URL (with /score) from exercise 6_      |
   | modernizeapp_DOCUMENTDB        | `AccountEndpoint=https://modernize-app-#SUFFIX#.documents.azure.com:443/;AccountKey={PRIMARY KEY};` |

   For the `modernizeapp_DOCUMENTDB` entry, replace `{PRIMARY KEY}` with the primary key for your Cosmos DB account.

5. Select **Save** and then **Continue** on the App Service menu to save the new application settings.

    ![The New application settings are filled out.](media/azure-function-new-appsettings.png 'New application settings')
        
### Task 2: Create an Azure Function based on a Cosmos DB trigger

1.  Open Visual Studio Code and navigate to the folder you created in Exercise 5 for Azure Functions. In the Azure menu, navigate to the top-right corner and select **Create Function...**

    ![Create Function is selected.](media/code-create-function.png 'Create Function')

2. Fill in the following for each step of the function creation wizard.

   | Step                             | Value                                              |
   | -------------------------------- | ------------------------------------------         |
   | Template                         | _select `CosmosDBTrigger`_                         |
   | Function Name                    | _`WriteMaintenancePredictionToCosmos`_             |
   | Namespace                        | _`ModernApp`_                                      |
   | Setting from local.settings.json | _select `Create new local app setting`_            |
   | Subscription                     | _select the appropriate subscription_              |
   | Database account                 | _select `modernize-app-#SUFFIX#`_                  |
   | Database name                    | _`sensors`_                                        |
   | Collection name                  | _`pressure`_                                       |

   After entering the collection name, you may see a modal dialog which indicates that in order to debug, you must select a storage account. Choose **Select storage account** and then select the **modernizeappstorage#SUFFIX#** account.

   ![Select storage account is selected.](media/code-create-function-storage.png 'In order to debug, you must select a storage account for internal use by the Azure Functions runtime.')

3. In the **WriteMaintenancePredictionToCosmos.cs** file, replace the existing code with the following.

    ```
    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    using Microsoft.Extensions.Logging;
    using Microsoft.Azure.Cosmos;
    using Newtonsoft.Json;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Linq;

    namespace ModernApp
    {
        
        internal class PredictionInput
        {
            [JsonProperty("Pressure")]
            internal double Pressure;
            [JsonProperty("MachineTemperature")]
            internal double MachineTemperature;
        }
        // The data structure expected by Azure ML
        internal class InputData
        {
            [JsonProperty("data")]
            internal List<PredictionInput> data;
        }

        public static class WriteMaintenancePredictionToCosmos
        {
            private static readonly string cosmosEndpointUrl = System.Environment.GetEnvironmentVariable("cosmosEndpointUrl");
            private static readonly string cosmosPrimaryKey = System.Environment.GetEnvironmentVariable("cosmosPrimaryKey");
            private static readonly string azureMLEndpointUrl = System.Environment.GetEnvironmentVariable("azureMLEndpointUrl");
            private static readonly string databaseId = "sensors";
            private static readonly string outputContainerId = "maintenancepredictions";
            private static CosmosClient cosmosClient = new CosmosClient(cosmosEndpointUrl, cosmosPrimaryKey);


            [FunctionName("WriteMaintenancePredictionToCosmos")]
            public static async Task Run([CosmosDBTrigger(
                databaseName: "sensors",
                collectionName: "pressure",
                ConnectionStringSetting = "modernizeapp_DOCUMENTDB",
                LeaseCollectionName = "leases",
                CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> input, ILogger log)
            {
                var maintenancepredictions = cosmosClient.GetContainer(databaseId, outputContainerId);

                foreach(Document doc in input) {
                    InputData payload = new InputData();
                    var machinetemp = doc.GetPropertyValue<double>("MachineTemperature");
                    var pressure = doc.GetPropertyValue<double>("Pressure");

                    payload.data = new List<PredictionInput> {
                        new PredictionInput { MachineTemperature = machinetemp, Pressure = pressure }
                    };

                    try
                    {
                        // Call Azure ML.  Get back response.
                        HttpClient client = new HttpClient();
                        var request = new HttpRequestMessage(HttpMethod.Post, new Uri(azureMLEndpointUrl));
                        request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                        
                        request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                        var response = client.SendAsync(request).Result;

                        // Azure ML returns an array of integer results, one per input item.
                        var predictions = JsonConvert.DeserializeObject<List<int>>(response.Content.ReadAsStringAsync().Result);
                        
                        // Add the recommendation to our document.
                        // We process one message at a time, so expect one result.
                        doc.SetPropertyValue("Maintenance", predictions.ElementAt(0));

                        // Write the updated document back to Cosmos DB into a new container.
                        await maintenancepredictions.CreateItemAsync<Document>(doc);
                    }
                    catch (Exception e) {
                        log.LogError("Exception pushing prediction into the maintenance predictions container: " + e);
                    }
                }
            }
        }
    }
    ```

4. In the Visual Studio Code terminal, enter the following commands.

    ```
    dotnet add package Microsoft.Azure.Cosmos
    dotnet restore
    dotnet build
    ```

    After running these commands, you should receive a message that the build succeeded.

    ![The Azure Function built successfully.](media/code-function-build-succeeded.png 'Build succeeded.')

5. Select the **WriteMaintenancePredictionToCosmos** function and then select the **Deploy to Function App...** operation. You may need to move the mouse to the top-right corner of the Functions tab to view this option.

    ![Deploy to Function App is selected.](media/code-function-deploy-2.png 'Deploy to Function App...')

6. Choose the appropriate subscription from the list. Then, choose the Function App starting with **modernize-app** from the Function App list.

    ![The appropriate Function App is selected.](media/code-function-deploy-app.png 'Select Function App in Azure')

7. Select **Deploy** in the ensuing modal dialog.

    ![The option to deploy is selected.](media/code-function-deploy-check.png 'Deploy')

8. To test the function, navigate to your Cosmos DB account and select **Data Explorer**. Drill down through the **sensors** database to the **maintenancepredictions** container and select **Items**.

    >**NOTE**: It may take 2-3 minutes for the new Azure Function to populate data into the **maintenancepredictions** container. If you do not see data after several minutes, navigate to the Overview panel of the App Service and **Stop** and then **Start** the functions.

    ![Maintenance recommendations are coming in.](media/azure-cosmos-maintenance-recommendations.png 'Maintenance recommendations')

### Task 3: Use Synapse Link to load into an Azure Synapse Analytics SQL pool

1. In the [Azure portal](https://portal.azure.com), type in "azure synapse analytics" in the top search menu and then select **Azure Synapse Analytics (workspaces preview)** from the results.

    ![In the Services search result list, Azure Synapse Analytics (workspaces preview) is selected.](media/azure-create-synapse-search.png 'Azure Synapse Analytics (workspaces preview)')

2. Select the workspace you created before the hands-on lab.

    ![The Azure Synapse Analytics workspace for the lab is selected.](media/azure-synapse-select.png 'modernizeapp workspace')

3. Select **Launch Synapse Studio** from the Synapse workspace page.

    ![Launch Synapse Studio is selected.](media/azure-synapse-launch-studio.png 'Launch Synapse Studio')

4. In the Azure Synapse workspace, select the **>>** chevron to expand icons to include names and then select **Develop**.

    ![The Develop option is selected.](media/azure-synapse-workspace-develop.png 'Develop')

13. Select the **+** and then choose **Notebook** from the ensuing dropdown list.

    ![Add a new Notebook.](media/azure-synapse-develop-notebook.png 'Add a new Notebook')

14. In the **Properties** tab, change the **Name** to `Write Maintenance Predictions to SQL Pool`.

15. In the notebook toolbar, change the language to **Spark (Scala)**. [The Azure Synapse Apache Spark connector to Synapse SQL connector currently only supports Scala](https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export), so the default language of PySpark will not work.

    ![Change the notebook language to Scala.](media/azure-synapse-develop-cosmos-language.png 'Spark (Scala)')

16. In the main section, select **{ } Add Code** and paste in the following code to retrieve data from the `maintenancepredictions` container and select only the necessary columns.

    ```
    val df = spark.read.format("cosmos.olap").
        option("spark.synapse.linkedService", "modernize_app_cosmos").
        option("spark.cosmos.container", "maintenancepredictions").
        load().
        select($"FactoryId", $"machineid", $"ProcessingTime",
            $"MachineTemperature", $"Pressure", $"Maintenance")

    df.show(10)
    ```

    Then, run the code block by selecting **Run** (shaped like a play button) or by pressing `Shift+Enter` when inside the notebook cell. After the job completes, you should see a table with the first ten anomalous results, assuming there are at least ten anomalous results by the time you reach this step.

    ![Code to connect to Cosmos DB is entered and ready to run.](media/azure-synapse-develop-cosmos-connect.png 'Connect to Cosmos DB from a notebook')

    >**NOTE**: it may take several minutes for a Spark session to start. The notebook cell output text will read "Starting Spark session" during this time.

17. Below the first code block, select **{ } Add Code** and add a new code block. Enter and then run the following code to create a new SQL Pool table with machine anomaly data.

    ```
    // Ensure that this table does not already exist on your SQL Pool; otherwise, you will get an error!
    df.write.sqlanalytics("modernapp.dbo.MaintenancePredictions", Constants.INTERNAL)
    ```

18. After the code segment completes, select **{ } Add Code** and add a new block with the following code to ensure that the SQL pool has loaded correctly.

    ```
    val sqldf = spark.read.sqlanalytics("modernapp.dbo.MaintenancePredictions") 
    sqldf.show(10)
    ```

    ![The SQL Pool results are returned.](media/azure-synapse-develop-cosmos-success-2.png 'Retrieve data from a SQL Pool')

## Exercise 8: View the factory status in a Power BI report

Duration: 15 minutes

\[insert your custom Hands-on lab content here . . .\]

### Task 1: Task name

1.  Number and insert your custom workshop content here . . .

    -  Insert content here

        -  
        
### Task 2: Task name

1.  Number and insert your custom workshop content here . . .

    -  Insert content here

        -  
        
## After the hands-on lab 

Duration: 10 minutes

### Task 1: Delete Lab Resources

1. Log into the [Azure Portal](https://portal.azure.com).

2. On the top-left corner of the portal, select the menu icon to display the menu.

    ![The portal menu icon is displayed.](media/portal-menu-icon.png "Menu icon")

3. In the left-hand menu, select **Resource Groups**.

4. Navigate to and select the `modernize-app` resource group.

5. Select **Delete resource group**.

    ![On the modernize-app resource group, Delete resource group is selected.](media/azure-delete-resource-group-1.png 'Delete resource group')

6. Type in the resource group name (`modernize-app`) and then select **Delete**.

    ![Confirm the resource group to delete.](media/azure-delete-resource-group-2.png 'Confirm resource group deletion')

You should follow all steps provided *after* attending the Hands-on lab.
