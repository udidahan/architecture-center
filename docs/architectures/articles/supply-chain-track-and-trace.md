---
title: Supply Chain Track and Trace 
description: Learn how to use the Azure Blockchain Workbench. Build an asset tracking application for supply chain with a step-by-step flowchart.
author: adamboeglin
ms.date: 10/29/2018
---
# Supply Chain Track and Trace 
A common blockchain pattern is IoT-enabled monitoring of an asset as it moves along a multi-party supply chain. A great example of this pattern is the refrigerated transportation of perishable goods like food or pharmaceuticals where certain compliance rules must be met throughout the duration of the transportation process. In this scenario, an initiating counterparty (such as a retailer) specifies contractual conditions, such as a required humidity and temperature range, that the custodians on the supply chain must adhere to. At any point, if the device takes a temperature or humidity measurement that is out of range, the smart contract state will be updated to indicate that it's out of compliance, recording a transaction on the blockchain and triggering remediating events downstream.

## Architecture
<img src="media/supply-chain-track-and-trace.svg" alt='architecture diagram' />

## Data Flow
1. IoT devices communicate with IoT Hub. IoT Hub as a route configured that will send specific messages to a Service Bus associated with that route. The message is still in the native format for the device and needs to be translated to the format used by Azure Blockchain Workbench.

An Azure Logic App performs that transformation. It is triggered when a new message is added to the Service Bus associated with the IoT hub, it then transforms the message and delivers it to the Service Bus used to deliver messages to Azure Blockchain workbench.

The first service bus effectively serves as an "Outbox" for IoT Hub and the second one serves as an "Inbox" for Azure Blockchain Workbench.
1. DLT Consumer fetches the data from the message broker (Service Bus) and sends data to Transaction Builder - Signer.
1. Transaction Builder builds and signs the transaction.
1. The signed transaction gets routed to the Blockchain (Private Ethereum Consortium Network).
1. DLT Watcher gets confirmation of the transaction commitment to the Blockchain and sends the confirmation to the message broker (Service Bus).
1. DB consumers send confirmed blockchain transactions to off-chain databases (Azure SQL Database).
1. Information analyzed and visualized using tools such as Power BI by connecting to off-chain database (Azure SQL Database).
1. Events from the ledger are delivered to Event Grid and Service Bus for use by downstream consumers. Examples of "downstream consumers" include logic apps, functions or other code that is designed to take action on the events. For example, an Azure Function could receive an event and then place that in a datastore such as SQL Server.

## Components
* Application Insights: Detect issues, diagnose crashes, and track usage in your web app with Application Insights. Make informed decisions throughout the development lifecycle.
* [Web Apps](href="http://azure.microsoft.com/services/app-service/web/): Quickly create and deploy mission critical web apps at scale
* [Storage](href="http://azure.microsoft.com/services/storage/): Durable, highly available, and massively scalable cloud storage
* [Virtual Machines](href="http://azure.microsoft.com/services/virtual-machines/): Provision virtual machines for Ubuntu, Red Hat, and more
* [Azure Active Directory](href="http://azure.microsoft.com/services/active-directory/): Synchronize on-premises directories and enable single sign-on
* [Azure SQL Database](http://azure.microsoft.com/services/sql-database/) is a relational database service that lets you rapidly create, extend, and scale relational applications into the cloud.
* [Azure Monitor](href="http://azure.microsoft.com/services/monitor/): Highly granular and real-time monitoring data for any Azure resource.
* [Service Bus](href="http://azure.microsoft.com/services/service-bus/): Connect across private and public cloud environments
* [Event Grid](href="http://azure.microsoft.com/services/event-grid/): Get reliable event delivery at massive scale

## Next Steps
* [Find run-time exceptions with Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions/)
* [Create a blockchain app with Azure Blockchain Workbench](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-create-app/)
* [Azure Storage on Blockchain Workbench](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-architecture#azure-storage)
* [Azure and Linux Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/overview/)
* [Blockchain Workbench API app registration](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-deploy#blockchain-workbench-api-app-registration)
* [Blockchain Workbench Database](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-getdb-details/)
* [Log Analytics Tutorial](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata/)
* [Service Bus on Blockchain Workbench](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-messages-overview#using-service-bus-topics-for-notifications)
* [Event Notifications on Blockchain Workbench](https://docs.microsoft.com/azure/blockchain-workbench/blockchain-workbench-messages-overview#event-notifications)