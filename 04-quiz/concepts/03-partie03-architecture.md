-------------------------------------
# Architecture
-------------------------------------

![image](https://github.com/user-attachments/assets/0828fda0-85e7-4781-abcd-b271bbc7cd2d)

-------------------------------------
# Explication:
-------------------------------------

This architecture shows a streaming data pipeline with several AWS services working together to analyze and store streaming data. Let's break it down:

### Storage Location
The storage is not explicitly shown in the diagram, but there are two implicit storage locations:
1. **Amazon OpenSearch Service**: This is used to index and store the data, making it available for querying and visualization via **OpenSearch Dashboards**.
2. **Amazon Kinesis Data Firehose**: While not a persistent storage solution itself, Firehose typically delivers data to external storage like Amazon S3, Amazon Redshift, or other analytics services. So, although the diagram does not specify where Firehose delivers the data, it's implied that it sends it to a storage location (like S3).

### Why Use Kinesis Data Streams and Firehose Together?
- **Kinesis Data Streams**: This is designed for real-time processing of data streams. It provides low-latency, highly reliable ingestion of large streams of data, allowing for real-time data processing. In this architecture, Kinesis Data Streams is used as the initial entry point for high-throughput real-time data.
  
- **Kinesis Data Firehose**: Firehose is used to load data into a target service, such as OpenSearch Service or an S3 bucket. Firehose is more of a fully-managed service that handles batching, compressing, and delivering streaming data. It simplifies the process of sending processed data to storage or analytics tools. It’s used here to bridge real-time data from Kinesis Data Streams into a storage or processing service (like OpenSearch).

### Why Use Kinesis Streams Before Firehose?
Kinesis Data Streams comes **first** because:
1. **Real-time processing**: Kinesis Data Streams is ideal for ingesting and processing data in real-time. 
2. **Customization and flexibility**: Kinesis Data Streams allows for more control and custom logic to be applied (for example, AWS Lambda functions or other consumers can process this data as it flows through).
3. **Firehose is not real-time**: Kinesis Firehose, on the other hand, is built for simpler, near-real-time delivery. It buffers data and processes it before sending it to its destination. Firehose does not offer the same level of control over individual data records that Streams does.

The sequence typically involves real-time ingestion via **Kinesis Data Streams** and delivery or batch processing via **Kinesis Data Firehose**.

### Why Use Lambda?
**AWS Lambda** is used here for:
1. **Real-time processing**: Lambda can process the data flowing through Kinesis Data Streams, transforming or enriching it as needed before passing it to Firehose or other services.
2. **Serverless execution**: Lambda functions allow for easy, automatic processing of data without managing servers. It executes custom code based on incoming events (like incoming data in Kinesis).
   
### Why Not Use Kinesis Streams Alone?
- **Kinesis Data Streams** alone is used for real-time processing, but it doesn't handle buffering, batch delivery, or loading data into final destinations as efficiently as Firehose.
- **Firehose** simplifies the delivery of data to storage (like S3) or analytics services (like OpenSearch) and can automatically manage buffering and retries for you.
  
Using **Kinesis Data Streams + Lambda** gives you more flexibility in how you process and transform the data in real-time, while **Firehose** helps you efficiently deliver that processed data to storage services.

In summary:
- **Streams** for real-time ingestion and flexibility in data processing.
- **Firehose** for easier delivery to final storage or analytics services.
- **Lambda** for custom, on-the-fly processing.

This combination provides flexibility, scalability, and efficiency in handling both real-time and near-real-time data processing.
