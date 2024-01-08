Introduction:
-------------
* In my current team we have about 11 devops engineers but we are splitted based on product
* I work for fintech. My product is business side you have the business users, the SM users like small and medium enterprises and then you have the regular users that want to use your fintech app, then you have the team that manages the card payment ensuring that they confirm the security best practices. 
* I am an SRE and Devops Engineer and my roles and resposibilities ranges from managing kubernetes clusters on both test and production environments, managing their docker files, managing their AWS and Azure infrastructure. We use both clouds simultaneously because our test environment is on AWS and our production environment is on Azure. 
  * I also manage the terraform code 
  * I don't manage the databases but I manage the network connection for instance if we need to create a point to site VPN for say a new DB Admin on the team that's part of my responsibility and if for some reason we need to create a site to site VPN for a customer that's also me doing that for my team
  * Cost optimization of AWS and Azure infrastruction is also part of my responsibility. If I come up with policy that ensures cost is cut down I have to propagate to the rest of the devops engineers this is what we need to be doing ensure that all your services have say HPA implemented fully
  * I wrote a Lambda function that is supposed to check for unattach EBS volumes. I would'nt run it in my own environment I have to run it across when I identify those volumes I will ask them okay these are the volumes that are not used go through the list before I take them down. I say this because from my experience I experienced that people create the EC2 instances quickly and take it down but the storage attached to it sometimes doesn't get deleted so you see a lot of volumes dangling around so it is part of my responsibility to ensure cost optimization in those scenarios.
 * For configuration management we use ansible but so much as terraform.

Questions:
----------

1. You see a sudden and sustained spike in database errors in your datadog dashboards. While the initial investigation reveals resource constraint they don't fully explaing the magnitude of the spike (they do not explain exactly what is happening ) How exactly as an SRE and devops Engineer you would identify the root cause of the error search using datadog features and integrations?

* Answer:
---------
* We can use APM(Application Performance Monitoring) profiling to profile slow database queries and identify potential problematic statements.
*  There is something known as anamoly detection so we can consider datadog anamoly detection algorithms to identify unusual patterns in data which might reveal previously unnoticed correlations 
* We can assess the network performance between your application servers and the database. Network issues can sometimes lead to increased error rates.
* We can check for recent code changes or deployments around the time of the error spike. Sometimes, a new release or a code change can introduce unforeseen issues.

2. Sometimes what happens is there is a performance degradation during any major event. Lets say there is a large user influx event like amazon sale, your website performance significantly degrades leading to complaints and potential revenue loss when you take a look at datadog dashboard they show increased error rates and API responses time. As a Devops Engineer how would you optimize performance during these kind of situation?

* Answer:
---------
* Resource utilization analysis you can monitor the resource utilization of key infrastructure components like host containers and load balancers to identify the bottlenecks
* We have the feature of scalability testing as well.
* We can leverage data using synthetic testing feature to proactively simulate the user influx and identify potential scaling issues before the actual event 
* There is a feature of dynamic dashboard and there is a feature of alert in which you can utilize data to alert escalation features to notify relevant teams instantly about the critical performance.

1. Scaling Resources:
Horizontal Scaling: Consider auto-scaling your infrastructure to handle increased demand. Configure auto-scaling groups to dynamically add or remove instances based on traffic patterns.
Vertical Scaling: If possible, scale up the resources (CPU, memory) on existing instances to handle increased load.
2. Content Delivery Network (CDN):
Leverage a CDN to distribute static assets closer to users, reducing the load on your servers. This can improve page load times and decrease the strain on your infrastructure.
3. Caching:
Implement aggressive caching strategies for both static and dynamic content. Utilize caching mechanisms at different layers, such as content delivery caching, server-side caching, and database query caching.
4. Load Balancing:
Ensure your load balancers are configured to distribute traffic efficiently across your infrastructure. Consider intelligent load balancing algorithms that take into account server health and capacity.
5. Database Optimization:
Optimize database queries to reduce response times. Use database indexing, query optimization, and caching strategies to handle increased database loads.
Consider read replicas to offload read operations from the primary database.
6. Asynchronous Processing:
Offload time-consuming tasks to asynchronous queues or background jobs to improve the responsiveness of your application. This can prevent the main application from being bogged down.
7. Monitoring and Alerting:
Set up proactive monitoring and alerting in Datadog to detect performance issues in real-time. Configure alerts for increased error rates, high API response times, and other relevant metrics.
8. Auto-Scaling Policies:
Define auto-scaling policies based on specific performance metrics. For example, automatically increase the number of instances if the CPU utilization exceeds a certain threshold.
9. Distributed Tracing (APM):
Use Datadog's APM features to trace and profile transactions across your application. Identify performance bottlenecks and optimize critical paths.
10. Static Content Optimization:
Utilize techniques like file compression, image optimization, and minification to reduce the size of static assets and improve load times.
11. Pre-Warming:
Pre-warm caches and connections in anticipation of the event. This ensures that systems are ready to handle the surge in traffic without experiencing delays due to cold starts.
12. Failover Strategies:
Implement failover strategies to handle potential service disruptions. Ensure that critical services have redundant components and failover mechanisms in place.
13. Performance Testing:
Conduct thorough performance testing before major events to identify potential bottlenecks and weaknesses in your infrastructure. Use tools like load testing to simulate high traffic scenarios.
14. Collaboration with Development:
Collaborate closely with the development team to identify and address application-level optimizations. Work together to streamline code and improve efficiency.
15. Post-Event Analysis:
After the event, conduct a post-mortem analysis to identify what worked well and areas for improvement. Use this information to refine your performance optimization strategies for future events.
By implementing these strategies and leveraging Datadog for real-time monitoring and analysis, you can enhance the performance of your infrastructure during major events and mitigate the risk of revenue loss due to degraded user experience.

3. You use both terraform and ansible in your organization which one do you prefer and why?

* Answer:
---------
* Both Terraform and Ansible are powerful tools for implementing Infrastructure as Code, but they have different strengths.
* Terraform is geared towards service and cloud orchestration, while Ansible is optimized for configuration management. 
* Some people prefer to use Terraform for infrastructure orchestration and Ansible for configuration management, while others use Ansible to provision cloud infrastructure. 
* Ultimately, the choice between the two tools depends on your organization's IT priorities and the specific tasks you need to accomplish. In some cases, it may be best to use both tools together.

4. Why do we need state file in terraform?
* Answer:
---------
* In Terraform, the state file is used to keep track of the resources created by your configuration and map them to real-world resources. 
* It is a JSON-encoded file that Terraform writes and reads at each operation. 
* The state file is used to determine which changes to make to your infrastructure and to improve performance for large infrastructures. 
* It is stored by default in a local file named "terraform.tfstate", but it is recommended to store it in Terraform Cloud to version, encrypt, and securely share it with your team. 
* The state file should not be manually modified, and direct file editing of the state is discouraged. 
* Terraform provides the terraform state command to perform basic modifications of the state. The state file format is subject to change in new Terraform versions, so it is important to ensure that the one-to-one rule is followed. 

5. Consider that the state file was deleted how to recover it?
* Answer:
---------
If the Terraform state file is deleted, you can proceed with the following steps to recover:
* Restore from a backup: If you have a backup of the state file, you can restore it by moving the backup file to the location of the original state file and renaming it as terraform.tfstate. After that, run terraform init to re-initialize the state file
* `mv terraform.tfstate.backup terraform.tfstate`
  `terraform init`
* Recover from corruption: If the state file is corrupted, you can try using the terraform state push and terraform state pull commands to recover the state
  `terraform state push`
  `terraform state pull`
* Import existing resources: If you have made direct changes to the Azure resources from the Azure portal, you can use the `terraform import` command to import the existing resources into the Terraform state file
  `terraform import .`
* Re-import the resource: If you don't have a suitable state file, you can remove the bad resource from your current state file using the terraform state rm command and then re-import the resource using the terraform import command
  `terraform state rm <resource_type> <resource_name>`
  `terraform import .`

6. what are some best practices for managing terraform state files?
* Answer:
----------
* Remote State Storage: Instead of storing the state file locally, use remote state storage like Amazon S3, Azure Storage, or other cloud-based solutions. This allows for efficient sharing, recovery, and collaboration among team members
  `terraform init -backend-config "bucket"="your_bucket_name" "key"="path/to/your/state/file"`
* Separate State Files for Different Environments: Create separate state files for different environments, such as production, staging, and development. This prevents changes in one environment from affecting others
  `terraform init -project-id "your_project_id" -backend-config "bucket"="your_bucket_name" "key"="path/to/your/state/file_dev"`
* Version Control: Utilize version control (e.g., Git) for your Terraform configuration files and remote state storage. This allows you to track changes, collaborate with your team, and maintain an audit trail of each modification
  `git add .`
  `git commit -m "Add initial state file"`
  `git push`
* State Locking: Implement state locking to prevent multiple users from accidentally modifying the same state file at the same time. Terraform's native state locking feature can be used, or you can use third-party tools for more advanced locking options
* Backup State Files: Regularly back up your state files to ensure recovery in case of corruption or loss
  `terraform state pull`
* Sensitive Data Caution: Be cautious when handling sensitive data in the Terraform state file. Use dedicated secrets management tools and avoid storing sensitive information in the state file
* Avoid Direct File Editing: Do not directly edit the state file, as this can lead to inconsistencies and errors. Use Terraform commands like terraform state to perform basic modifications of the state file.

7. Can you walk me through what you have done in kubernetes?
* Answer:
* When we write our CI/CD pipeline it pulls the image from ECR and then it deploys to EKS.
* My roles and responsibilities in kubernetes are 
  * Creating the Kubernetes cluster
  * Managing the ingress and egress rules
  * Managing the manifest files using customized to manage the config map generation
  * Deploying, monitoring the applications in a Kubernetes environment
  * Configuring hardware, services, managing settings and storage
  * Improve the monitoring and alert system
  * Troubleshoot issues and solve problems where needed

8. Walk me through a recent issue you have resolved in kubernetes?
* Answer:
---------
* A developer approached me on test environment saying that he has this issue like while he is testing out his API sometimes he makes the request and he makes thorugh but some times he fails. He does not understand why this is and he approached me 
  * I went into the cluster and I started trouble shooting and I discovered that the problem is it was drawing out of memory that's OOMKilled error so that's out of memory exception so what was happening was some times he makes a request and he goes through and after making subsequent requests it is consuming so much of memory and slowing down. Basically its a test environment so you dont spend much cost on test environments. 
  * Our restart policy in kubernetes is like whenever a pod crashes it will restart that pod again so if the request was placed during the time when pod crashed his request is failing.
  * I discussed the same thing to him and I have increased the memory capacity for that particular microservice and that's how the issue got resolved.

9. You have a critical application running on EKS with two replicas in a single essential pod disruption budget(PDB). A node needs scheduled maintenance. During the maintenance one replica inevitably get evicted as there is only one node, however the application requires atleast one healthy replica available all the time. How will you solve it?
* Answer:
---------
* Step-1: 

Drain the node
`kubectl drain `
This will gracefully evict the pod from the node and it will reschedule it to another healthy node then you can ensure that PDB minimum available replicas are still met while migrating

* Step-2:
----------
* Next step is to use rolling updates with surge and readiness probe so you can configure a deployment both with surge and readiness probes.
  Because of that what will happen is 