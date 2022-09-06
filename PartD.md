
## Part D: Cleaning up

To clean up the instances, go to the EC2 panel: [https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Instances:)

Then 'Terminate' your instance. This will delete it. Warning, this action cannot be undone and will also delete all the data you've analyzed.

![image](https://user-images.githubusercontent.com/1218301/185647144-165eedd5-86d1-47f7-9bf2-18b2903e1612.png)

![image](https://user-images.githubusercontent.com/1218301/185647192-154ac88d-563a-4d48-b9bb-d4cd8d962c10.png)

If it went well, the instance should now be marked terminated. It means the machine no longer exists and no costs are occurring for it.

In addition, you may go to the 'Volumes' panel to check that the corresponding volume is also gone (it should be automatically deleted when the instance is terminated). Not only instance, but also volumes occur (small) houry costs.

![image](https://user-images.githubusercontent.com/1218301/185647941-db982fd5-d2a0-41db-bafb-0c2202b119ac.png)
