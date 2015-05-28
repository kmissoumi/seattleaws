# seattleaws
####Seattle AWS Architects &amp; Engineers


#####Updates 2015-MAY-27

* I added the NACL and while there may be some updates to the routetable
assignements, the issue seems to be the security group rules.

* There wasn't enough time to troubleshoot everything and there were
 unrelated rollbacks with the ELBs and bucket, so I moved those resources
into the _removed-resources.json_ file so it'd be easier to triage.

* It can be faster to validating the template prior to deploying by running
an tempalte validation check via AWS tools. Sample command below.

> ```bash
aws cloudformation validate-template --template-url http://tlp-dead-drop.s3.amazonaws.com/cfn/awsmeetup.json
```
```json
{
    "CapabilitiesReason": "The following resource(s) require capabilities: [AWS::IAM::InstanceProfile, AWS::IAM::Role]",
    "Description": "Seattle AWS Architects & Engineers - AWS Study Group Stack",
    "Parameters": [
        {
            "NoEcho": false,
            "Description": "Name of an existing EC2 KeyPair",
            "ParameterKey": "ParamKeyName"
        },
        {
            "DefaultValue": null,
            "NoEcho": false,
            "Description": "The name of the S3 bucket to create for log shipping",
            "ParameterKey": "LogBucketName"
        },
        {
            "DefaultValue": "0.0.0.0/0",
            "NoEcho": false,
            "Description": "SSH In CIDR",
            "ParameterKey": "ParamSshCidr"
        },
        {
            "DefaultValue": "us-west-1c",
            "NoEcho": false,
            "Description": "Instance key name",
            "ParameterKey": "ParamAvailabilityZone2"
        },
        {
            "DefaultValue": "us-west-1b",
            "NoEcho": false,
            "Description": "Instance key name",
            "ParameterKey": "ParamAvailabilityZone1"
        }
    ],
    "Capabilities": [
        "CAPABILITY_IAM"
    ]
}
```

* If it's of interest, I've used cloud-init to execute configsets out of
meta-data and also send signals back to cloudformation. It's useful for
setting dependencies based on criteria other than Cloudformation status.

* Also one way I've found speeds up triage and testing is by having a seprate
file for each resource. It's easier to read the template, add/remove/update, and
when you want to validate and deploy, you just need to insert the contacts into
the resources block. you can move each resource into a seperate file and then contactate before getting ready to deploy

* I did a quick install of chef client on each type of host to test connectivity. Sample below.

>```bash
curl -L https://www.opscode.com/chef/install.sh | bash```


>```bash
[root@ip-10-0-1-57 ~]# curl -L https://www.opscode.com/chef/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 18736  100 18736    0     0  38223      0 --:--:-- --:--:-- --:--:-- 38236
Downloading Chef  for el...
downloading https://www.opscode.com/chef/metadata?v=&prerelease=false&nightlies=false&p=el&pv=6&m=x86_64
  to file /tmp/install.sh.2305/metadata.txt
trying wget...
url	https://opscode-omnibus-packages.s3.amazonaws.com/el/6/x86_64/chef-12.3.0-1.el6.x86_64.rpm
md5	c19fefcb3d033107e9fbdb3839312584
sha256	4b7c846a9ad93564cc203a5ac99890431f7d6ad159c424aa89827fd772c9881d
downloaded metadata file looks valid...
downloading https://opscode-omnibus-packages.s3.amazonaws.com/el/6/x86_64/chef-12.3.0-1.el6.x86_64.rpm
  to file /tmp/install.sh.2305/chef-12.3.0-1.el6.x86_64.rpm
trying wget...
Comparing checksum with sha256sum...
Installing Chef
installing with rpm...
warning: /tmp/install.sh.2305/chef-12.3.0-1.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 83ef826a: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:chef-12.3.0-1.el6                ################################# [100%]
Thank you for installing Chef!
[root@ip-10-0-1-57 ~]#```
