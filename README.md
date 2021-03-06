# snakemake-mothur

Example of a snakemake workflow implementation of the mothur MiSeq SOP.
Please see http://www.mothur.org/wiki/MiSeq_SOP

This workflow has some errors in the last several commands. I ran into an issue with remove.groups() where it claimed to create a file that was never made. Unfortunately I did not have time to correct these problems.  
Everything up to remove.groups was tested and ran correctly on the AAFC biocluster with both DRMAA and qsub.


## Table of Contents
  * [Workflow](#workflow)
  * [Requirements](#reqs)
  * [Installation/Setup](#install)
  * [Running](#running)
  * [Additional Information](#info)
  * [See also](#seealso)
  * [Caveats](#caveats)
 
## <a name="workflow"></a>Workflow

Here is an image of the current workflow implemented in the Snakefile. This image was generated using snakemake.

![mothur workflow](dag.final.png?raw=true "Mothur Workflow")

## <a name="reqs"></a>Requirements

- Python 3.3+
- snakemake 3.5+

I used Python 3.5.1 and Snakemake 3.6.0

## <a name="install"></a>Install Instructions

Install Python and snakemake with sudo priviledges
```
> sudo apt-get install python3
> sudo apt-get install python3-pip  # may be installed with Python 3.4+
> pip3 install snakemake
```

#### If you don't have sudo priviledges, it's still possible but a bit more involved

1) Download the Python source
```
> wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tgz
> gunzip Python-3.5.1.tgz
> tar -xf Python-3.5.1.tar
```

2) Compile it to a local directory and add it to your path
```
> cd Python-3.5.1
> ./configure --prefix=/home/<your username>/Python35
> make && make altinstall
> export PATH=/home/<username>/Python35/bin:$PATH 
```

__Notes__: 
  - `altinstall` does not make the "python" executable -- only "python3.5". This is so that you don't covert your default python interpreter which may be needed by system processes. 
  - Using `export` in the shell will effect this session. Add the command to your .bash_profile to make it permanent

3) Make a virtualenv and install snakemake
```
> mkdir workdir
> cd workdir
> python3.5 -m venv smenv
> source env/bin/activate
> pip3 install snakemake
```

## Getting this repo

Download a copy of this repo manually, or with `git`  
`> git clone https://github.com/tomsitter/snakemake-mothur`

## Getting the test data

Data files are excluded from this repository because many files they are too large for GitHub. Links to the test data can be found in the MiSeq SOP wiki linked at the top of this page.
  
All files should be placed in the data/ folder of this repository. 

```
> cd snakemake-mothur/data
> wget http://www.mothur.org/w/images/d/d6/MiSeqSOPData.zip
> wget http://www.mothur.org/w/images/9/98/Silva.bacteria.zip
> wget http://www.mothur.org/w/images/5/59/Trainset9_032012.pds.zip

> unzip MiSeqSOPData.zip
> unzip Silva.bacteria.zip
> unzip Trainset9_032012.pds.zip
```

Mothur wants to look in the working directory for the sequences, so copy everything from the folder here
`> cp MiSeq_SOP/* .`

This gives me the following files
```
.
├── cluster.json
├── commands.txt
├── config.json
├── dag.final.png
├── data
│   ├── F3D0_S188_L001_R1_001.fastq
│   ├── F3D0_S188_L001_R2_001.fastq
│   ├── F3D141_S207_L001_R1_001.fastq
│   ├── F3D141_S207_L001_R2_001.fastq
│   ├── F3D142_S208_L001_R1_001.fastq
│   ├── F3D142_S208_L001_R2_001.fastq
│   ├── F3D143_S209_L001_R1_001.fastq
│   ├── F3D143_S209_L001_R2_001.fastq
│   ├── F3D144_S210_L001_R1_001.fastq
│   ├── F3D144_S210_L001_R2_001.fastq
│   ├── F3D145_S211_L001_R1_001.fastq
│   ├── F3D145_S211_L001_R2_001.fastq
│   ├── F3D146_S212_L001_R1_001.fastq
│   ├── F3D146_S212_L001_R2_001.fastq
│   ├── F3D147_S213_L001_R1_001.fastq
│   ├── F3D147_S213_L001_R2_001.fastq
│   ├── F3D148_S214_L001_R1_001.fastq
│   ├── F3D148_S214_L001_R2_001.fastq
│   ├── F3D149_S215_L001_R1_001.fastq
│   ├── F3D149_S215_L001_R2_001.fastq
│   ├── F3D150_S216_L001_R1_001.fastq
│   ├── F3D150_S216_L001_R2_001.fastq
│   ├── F3D1_S189_L001_R1_001.fastq
│   ├── F3D1_S189_L001_R2_001.fastq
│   ├── F3D2_S190_L001_R1_001.fastq
│   ├── F3D2_S190_L001_R2_001.fastq
│   ├── F3D3_S191_L001_R1_001.fastq
│   ├── F3D3_S191_L001_R2_001.fastq
│   ├── F3D5_S193_L001_R1_001.fastq
│   ├── F3D5_S193_L001_R2_001.fastq
│   ├── F3D6_S194_L001_R1_001.fastq
│   ├── F3D6_S194_L001_R2_001.fastq
│   ├── F3D7_S195_L001_R1_001.fastq
│   ├── F3D7_S195_L001_R2_001.fastq
│   ├── F3D8_S196_L001_R1_001.fastq
│   ├── F3D8_S196_L001_R2_001.fastq
│   ├── F3D9_S197_L001_R1_001.fastq
│   ├── F3D9_S197_L001_R2_001.fastq
│   ├── HMP_MOCK.v35.fasta
│   ├── Mock_S280_L001_R1_001.fastq
│   ├── Mock_S280_L001_R2_001.fastq
│   ├── silva.bacteria
│   │   ├── silva.bacteria.fasta
│   │   ├── silva.bacteria.gg.tax
│   │   ├── silva.bacteria.ncbi.tax
│   │   ├── silva.bacteria.pcr.8mer
│   │   ├── silva.bacteria.pcr.fasta
│   │   ├── silva.bacteria.rdp6.tax
│   │   ├── silva.bacteria.rdp.tax
│   │   ├── silva.bacteria.silva.tax
│   │   └── silva.gold.ng.fasta
│   ├── stability.batch
│   ├── stability.files
│   ├── trainset9_032012.pds.fasta
│   └── trainset9_032012.pds.tax
├── preprocess.mothur2.Snakefile
├── preprocess.mothur.Snakefile
├── README.md
└── Snakefile
```

## <a name="running"></a>Running

Please read the [official snakemake tutorial and documentation](https://bitbucket.org/snakemake/snakemake/wiki/Home)

This guide assumes you have read them.

Navigate to the directory containing the Snakefile

To see what commands will be run (and check your syntax), perform a dry-run

`snakemake -npr`

__Note__: If you are using the run: directive in any of your rules (opposed to the shell: directive), you will be missing the actually command in this printout because it is not interpreted until runtime.

You can run this pipeline locally by just typing `snakemake`

Or, you can submit it to the cluster using DRMAA or just qsub. With qsub:

`snakemake --cluster "qsub -S /bin/bash" --jobs 32`

I found I needed to add "-S /bin/bash" in order to initialize the PATH to run the command sI wanted

Alternatively, you can run using DRMAA.

### DRMAA

Python 3.5 requires an environmental variable DRMAA_LIBRARY_PATH on the head node (the node running snakemake). I used

`> export DRMAA_LIBRARY_PATH=$SGE_ROOT/lib/linux-x64/libdrmaa.so`

In the following example we are using drmaa to run jobs on up to 32 nodes of the cluster. In order to get a shell, I had to pass some additional parameters through to qsub:

`snakemake -j32 --drmaa ' -b n -S /bin/bash'`  

**Note the leading whitespace in the parameters string! This is essential!**  
The above command tells qsub that these are not binary commands, and that I want the /bin/bash shell

You can pass additional parameters to qsub such as the number of slots/threads, where {threads} is defined in each snakemake rule.

`snakemake -j32 --drmaa ' -n {threads} -b n -S /bin/bash'`

snakemake can automatically figure out which rules can be run in parallel, and will schedule them at the same time (if -j is > 1).

The following is a qstat taken directly after running the above command. As you can see snakemake submitted two jobs in parallel (make_contigs and pcr -- see the graph at the top of this file).

```
job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID 
-----------------------------------------------------------------------------------------------------------------
 177228 0.00000 snakejob.m bctraining03 qw    03/23/2016 15:06:22                                    8        
 177229 0.00000 snakejob.p bctraining03 qw    03/23/2016 15:06:23                                    8        
```

### Cluster Configuration

Snakemake allows you to create a JSON configuration file for running files on a cluster. See the [documentation for cluster configuration](https://bitbucket.org/snakemake/snakemake/wiki/Documentation#markdown-header-cluster-configuration)

## <a name="info"></a>Additional Information

__Specifying Queue__

You can also specify which queue each job should run in in the snakemake rule. This can be useful if you have tasks which need certain cluster requirements.

```
rule example_rule:
  input: somefile.fasta
  output: someotherfile.result
  params: queue=all.q
  shell: "cmd {input} > {output}"
```

`> snakemake -j --drmaa ' -q {params.queue} -b n -S /bin/bash'`

__Parsing Intermediate Results__

Instead of shell commands, you can run a python script. I used this in one example to parse a file from the previous rule and pass the contents into the following rule.

```python
import os
from collections import namedtuple 

Summary = namedtuple('Summary', ['start', 'end'])

def parse_summary(wildcards):
    filename = "{dataset}.summary.txt".format(dataset=dataset)
    if os.path.isfile(filename):
        with open(filename) as f:
            for line in f:
                if line.startswith("Median"):
                    _, start, end, *extra = line.split('\t')
                    return Summary(start=start, end=end)
    return Summary(start=0, end=0)

rule screen2:
    version: "1.36.1"
    input:
        fasta = "{dataset}.trim.contigs.good.unique.align".format(dataset=dataset),
        count = "{dataset}.trim.contigs.good.count_table".format(dataset=dataset),
        summary = "{dataset}.trim.contigs.good.unique.summary".format(dataset=dataset),
    output:
        "{dataset}.trim.contigs.good.unique.good.align".format(dataset=dataset),
        "{dataset}.trim.contigs.good.good.count_table".format(dataset=dataset),
    run:
        summary = parse_summary(input.summary)
        cmd = "mothur \"#screen.seqs(fasta={},count={},summary={},start={},end={},maxhomop={})\"".format(
               input.fasta, input.count, input.summary, summary.start, summary.end, config["maxhomop"])
        os.system(cmd)
```

__Creating Workflow Diagrams__

To automatically generate an image like the one at the top of this file, you can run:

`> snakemake --dag | dot -Tpdf > dag.pdf`

__Things to look into I didn't have time for__

There are a number of snakemake command line flags that aren't adequately documented but may be very useful. They are:

```
--resources [NAME=INT [NAME=INT ...]], --res [NAME=INT [NAME=INT ...]]
                        Define additional resources that shall constrain the
                        scheduling analogously to threads (see above). A
                        resource is defined as a name and an integer value.
                        E.g. --resources gpu=1. Rules can use resources by
                        defining the resource keyword, e.g. resources: gpu=1.
                        If now two rules require 1 of the resource 'gpu' they
                        won't be run in parallel by the scheduler.
                        
--immediate-submit, --is
                        Immediately submit all jobs to the cluster instead of
                        waiting for present input files. This will fail,
                        unless you make the cluster aware of job dependencies,
                        e.g. via: $ snakemake --cluster 'sbatch --dependency
                        {dependencies}. Assuming that your submit script (here
                        sbatch) outputs the generated job id to the first
                        stdout line, {dependencies} will be filled with space
                        separated job ids this job depends on.
--jobscript SCRIPT, --js SCRIPT
                        Provide a custom job script for submission to the
                        cluster. The default script resides as 'jobscript.sh'
                        in the installation directory.
```

# <a name="seealso"></a>See Also

* [Snakemake Homepage](https://bitbucket.org/snakemake/snakemake/wiki/Home)
* [Snakemake Tutorial](http://snakemake.bitbucket.org/snakemake-tutorial.html)
* [Snakemake documentation](https://bitbucket.org/snakemake/snakemake/wiki/Documentation)
* [Snakemake Google Group](https://groups.google.com/forum/#!forum/snakemake)


# <a name="caveats"></a>Caveats

This repo demonstrates a very minimal amount of the language and features of snakemake.

One large omittance is how you can specify filenames for input into the pipeline. Please see the tutorial and documentation for various other (more powerful and flexible) methods for this.

There are also many more directives that can help you track rules and specify how they work (e.g. logging, benchmarking, resource usage)
