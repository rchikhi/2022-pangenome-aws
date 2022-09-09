##  Perform (fast) genomics analyses 

When I designed this hackathon session, I had in mind to demonstrate that it is possible to analyze data quickly on the cloud. (You probably noticed that the tutorial was written with the perspective of a really impatient user.) However, I was struck by a grim reality: downloading reads may be fast now, but then even aligning to a reference genome would become the next bottleneck. Some parts of the bioinformatics community have attempted to solve this problem by designing custom solutions, e.g. aligning using GPU, FPGA, or distributed CPUs on the cloud, etc.. 

What I believe is the fastest solution available is DRAGEN from Illumina, which uses a FPGA. AWS provides machines with FPGA, and even with DRAGEN pre-installed! However, this come at a cost: [a high subscription fee](https://aws.amazon.com/marketplace/pp/prodview-ypz2tpzy6f5xq). This is great but a bit expensive for a 30-participants hackathon. Therefore I sought alternative solutions. NVidia has made some efforts accelerating [many genomics workflows](https://docs.nvidia.com/clara/parabricks/v3.5/text/software_overview.html#software-tools-overview) using GPUs, in particular variant calling from raw reads, which they claim is [30x-60x faster](https://www.nvidia.com/en-us/clara/genomics/). Now with cloud resource anyone can put that claim to the test without owning a GPU. Nvidia provides a relatively cheap ($0.3/hour) AWS subscription for their tools. This is what we will use here. I hope you will find the perspective of doing fast human variant calling using GPUs interesting, and possibly fun, or at least worth continuining reading.

### A. Setting up a Parabricks instance

Go to https://aws.amazon.com/marketplace/pp/prodview-apbngojlskcyq
and subscribe to this "Parabricks Pipeline". It should cost around $4.2 an hour using their recommended instance (a beefy g4dn.12xlarge). Make sure you are in the correct region for this session (us-west-2), and give it a unique name so you can later find it in the EC2 console. (Launching through EC2 enables to give a unique name, not with 'launch from website').

You may then connect to the instance. Note that the username is no longer 'ec2-user' but 'ubuntu'. Upon the first connection, it will format the allocated NVME SSD drive (which should be quick).

Note that you apparently don't need to 'unsubscribe' at the end of our session. Deleting the instance will be enough (or at least, I hope).

### B. Preparing the analysis

Refer to PartB to download the data to this new instance. (You will have deleted the previous instance, that's fine, the data is quick to download anyway.) We'll get the same human genome and PCR-free Illumina reads. The instance come 'pre-packaged' with two disk volumes, one 140 GB with the system and a 825 GB entirely fresh. Let's use the latter.

    cd /mnt/disks/local
    df -h .

We'll get a pre-indexed human genome also:

    aws s3 --no-sign-request cp s3://parabricks.sample/parabricks_sample.tar.gz .
    tar xf parabricks_sample.tar.gz 

### C. Aligning reads

Let's first try with bwa mem, to get a sense of speed.

    \time bwa mem -t48 parabricks_sample/Ref/Homo_sapiens_assembly38.fasta CHM13_prep5_S13_L002_R1_001.fastq.gz CHM13_prep5_S13_L002_R2_001.fastq.gz > bwamem.sam
    
You may stop it after you get an estimation of its total running time, extrapolating from:

    [M::mem_process_seqs] Processed 3200000 reads in XXXX CPU sec, XXXX real sec

We are now ready to use a GPU aligner:

    pbrun fq2bam --ref parabricks_sample/Ref/Homo_sapiens_assembly38.fasta \
                 --in-fq CHM13_prep5_S13_L002_R2_001.fastq.gz CHM13_prep5_S13_L002_R2_001.fastq.gz \
                 --out-bam output.bam
                 
How long did it take? Some food for hackathon thoughts: How much faster is it than bwa mem or minimap2? How close are we to real-time precision medicine?

The next steps, in a proper bioinformatics pipeline, would be for example to run variant calling. Adapt this command line to your case:

    pbrun germline --ref Ref.fa --in-fq sample_1.fq.gz sample_2.fq.gz --out-bam output.bam --out-recal-file recal.txt --out-variants result.vcf

### D. Hackathon part

We finally get to the end of this tutorial. You may now use the remaining time to use the instance as you want, taking advantage of a dedicated 48-cores 4-GPU machine you're root on to try the speed of your favorite software. 

After you are done, move to Part D to cleanup the instance.
