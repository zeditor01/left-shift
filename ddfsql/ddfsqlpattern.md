# Db2 SQL Pattern 

The diagram below is representative of a typical deployment of Infosphere CDC in an enterprise 
with transactional databases on IBM Z. Data from sources such as IMS, Db2 and VSAM may be streamed
in near realtime to various targets (Kafka, LUW RDBMS's) for use cases such as data science, analytics or data integration.

![typical_cdc](./images/typical_cdc.JPG)




## Abstract
This github repository is dedicated to addressing the practical aspects of implementing sustainable CDC data replication solutions, primarily 
through the use of documenting worked examples of how to build CDC solutions that are easy to operate, manage and maintain.

This document is a reflection of the author's experiences in deploying CDC solutions at large enterprise clients, and contains 
many practical observations and recommendations. 
It is intended to be read in conjuction with 
the [official product documentation](https://www.ibm.com/docs/en/idr/11.4.0?topic=change-data-capture-cdc-replication), 
which is the definitive IBM-provided reference point of information for CDC Software Solutions.

* Author: neale.armstrong@au1.ibm.com
* IBM Z Data and AI Technical Specialist, IBM Australia.
* Last Updated: April 2023.

## Links to Documents in this repository
The content of this repository has been structured into separate documents as follows

1. [Establishing sustainable devops management of the designed CDC solution](https://github.com/zeditor01/cdc_examples/blob/main/create_scale_sustain_cdc_systems.md) (this document).
2. Deploying selected CDC agents... 
    * [Classic CDC for IMS](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_cdc_ims.md)
    * [CDC for DB2 z/OS](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_cdc_db2zos.md)
    * [Classic CDC for VSAM](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_cdc_vsam.md)
    * [CDC for Kafka](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_cdc_kafka.md)
    * [CDC for Db2 for Linux](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_cdc_db2linux.md)
    * [remote capture for VSAM](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_remotecdccapture_vsam.md)
    * [remote capture for Db2 z/OS](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_remotecdccapture_db2zos.md)
3. Deploying [Management Console and Access Server](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_admintools.md)
4. Deploying [CHCCLP for z/OS](https://github.com/zeditor01/cdc_examples/blob/main/documents/deploy_chcclp_zos.md)
5. [Developing CDC Subscriptions](https://github.com/zeditor01/cdc_examples/blob/main/documents/develop_subscriptions.md)
6. [Securing all points of the CDC solution - Authentication, Authorisation, Encryption](https://github.com/zeditor01/cdc_examples/blob/main/documents/securing_cdc.md)
7. [Monitoring and Alerting for CDC subscriptions](https://github.com/zeditor01/cdc_examples/blob/main/documents/monitoring_alerting.md)
8. [Basic Operations Management](https://github.com/zeditor01/cdc_examples/blob/main/documents/operations.md) 
9. [Performance Management](https://github.com/zeditor01/cdc_examples/blob/main/documents/performance.md)
10. [Devops approaches and Change Management](https://github.com/zeditor01/cdc_examples/blob/main/documents/devops_cdc.md)
11. [Shift-Right deployment options](https://github.com/zeditor01/cdc_examples/blob/main/documents/shift_right_cdc_operations.md) to facilitate an LUW operational control plane
12. [Shift-Left deployment options](https://github.com/zeditor01/cdc_examples/blob/main/documents/shift_left_cdc_operations.md) to facilitate a z/OS operational control plane.

## Contents of *this* document

1. The true nature of a heterogenous data replication project
2. Project Risk Mitigation
3. CDC Implementation (Theory) 
4. CDC Implementation (Practice)
5. Common Constraints that will make things difficult
6. Staggered Deployment Steps
7. Ongoing Devops after Successful Production Deployment
8. Shift Left or Shift Right for Devops Sustainability

## 1. The true nature of a heterogenous data replication project
Establishing CDC replication solutions in a "simple unconstrained environment" is fairly straightforward. 
* Install and configure the CDC components (capture agent, apply agent, admin tools)
* Define data replication subscriptions
* Start and monitor those subscriptions 

Simple huh ?
