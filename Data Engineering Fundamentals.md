# Data Engineering Fundamentals

## What Is Data Engineering?

Data engineering is the development, implementation, and maintenance of systems and processes that take in raw data and produce high-quality, consistent information that supports downstream use cases, such as analysis and machine learning. Data engineering is the intersection of security, data management, DataOps, data architecture, orchestration, and software engineering. A data engineer manages the data engineering lifecycle, beginning with getting data from source systems and ending with serving data for use cases, such as analysis or machine learning.

The stages of the data engineering lifecycle are as follows: 

- Generation
- Storage
- Ingestion
- Transformation
- Serving

The Oxford Dictionary defies Big Data as “extremely large data sets that may be analyzed computationally to reveal patterns, trends, and associations, especially relating to human behavior and interactions.”

### Data engineering and Data Science

Data engineering sits upstream from data science (Figure 1-4), meaning data engineers provide the inputs used by data scientists (downstream from data engineering), who convert these inputs into something useful.

### What a Data Engineer needs to know?

By definition, a data engineer must understand both data and technology, a data engineer must be aware of various options for tools, their interplay, and their trade-offs, must also understand the requirements of data consumers (data analysts and data scientists)

Skill set:

- SQL and Data Warehousing Fundamentals: Data engineers need to know how to query databases, and SQL is the universal language to do so. Experienced data engineers know how to write high-performance SQL and understand the fundamentals of data warehousing and data modeling.
- Python and/or Java: The language in which a data engineer is proficient will depend on the tech stack of their team. Python and Java currently dominate in data engineering, but newcomers like Go are emerging.
- Distributed Computing Solving: a problem that involves high data volume and a desire to process data quickly has led data engineers to work with distributed computing platforms. Distributed computing combines the power of multiple systems to efficiently store, process, and analyze high volumes of data. eg. The Hadoop ecosystem, which includes distributed file storage via Hadoop Distributed File System (HDFS), processing via MapReduce, data analysis via Pig, and more.
- Basic System Administration: A data engineer is expected to be proficient on the Linux command line and be able to perform tasks such as analyze application logs, schedule cron jobs, and troubleshoot firewall and other security settings.
- A Goal-Oriented Mentality: A good data engineer doesn’t just possess technical skills. They may not interface with stakeholders on a regular basis, but the analysts and data scientists on the team certainly will. The data engineer will make better architectural decisions if they’re aware of the reason they’re building a pipeline in the first place.

Business Responsabilities

- Know how to communicate with nontechnical and technical people.
- Understand how to scope and gather business and product requirements.
- Understand the cultural foundations of Agile, DevOps, and DataOps.
- Control costs
- Learn continuously

## What is Data Enngineering Lifecycle?

The data engineering lifecycle comprises stages that turn raw data ingredients into a useful end product, ready for consumption by analysts, data scientists, ML engineers, and others.

We begin the data engineering lifecycle by getting data from source systems and storing it. Next, we transform the data and then proceed to our central goal, serving data to analysts, data scientists, ML engineers, and others.

![Screen Shot 2023-01-11 at 14.42.56.png](https://github.com/miguelmont/Professional-Data-Engineer/blob/main/Data%20Engineering%20Fundamentals%20images/Screen_Shot_2023-01-11_at_14.42.56.png)

### Generation: Source Systems

A source system is the origin of the data used in the data engineering lifecycle. For example, a source system could be an IoT device, an application message queue, or a transactional database.

### Storage

You need a place to store data. Choosing a storage solution is key to success in the rest of the data lifecycle, and it’s also one of the most complicated stages of the data lifecycle. 
Storage runs across the entire data engineering lifecycle, often occurring in multiple places in a data pipeline, the way data is stored impacts how it is used in all of the stages of the data engineering lifecycle.  

A few key engineering questions to ask when choosing a storage system for a data warehouse, data lakehouse, database, or object storage:

- Is this storage solution compatible with the architecture’s required write and read speeds?
- Will storage create a bottleneck for downstream processes?
- Will this storage system handle anticipated future scale?
- Will downstream users and processes be able to retrieve data in the required service-level agreement (SLA)?
- Is this a pure storage solution (object storage), or does it support complex query patterns
Is the storage system schema-agnostic (object storage)? Flexible schema (Cassandra)? Enforced schema (a cloud data warehouse)?
- How are you handling regulatory compliance and data sovereignty?

Data access frequency will determine the temperature of your data.

- Data that is most frequently accessed is called hot data.
- Cold data is seldom queried and is appropriate for storing in an archival system.

### Ingestion

After you understand the data source, the characteristics of the source system you’re using, and how data is stored, you need to gather the data.

Key engineering considerations for the ingestion phase : 

- What are the use cases for the data I’m ingesting?
- Can I reuse this data rather than create multiple versions of the same dataset?
- Are the systems generating and ingesting this data reliably, and is the data available when I need it?
- What is the data destination after ingestion?
- How frequently will I need to access the data?
- In what volume will the data typically arrive?
- What format is the data in?
- Can my downstream storage and transformation systems handle this format?
- Is the source data in good shape for immediate downstream use?
- If so, for how long, and what may cause it to be unusable?
- If the data is from a streaming source, does it need to be transformed before reaching its destination?

Data is nearly always produced and updated continually at its source. 

Batch ingestion is simply a specialized and convenient way of processing this stream in large chunks—for example, handling a full day’s worth of data in a single batch. Batch data is ingested either on a predetermined time interval or as data reaches a preset size threshold. Batch ingestion is a one-way door: once data is broken into batches, the latency for downstream consumers is inherently constrained.

Streaming ingestion allows us to provide data to downstream systems—whether other applications, databases, or analytics systems—in a continuous, real-time fashion means that the data is available to a downstream system a short time after it is produced (e.g., less than one second later). The latency required to qualify as real-time varies by domain and requirements. 

![Screen Shot 2023-01-12 at 0.32.06.png](https://github.com/miguelmont/Professional-Data-Engineer/blob/main/Data%20Engineering%20Fundamentals%20images/Screen_Shot_2023-01-11_at_14.42.56.png)

Key considerations for batch versus stream ingestion

- If I ingest the data in real time, can downstream storage systems handle the rate of data flow?
Do I need millisecond real-time data ingestion? Or would a micro-batch approach work, accumulating and ingesting data, say, every minute?
- What are my use cases for streaming ingestion?
- What specific benefits do I realize by implementing streaming?
- If I get data in real time, what actions can I take on that data that would be an improvement upon batch?
- Will my streaming-first approach cost more in terms of time, money, maintenance, downtime, and opportunity cost than simply doing batch?
- Are my streaming pipeline and system reliable and redundant if infrastructure fails? What tools are most appropriate for the use case?
- If I’m deploying an ML model, what benefits do I have with online predictions and possibly continuous training?

Push vs Pull

- In the push model of data ingestion, a source system writes data out to a target, whether a database, object store, or filesystem.
- In the pull model, data is retrieved from the source system.

With streaming ingestion, data bypasses a backend database and is pushed directly to an endpoint, typically with data buffered by an event-streaming platform. This pattern is useful with fleets of IoT sensors emitting sensor data.

### Transformation

After you’ve ingested and stored data, you need to do something with it. The next stage of the data engineering lifecycle is transformation, meaning data needs to be changed from its original form into something useful for downstream use cases.

Immediately after ingestion, basic transformations map data into correct types (changing ingested string data into numeric and date types, for example), putting records into standard formats, and removing bad ones.

Key considerations for the transformation phase:

- What’s the cost and return on investment (ROI) of the transformation?
- What is the associated business value?
- Is the transformation as simple and self-isolated as possible?
- What business rules do the transformations support?

### Data Transformation vs Data Modeling

- Data transformation Transforming data is a broad term that is signified by the T in an ETL or ELT process. A transformation can be something as simple as converting a timestamp stored in a table from one time zone to another. It can also be a more complex operation that creates a new metric from multiple source columns that are aggregated and filtered through some business logic.

- Data modeling Data modeling is a more specific type of data transformation. A data model structures and defines data in a format that is understood and optimized for data analysis. A data model is usually represented as one or more tables in a data warehouse.

### Serving Data

Data has value when it’s used for practical purposes.

Data projects must be intentional across the lifecycle. What is the ultimate business purpose of the data so carefully collected, cleaned, and stored? Data serving is perhaps the most exciting part of the data engineering lifecycle. This is where the magic happens. This is where ML engineers can apply the most advanced techniques.

Analytics 

Analytics is the core of most data endeavors. Once your data is stored and transformed, you’re ready to generate reports or dashboards and do ad hoc analysis on the data.

Machine Learning

The emergence and success of ML is one of the most exciting technology revolutions. Once organizations reach a high level of data maturity, they can begin to identify problems amenable to ML and start organizing a practice around it.

A good data engineer is conversant in the fundamental ML techniques and related data-processing requirements, the use cases for models within their company, and the responsibilities of the organization’s various analytics teams.

- Is the data of sufficient quality to perform reliable feature engineering?
- Quality requirements and assessments are developed in close collaboration with teams consuming the data. Is the data discoverable?
- Can data scientists and ML engineers easily find valuable data?
- Where are the technical and organizational boundaries between data engineering and ML engineering?

Reverse ETL

Reverse ETL takes processed data from the output side of the data engineering lifecycle and feeds it back into source systems

## Major Undercurrents Across the Data Engineering Lifecycle

![Screen Shot 2023-01-11 at 14.44.05.png](Data%20Engineering%20Fundamentals%2035e3b59b1042475ab5468f2fe124bf8f/Screen_Shot_2023-01-11_at_14.44.05.png)

Data Modeling: 

Data modeling can happen almost anywhere in an organization. Firmware engineers develop the data format of a record for an IoT device, or web application developers design the JSON response to an API call or a MySQL table schema

Data modeling has become more challenging because of the variety of new data sources and use cases. For instance, strict normalization doesn’t work well with event data. Fortunately, a new generation of data tools increases the flexibility of data models, while retaining logical separations of measures, dimensions, attributes, and hierarchies. Cloud data warehouses support the ingestion of enormous quantities of denormalized and semistructured data, while still supporting common data modeling patterns, such as Kimball, Inmon, and Data Vault. Data processing frameworks such as Spark can ingest a whole spectrum of data, from flat structured relational records to raw unstructured text.

DataOps

DataOps maps the best practices of Agile methodology, DevOps, and statistical process control (SPC) to data.
Like DevOps, DataOps borrows much from lean manufacturing and supply chain management, mixing people, processes, and technology to reduce time to value.
DataOps is a collection of technical practices, workflows, cultural norms, and architectural patterns that enable: 

- Rapid innovation and experimentation delivering new insights to customers with increasing velocity
- Extremely high data quality and very low error rates
- Collaboration across complex arrays of people, technology, and environments
- Clear measurement, monitoring, and transparency of results

## What are Data Pipelines?

Data pipelines are sets of processes that move and transform data from various sources to a destination where new value can be derived. They are the foundation of analytics, reporting, and machine learning capabilities.

## Workflow Orchestration Platforms

Workflow orchestration platforms are also referred to as workflow management systems (WMSs), orchestration platforms, or orchestration frameworks.

Some platforms, such as Apache Airflow, Luigi, and Cloud Composer, are designed for more general use cases and are thus used for a wide variety of data pipelines. Others, such as Kubeflow Pipelines, are designed for more specific use cases and platforms (machine learning workflows built on Docker containers in the case of Kubeflow Pipelines).

### Directed Acyclic Graphs

Pipeline steps are always directed, meaning they start with a general task or multiple tasks and end with a specific task or tasks. This is required to guarantee a path of execution. In other words, it ensures that tasks do not run before all their dependent tasks are completed successfully. 

Pipeline graphs must also be acyclic, meaning that a task cannot point back to a previously completed task. In other words, it cannot cycle back. If it could, then a pipeline could run endlessly! With these two constraints in mind, orchestration pipelines produce graphs called directed acyclic graphs (DaGs).

DAGs are a representation of a set of tasks and not where the logic of the tasks is defined. An orchestration platform is capable of running tasks of all sorts.

1. The first executes a SQL script that queries data from a relational database and stores the result in a CSV file. 
2.  The second runs a Python script that loads the CSV file, cleans, and then reshapes the data before saving a new version of the file.  
3. Finally, a third task, which runs the COPY command in SQL, loads the CSV created by the second task into a Snowflake data warehouse.
