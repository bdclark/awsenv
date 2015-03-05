#awsenv
Simple python script to simplify switching AWS related environment variables
between multiple profiles. Parses aws-cli config file and maps related settings
to environment variables for use with other software (e.g. Vagrant, kitchen-ec2,
etc.).


##Installation
The following assumes BASH. Copy the script somewhere in your path, then create a
function in .bashrc, .bash_profile, etc.:

```
$ wget https://raw.githubusercontent.com/bdclark/awsenv/master/awsenv -O ~/bin/awsenv
$ chmod +x ~/bin/awsenv
$ echo 'setaws() { [[ $# -gt 0 ]] && eval "$(~/bin/awsenv $@)"; }' >> ~/.bashrc
```
You will need to restart your shell or re-source your .bashrc.

Make sure you have an aws config file. See the
[AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
guide for details. The script will look for the file in `ENV['AWS_CONFIG_FILE']`
if set, otherwise it looks for `~/.aws/config` and `~/.aws/credentials`.

##Usage
To set a particular profile, simply call the `setaws` function configured
earlier:
```
$ setaws myprofile
```
where in this example `myprofile` maps to the `[profile myprofile]` block
in ~/.aws/config and/or the `[myprofile]` block in ~/.aws/credentials.

To see what environment variables are set and which profile block(s) they
match-up with, call `awsenv` script with no arguments:
```
$ awsenv

Current AWS Environment Variables:

AWS_SECRET_KEY matches ['myprofile']
AWS_SSH_KEY_PATH not set
AWS_DEFAULT_REGION matches ['myprofile', 'myotherprofile']
AWS_ACCESS_KEY matches ['myprofile']
AWS_SECRET_ACCESS_KEY matches ['myprofile']
AWS_ACCESS_KEY_ID matches ['myprofile']
AWS_SSH_KEY_ID not set
AWS_VAGRANT_SUBNET_ID not set
AWS_VAGRANT_SECURITY_GROUP not set
```

To unset your AWS environment variables in the current session, call the
`setaws` function with the `--unset` argument:
```
$ setaws --unset
```

##Additional
You'll notice in the example above some additional settings / environment
variables that don't exactly jive with aws-cli documentation.  I've added a few
extra settings to my `~/.aws/config` file and to this script to
make it easier to work with other tools such as Vagrant, Chef, Test-Kitchen, etc.
The aws-cli program doesn't seem to mind the extra settings in the config file.

##Mappings
~/.aws/config,  ~/.aws/credentials | Environment Variables
---------------------------|----------------
aws_access_key_id          | AWS_ACCESS_KEY_ID, AWS_ACCESS_KEY
aws_secret_access_key      | AWS_SECRET_ACCESS_KEY, AWS_SECRET_KEY
region                     | AWS_DEFAULT_REGION
aws_ssh_key_id             | AWS_SSH_KEY_ID
aws_ssh_key_path           | AWS_SSH_KEY_PATH
aws_vagrant_subnet_id      | AWS_VAGRANT_SUBNET_ID
aws_vagrant_security_group | AWS_VAGRANT_SECURITY_GROUP

##License
```
Copyright © 2014 Brian Clark <brian@clark.zone>
This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See http://www.wtfpl.net/ for more details.
```
