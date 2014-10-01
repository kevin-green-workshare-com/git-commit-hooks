Why do we have this?

In order to validate your commits a client commit hook must be installed on your repository. This is a Python script that it's executed each time you try to commit on your repo and it does these checks:

  * checks that the commit reference one or more fogbugz cases, looking for these formats anywhere in the commit message:
    * [123456]
    * case:123456
    * case#123456
  * checks that the cases referenced exist
  * checks that the cases areopen

If any of those checks are failing the commit will be refused. You can use the special keyword NOFOGZ anywhere in your commit to skip the control, but please do not do that (you will have to explain why)
 
How do I install it?

The prerequisite to run the hook is Python 2.5+, so please follow the instruction for your operating system to install it. You will have then to install the Fogbugz client library and then you have to copy on your repository the commit-hook script getting it from the repo.

Assuming you are sitting in the repository root folder you can execute this two commands:

```bash
wget http://-----/commit-hooks/commit-msg -O .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

You can of course copy the file manually as well :) from here (remember also to add exec right to the script):

```bash
https://github.com/workshare/qa/blob/master/commit-hooks/commit-msg
``

How do I configure it?

The script needs to access fogbugz and for that reason you will need to setup an access token before starting using the script the first time. Assuming you are sitting in the repository root folder you have to execute this command:

```bash
.git/hooks/commit-msg setup-credentials your-email@domain.com your-fogbugz-password
```bash

Note that you have to do this once as the configuration is kept on a file on your local home (no need to do this step for every repository you setup the hook against)

 
How does it work?

Ok, imagine you have your commit loaded on your repository and you decide you wan to commit something. In your commit message you have to reference the related case this commit is against, so the simplest and clean thing you can do is prepend it to your commit message

```bash
$> git commit -m "[24587] This is my first commit"

> FOGBUGZ commit hook  === c/o Workshare Ltd ===
>
> Detected cases ['24587']
> Checking cases...
>  case 24587, OPEN - Unable to download pdf (via api) for files that have restricted download permissions
>
> SUCCESS - Commit accepted!

[master 981dcab] This is my first commit [24587]
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 444
```


Troubleshooting

Q. When I commit I am getting this message:

FAILED - Temporarily unable to connect to fogbugz: Error Code 3: <![CDATA[Not logged in]]>

What should I do?

A. Your access token to fogbugz has been compromised or it's not valid anymore. Please re-execute the setup as described in the "How do I configure it?" section of this wiki page

 

Q. When I commit I am getting this message:

File ".git/hooks/commit-msg", line 119
    print '> '

      SyntaxError: invalid syntax

What should I do?

A. You are using Python3, where print statements are now functions (hooray!). To sort this out you will have to install Python2 and setup the fogbugz library using the manual install procedure.

