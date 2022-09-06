## Part A: Create, launch a AWS cloud instance

1. Signing in to EC2 console

Sign in to your AWS console using the provided link provided to you at the workshop.
It looks like: https://[somenumber].signin.aws.amazon.com/console

<img src="https://user-images.githubusercontent.com/1218301/185639392-8e1aae31-4c34-4c13-b5d5-333c48b0f141.png" width=300>

Leave the account ID as-is, input the username (potsdam-participantXX) and provided password.

You should see this screen:

![image](https://user-images.githubusercontent.com/1218301/185641343-6be184ad-ef9d-4a48-b7e2-b6aa5640cd4c.png)

2. Change your region

Make sure your AWS region is 'US-west-2' (Oregon). Note that the SRA is mirrored at us-east-1, yet many T2T data is on us-west-2. You can launch instances anywhere in the world, but in this session we will want to use instances that are close to T2T data.

To change the AWS region, click on the top right:

![image](https://user-images.githubusercontent.com/1218301/185642305-ca474650-c008-4b31-8ef3-ec9e8b4b8e19.png)

then:

![image](https://user-images.githubusercontent.com/1218301/188676607-f788a8f1-e2a1-447d-b64e-55791cb643f8.png)


3. Launch a cloud instance

Next, go to "Launch a virtual machine". You should see:

![image](https://user-images.githubusercontent.com/1218301/185641472-a3a1251a-9f03-4508-8f00-36422820656d.png)

Give a meaningful name to your machine, preferably one that includes your username, as the list of launched machines will be shared across all participants. E.g.: "participant1-mymachine". Then, select Amazon Linux as distribution (latest version is fine).

Click on the instance type to change it. I recommend "c5a.8xlarge", as it is one of the cheapeast options that guarantees ultra-fast download speeds (10 Gbits).

![image](https://user-images.githubusercontent.com/1218301/185644210-89c943df-8d5a-4a9a-8435-cda9fe92f28d.png)

Then, create your own keypair. It is important as this will allow you to connect securely to the instance via SSH. When your keypair is created, your browser will automatically download it. Keypairs are confidential, and should never be published publicly. At the end of today's session, you may delete the keypair, as it will not be used anymore.

![image](https://user-images.githubusercontent.com/1218301/185644300-9eef49cf-0ed8-406f-b8fd-97cdf484a1b9.png)

You may leave security group as default:

![image](https://user-images.githubusercontent.com/1218301/185644401-8bb8460d-777d-47a1-b265-1c4d1b4bcfd4.png)

For storage, allocate a 30 GB root volume:

![image](https://user-images.githubusercontent.com/1218301/185644922-931504a4-f73c-48e7-9c10-dd09b62700f2.png)

Finally, review all the details on the right of the screen and click 'Launch instance'. (This is a misnomer, it should be called 'Create instance').

![image](https://user-images.githubusercontent.com/1218301/185645086-67cfe340-9463-4e5c-87f2-8e73a05ca9b0.png)

If all went well, you should see this in the instance panel of the EC2 console:

![image](https://user-images.githubusercontent.com/1218301/185645976-89fdea52-af39-4463-812d-d05b08e274a1.png)

You may now connect to your instance using the public IP and the keypair. The default linux username is 'ec2-user' and there is no password. If you managed to connect, you will see this:

![image](https://user-images.githubusercontent.com/1218301/185646720-adfd54b7-b126-4891-9c71-4b2d2bbb0f19.png)

At this point, congratulations! you have successfully created and connected to a AWS instance. The machine is ready to perform bioinformatics analyses.