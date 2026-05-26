# Linux Project 1: User & Permission Management (Mini Company Setup)

## Objective

Build a mini company environment in Linux to learn:

-   User creation
-   Group management
-   File ownership
-   Permissions (`chmod`, `chown`)
-   Access control
-   Debugging `Permission denied`
-   Multi-group access

------------------------------------------------------------------------

## Scenario

Company teams:

-   Developers → `dev1`, `dev2`
-   Testers → `test1`, `test2`
-   Manager → `manager1`

Folders:

-   `/tmp/company/developers`
-   `/tmp/company/testers`
-   `/tmp/company/shared`

Rules:

-   Developers access only developers + shared
-   Testers access only testers + shared
-   Manager accesses all folders
-   Unauthorized users denied

------------------------------------------------------------------------

## Step 1: Create Groups

``` bash
sudo groupadd developers
sudo groupadd testers
```

Verify:

``` bash
grep developers /etc/group
grep testers /etc/group
```

------------------------------------------------------------------------

## Step 2: Create Users

``` bash
sudo useradd -m dev1
sudo useradd -m dev2
sudo useradd -m test1
sudo useradd -m test2
sudo useradd -m manager1
```

Set passwords:

``` bash
sudo passwd dev1
sudo passwd test1
sudo passwd manager1
```

------------------------------------------------------------------------

## Step 3: Add Users to Groups

``` bash
sudo usermod -aG developers dev1
sudo usermod -aG developers dev2

sudo usermod -aG testers test1
sudo usermod -aG testers test2

sudo usermod -aG developers manager1
sudo usermod -aG testers manager1
```

Verify:

``` bash
groups manager1
```

Expected:

``` text
manager1 : manager1 developers testers
```

------------------------------------------------------------------------

## Step 4: Create Project Directories

``` bash
sudo mkdir -p /tmp/company/developers
sudo mkdir -p /tmp/company/testers
sudo mkdir -p /tmp/company/shared
```

`-p` creates parent directories automatically.

------------------------------------------------------------------------

## Step 5: Assign Ownership

``` bash
sudo chown :developers /tmp/company/developers
sudo chown :testers /tmp/company/testers
```

Verify:

``` bash
ls -ld /tmp/company/*
```

------------------------------------------------------------------------

## Step 6: Set Permissions

``` bash
sudo chmod 770 /tmp/company/developers
sudo chmod 770 /tmp/company/testers
sudo chmod 777 /tmp/company/shared
```

Meaning of `770`:

-   Owner → rwx
-   Group → rwx
-   Others → no access

------------------------------------------------------------------------

## Step 7: Test Access

Switch users:

``` bash
su - dev1
touch /tmp/company/developers/file.txt
```

Should work.

Try:

``` bash
touch /tmp/company/testers/file.txt
```

Expected:

``` text
Permission denied
```

Manager test:

``` bash
su - manager1
touch /tmp/company/developers/file.txt
touch /tmp/company/testers/file.txt
```

Should work because manager belongs to both groups.

------------------------------------------------------------------------

## Common Mistakes & Fixes

### Error:

``` text
chown: invalid user: developers
```

Fix:

Wrong:

``` bash
sudo chown developers folder
```

Correct:

``` bash
sudo chown :developers folder
```

------------------------------------------------------------------------

### Mistake:

`chmod 777`

Problem:

Everyone gets access.

Fix:

``` bash
chmod 770 folder
```

------------------------------------------------------------------------

## Debug Commands

``` bash
groups username
id username
ls -ld folder
```

Use these first for permission issues.

------------------------------------------------------------------------

## Skills Learned

-   Linux users
-   Linux groups
-   chmod
-   chown
-   ownership
-   permissions
-   access control
-   troubleshooting
-   multi-group management

------------------------------------------------------------------------

## Outcome

Built a mini Linux company access-control system with role-based
permissions.

This project helps build foundations for:

-   Linux Administrator
-   System Administrator
-   DevOps Engineer
