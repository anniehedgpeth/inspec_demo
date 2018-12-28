# inspec_demo

## Prerequisites

 - InSpec is [installed](https://www.inspec.io/downloads/)

## What InSpec shows in this demo:

1. InSpec is running locally, although it can be run remotely, too, via ssh, winrm, or Chef Automate. 

2. It is validating that the node is in the state in which it is expected to be in as defined by the InSpec profile. (Anything that you can query by searching a file or running a command can be done with InSpec.)

## What to do:

Were just going to run a quick profile to show what it looks like and how quickly it tests a node. These are expected to fail, but you can see what kinds of tests are being run.
NOTE: This is not making any changes to your node.

1. From the command line of your choice, run this command from any directory.

If you're on Windows you can run:

```
inspec exec https://github.com/dev-sec/windows-baseline
```

If you're on MacOS you can run:

```
inspec exec https://github.com/dev-sec/mysql-baseline
```

When you run this command, InSpec is querying your local machine to see if it's in the desired state that is specified in the InSpec profile. It will not be, and therefore you will see many many errors, which is what you want.

Your output will likely look like the following, mostly failures with a few passing tests. This is truncated:

```Profile: DevSec MySQL Baseline (mysql-baseline)
Version: 3.0.0
Target:  local://

  ×  mysql-conf-01: ensure the service is enabled and running (2 failed)
     ×  Service  should be enabled
     undefined method `[]' for nil:NilClass
     ×  Service  should be running
     undefined method `[]' for nil:NilClass
  ×  mysql-conf-05: ensure data path is owned by mysql user (2 failed)
     ×  File /ibdata1 should be owned by "mysql"
     expected `File /ibdata1.owned_by?("mysql")` to return true, got false
     ×  File /ibdata1 should be grouped into "mysql"
     expected `File /ibdata1.grouped_into?("mysql")` to return true, got false
     ✔  File /ibdata1 should not be readable by others
     ✔  File /ibdata1 should not be writable by others
     ✔  File /ibdata1 should not be executable by others
  ↺  mysql-conf-08: ensure the mysql hardening config file is owned by root
     ↺  Skipped control due to only_if condition.
  ✔  mysql-conf-09: the use of MYSQL_PWD is insecure and can exploit the password
     ✔  Command: `env` stdout should not match /^MYSQL_PWD=/
  ✔  mysql-db-01: use supported mysql version in production
     ✔  Command: `mysql -uroot -piloverandompasswordsbutthiswilldo mysql -s -e 'select version();' | tail -1` stdout should not match /Community/
  ×  mysql-db-02: use mysql version 5 or higher
     ×  Command: `mysql -uroot -piloverandompasswordsbutthiswilldo mysql -s -e 'select substring(version(),1,1);' | tail -1` stdout should match /^5/
     expected "" to match /^5/
     Diff:
     @@ -1,2 +1,2 @@
     -/^5/
     +""

  ✔  mysql-db-03: test database must be deleted
     ✔  Command: `mysql -uroot -piloverandompasswordsbutthiswilldo -s -e 'show databases like "test";'` stdout should not match /test/
  ×  mysql-db-04: deactivate annonymous user names
     ×  Command: `mysql -uroot -piloverandompasswordsbutthiswilldo mysql -s -e 'select count(*) from mysql.user where user="";' | tail -1` stdout should match /^0/
     expected "" to match /^0/
     Diff:
     @@ -1,2 +1,2 @@
     -/^0/
     +""

Profile Summary: 3 successful controls, 13 control failures, 1 control skipped
Test Summary: 8 successful, 31 failures, 1 skipped
```