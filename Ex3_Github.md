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

# Commit Ex3_Github.md
```

#### 9. Push to the remote and on GitHub, check that the changes were applied in the online repo.
