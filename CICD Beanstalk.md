![alt text](image-1.png)

![alt text](image-2.png)

- Support blue/green deployment, by creating a new environment for the updated version, testing it, and then swaping CNAME(DNS Alias) to route a traffic from old to new environment
- To perform a CNAME swap, a custom action is required. CodeDeploy native Elastic beanstalk doesn't automatically handle CNAME swaping for Blue/green, using a lambda function to do it.