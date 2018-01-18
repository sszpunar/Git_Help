# National Audubon Society GitHub

```https://github.com/audubongit```

## New Repository Setup

```
echo "# GitBash_Test_002" >> README.md (without quotes)
git init
git add README.md
git commit -m "first commit"
git remote add origin <repository_endpoint> (i.e. git@github.com:sszpunar/GitBash_Test_002.git)
git push -u origin master
```

## Existing Repository Push

```
git remote add origin <repository_endpoint> (i.e. git@github.com:sszpunar/GitBash_Test_002.git)
git push -u origin master
```

## Clone Existing Repository

```git clone <repository_endpoint>```

## Create New Repository Branch

```
git checkout -b <branch_name>
git add -A
git commit -am "<commit description>" (i.e. "initial commit")
```

* Branches should be named in the following convention: TLA-1234/featureDescription
* "TLA_1234" is the JIRA ticket number
* "featureDescription" is the brief, camel-cased description of the changes being made

## Branch Updates

```
git checkout <repository_branch> (i.e. TLA-1234/featureDescription)
git pull origin develop
```

# Windows Console Commands

## Basic Windows Commands

```
dir
cd <directory_path>
cd..
```

## Git Commands

```
git status
```