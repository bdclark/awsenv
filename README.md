#awsenv
Simple python script to simplify switching AWS related environment variables
between multiple profiles. Parses aws-cli config file and maps related settings
to environment variables for use with other software (e.g. Vagrant, kitchen-ec2,
etc.).


##Installation
The following assumes BASH. Copy the script somewhere and create a function in
.bashrc:

```
$ wget https://raw.githubusercontent.com/bdclark/awsenv/master/awsenv -O ~/bin/awsenv
$ chmod +x ~/bin/awsenv
$ echo 'setaws() { [[ $# -eq 1 ]] && eval "$(~/bin/awsenv -p $1)"; [[ $# -eq 2 ]] && eval "$(~/bin/awsenv -p $1 -c $2)"; }' >> ~/.bashrc
$ echo 'unsetaws() { eval "$(~/bin/awsenv --unset)"; }'  >> ~/.bashrc
```
You will need to restart your shell or re-source your .bashrc.

Make sure you have an aws config file. See the
[aws-cli repo on GitHub](https://github.com/aws/aws-cli) for details. The script
will look for the file in `ENV['AWS_CONFIG_FILE']` if set, otherwise it defaults
to `~/.aws/config`.  A specific path can be specified if necessary (see options
below).

##Usage
To set a particular profile, simply call the `setaws` function in your .bashrc:
```
$ setaws client1-prod
```
where in this example "client1-prod" maps to the `[profile client1-prod]` block
in ~/.aws/config.

To see what environment variables are set and which profile block(s) they match-up
with, call `setaws` with no arguments:
```
$ setaws

Current AWS Environment Variables:

AWS_SECRET_KEY matches ['client1-prod']
AWS_SSH_KEY_PATH not set
REGION matches ['jdoe-personal', 'client1-prod']
AWS_ACCESS_KEY matches ['client1-prod']
AWS_SECRET_ACCESS_KEY matches ['client1-prod']
AWS_ACCESS_KEY_ID matches ['client1-prod']
AWS_SSH_KEY_ID not set
AWS_VAGRANT_SUBNET_ID not set
AWS_VAGRANT_SECURITY_GROUP not set
```

To unset your AWS environment variables in the current session, call the
`unsetaws` function from your .bashrc:
```
$ unsetaws
```

All available options:
```
$ awsenv --help
usage: awsenv [-h] [-p PROFILE] [-c PATH] [-u]

optional arguments:
  -h, --help            show this help message and exit
  -p PROFILE, --profile PROFILE
                        AWS profile name to set
  -c PATH, --config-path PATH
                        AWS config (default: ENV['AWS_CONFIG_FILE'] or
                        ~/.aws/config)
  -u, --unset           unset AWS related ENV variables
```

##Additional
You'll notice in the example above some additional settings / environment
variables that don't exactly jive with aws-cli documentation.  I've added a few
extra settings to my `~/.aws/config` file as well as this script in order to
make it easier to work with other tools such as Vagrant, Kitchen-EC2, Chef
knife-ec2, etc.  The aws-cli doesn't seem to mind the extra settings.

Also, the script duplicates `AWS_ACCESS_KEY_ID` to `AWS_ACCESS_KEY` and
`AWS_SECRET_ACCESS_KEY` to `AWS_SECRET_KEY` to make it more compatible with other
external tools.

##License
```
Copyright © 2014 Brian Clark <brian@clark.zone>
This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See http://www.wtfpl.net/ for more details.
```
