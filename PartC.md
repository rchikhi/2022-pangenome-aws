##  Perform (fast) genomics analyses 

When I designed this hackathon session, I had in mind to demonstrate how fast it is possible to analyze data on the cloud. (You probably noticed that the tutorial was written with the perspective of someone who was quite impatient.) However, I was struck by a grim reality: downloading reads may be fast there, but then even aligning to a reference genome would become the next bottleneck. Some parts of the bioinformatics community have attempted to solve this problem by designing custom solutions, e.g. aligning using GPU, FPGA, or distributed CPUs on the cloud, etc.. 

What I believe is the fastest solution available is DRAGEN from Illumina, which uses a FPGA. AWS provides machines with FPGA, and even with DRAGEN pre-installed! However, this come at a cost: a high subscription fee. Therefore I sought alternative solutions. NVidia has made some efforts accelerating some genomics workflows using GPUs. They provide a cheaper AWS subscription for their tools. This is what we will use here. I hope you will find the perspective of doing ultra-fast read alignment using GPUs interesting, and possibly fun, or at least worth continuining reading.

A. Setting up a Parabricks instance

Go to https://aws.amazon.com/marketplace/pp/prodview-apbngojlskcyq
and subscribe to this "Parabricks Pipeline". It should cost around $4.2 an hour using their recommended instance (a beefy g4dn.12xlarge). Make sure you are in the correct region for this session (us-west-2).

You may then connect to the instance. Note that the username is no longer 'ec2-user' but 'ubuntu'. Upon the first connection, it will format the allocated NVME SSD drive (which should be quick).

B. Preparing the analysis

Refer to PartB to download the data to this instance. We'll get the same human genome and PCR-free Illumina reads. The instance come 'pre-packaged' with two disk volumes, one 140 GB with the system and a 825 GB entirely fresh. Let's use the latter.

    cd /mnt/disks/local
    df -h .

We'll get a pre-indexed human genome also:

    aws s3 --no-sign-request cp s3://parabricks.sample/parabricks_sample.tar.gz .
    tar xf parabricks_sample.tar.gz 
