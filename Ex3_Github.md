# Excercise 1: GitHub

#### 1. Go to the GitHub website, log in, and **create a new GitHub repository**. This will be the remote counterpart of the Git repo you made as part of last week’s assignment.

- You can give the GitHub repo any name you want.
- Like we did in class, keep the repo Public (the default), because that’s needed for the instructor tagging exercise later on.
- Also like we did in class, don’t let GitHub add a README or any other files when you create the repo.


#### 2. **Navigate** to your dir for last week’s assignment in the VS Code terminal (you can also use the “Open Folder” option in VS Code to open that folder if you want).

The GA2/ dir was copied into the EX3 dir
```bash
cp -r GA2/ EX3/
```

#### 3. Set up a **connection** between the Git repo and the GitHub counterpart using Git commands.

```bash
# Add and commit all files to repo
git add --all
git commit -am "Adding Markdown file for Ex3"

# Setting up remote connection
git remote add origin git@github.com:20ndameron/PLNTPTH_5006_Ex3.git
```

#### 4. **Push** your local repo to the online counterpart.

```bash
git push -u origin main
```

#### 5. Explore your repo on **GitHub**. Does your `README.md` render well? Can you see all your commits? Are your `data` and `results` dirs there? Why or why not?

The `README.md` file renders well and is in markdown format. This allows the code chunks to be more clear and formatted in the bash language. All of the commits are visible and can be seen by clicking on the commit section of the repo. The `data/` and `results/` subdir are not included in the remote repo because we added them to the `.gitignore` file which tells Git to ignroe these files when committing. 

#### 6. You will hand in the remaining assignments for this course by “tagging” the instructors in a GitHub Issue. To make sure you know how this works, let’s do a test run. On GitHub, open a **GitHub Issue** for your repo: click the Issues tab and then **“New Issue”**.
- Under “Add a title”, give the issue a title like “Testing whether tagging works”.

- Under “Add a description”, tag both instructors using `@<username>` syntax:
```git
Hey @jelmerp and @kstarr791, can you please take a look at my repo?
```

- Click the green **“Create”** button to post your Issue. If the tagging worked, the `@jelmerp` and `@kstarr791` should have turned into links to the instructors’ GitHub profiles.

#### 7. Back in VS Code, add a line to your `README.md` to describe that you created a GitHub repo on today’s date (include the repo’s URL in the note).

```bash
# Add empty line to README.md
echo -e "\n" >> README.md

# Add text to README.md
echo "Github repo created on 2026-10-01 (https://github.com/20ndameron/PLNTPTH_5006_Ex3)" >> README.md
```

#### 8. Commit the change to README.md to your Git repo.

```bash
# Add README.md
git add README.md

# Commit README.md
git commit -m "Updated README.md with GitHub link"

# Add Ex3_Github.md
git add Ex3_Github.md

# Commit Ex3_Github.md
git commit -m "Updated Ex3_Github.md file"
```

#### 9. Push to the remote and on GitHub, check that the changes were applied in the online repo.

```bash
git push
```

# Exercise 2: Login versus compute nodes

#### 1. In OnDemand, open a shell on a login node by clicking “Clusters” in the top bar and then **“Cardinal Shell Access”**. You should see many lines of text printed to the terminal, which is standard information that OSC prints on login (but not compute) nodes, including details about your Project’s usage of its storage quota2.

#### 2. In the Cardinal shell, run the command `hostname`, which prints the name of the node that you’re on (like echo `$HOSTNAME` that you used in GA1). Then, run the same command in your VS Code terminal, and compare the outputs. What in each node name tells you whether it is a login node or a compute node? And which cluster is each node part of?

```
# Output from `hostname` command in Cardinal shell
cardinal-login03.hpc.osc.edu

# Output from `hostname` command in VS Code terminal 
p0220.ten.osc.edu
```
A login node is differentiated by the `login03` portion of the output while a compute node is differentiated by a compute ID (`0220`). The cluster is is shown by `cardinal` in the login node and by `p` in the compute node. 

#### 3. Why does the Cardinal shell put you on a login node, whereas your VS Code session is on a compute node? And what does this mean for what you should (not) do in the Cardinal shell? For example, would it be OK to run FastQC on all FASTQ files of the Garrigós data there?

The Cardinal shell puts you on a login node because you have not requested any compute time. The login node just allows you to explore the terminal, your files, and perform basic commands. You should not run any computationally intensive commands or jobs in the login node. These should be submitted as jobs with compute nodes requested. 

#### 4. In the Cardinal shell, navigate to `/fs/ess/PAS3493/people/$USER` and list its contents. Do you see the same files as in VS Code, even though your VS Code session is running on a different cluster?

Navigating to `/fs/ess/PAS3493/people/$USER` in both the Cardinal login node and in the VS Code both show the same files. This is because you are listing your directories/files under `$USER`. Your files are accessbile in both the login and compute nodes. 

#### 4. In your VS Code terminal, load the FastQC module and check that fastqc -v works. Predict whether fastqc -v will also work in the following two scenarios, and then check your predictions and interpret the results:
- In the Cardinal shell.
- In a second VS Code terminal (click the + icon in the terminal panel or the downward arrow next to it).

```bash
# Load Fastqc Module
module spider fastqc
module load fastqc/0.12.1 

# Confirm fastqc works (will print `FastQC v0.12.1`)
fastqc -v
```
You are able to load FastQC in both the login node on the Cardinal shell and in a second terminal on VS Code. 


#### 6. In the lecture’s self-study exercise on modules, module spider salmon in VS Code said there was no Salmon module. Run the same command in the Cardinal shell. What do you get, and why?

```bash
# Try and load salmon in VS Code terminal
module spider salmon
```
```
Lmod has detected the following error:  Unable to find: "salmon".
```
```bash
# Try and load salmon in Cardinal shell
module spider salmon
```
```

--------------------------------------------------------------------------------------------------------------------------------------------------------
  salmon: salmon/1.4.0
--------------------------------------------------------------------------------------------------------------------------------------------------------
    This module can be loaded directly: module load salmon/1.4.0
    Help:
      This module loads salmon
      Configured and installed with modules:
      No modules loaded

```
When you run `module spider salmon` in the VS Code terminal, you are shown an error and are not able to load the module. However, when you run it in the Cardinal Shell, you see that you are able to load it. This is because the module is only available on the Cardinal cluster and not the Pitzer cluster. The ability to load the module has nothing to do with it being a login node vs. compute node. 

# Exercise 3: A specific software version

#### 1. Is FastQC version 0.11.9 available as an OSC module?

FastQC version 0.11.9 is not available as an OSC module

```bash
module spider fastqc
```
```
fastqc: fastqc/0.12.1
```

#### 2. Get the URI of a Seqera container that contains FastQC version 0.11.9.

```
oras://community.wave.seqera.io/library/fastqc:0.11.9--1cc469c72218f8fc
```

#### 3. Check the container’s FastQC version.

```bash 
oras://community.wave.seqera.io/library/fastqc:0.11.9--1cc469c72218f8fc
```
```
INFO:    Using cached SIF image
INFO:    gocryptfs not found, will not be able to use gocryptfs
FastQC v0.11.9
```

#### 4. Load the `fastqc/0.12.1` module. You now have access to two different versions of FastQC. Before running anything, **predict** which version `fastqc -v` will print when you run it with and without the container prefix. Then, run both commands to check.

After loading `fastqc/0.12.1` through modules, version 0.12.1 will run if you just use `fastqc` in the terminal. Version 0.11.9 will run only if you use the container prefix. 

```bash
# Load fastqc module
module load fastqc/0.12.1

# Check fastqc version
fastqc -v

# Check fastqc version with container
apptainer exec oras://community.wave.seqera.io/library/fastqc:0.11.9--1cc469c72218f8fc \
    fastqc -v
```
```
# Check fastqc version
FastQC v0.12.1

# Check fastqc version with container
FastQC v0.11.9
```