Conectivity Checker
=========

This role will check wether there is or not connectivity issues between the localhost and the host writen in the configuration file.

Requirements
------------

The only requirements for this role is a laptop and a network connection, as well as python and ansible installed in the laptop.

Example of execution
----------------

To run the role just need to open a terminal and run the following command:

```shell
ansible-playbook connectivity.yml
```
the output of that command should be:

```shell

PLAY [check ports] ***********************************************************************************************************************************************************************************************************

TASK [cxn_checker : Check global ports] **************************************************************************************************************************************************************************************
ok: [localhost] => (item={'hostname': 'github.com'})
ok: [localhost] => (item={'hostname': 'gmail.com', 'port': 443})
ok: [localhost] => (item={'hostname': 'stocardapp.com', 'port': 443})
failed: [localhost] (item={'hostname': 'google.com', 'port': 1443}) => {"ansible_loop_var": "item", "changed": false, "elapsed": 4, "item": {"hostname": "google.com", "port": 1443}, "msg": "Timeout when waiting for google.com:1443"}
failed: [localhost] (item={'hostname': 'invalid.address'}) => {"ansible_loop_var": "item", "changed": false, "elapsed": 3, "item": {"hostname": "invalid.address"}, "msg": "Timeout when waiting for invalid.address:80"}
failed: [localhost] (item={'hostname': 'stocardapp.com', 'port': 'invalid-port'}) => {"ansible_loop_var": "item", "changed": false, "item": {"hostname": "stocardapp.com", "port": "invalid-port"}, "msg": "argument 'port' is of type <class 'str'> and we were unable to convert to int: <class 'str'> cannot be converted to an int"}
ok: [localhost] => (item={'hostname': 'klarna.com'})
ok: [localhost] => (item={'hostname': 'apple.com', 'port': 443})
ok: [localhost] => (item={'hostname': 'klarna.com', 'port': 443})
...ignoring

PLAY RECAP *******************************************************************************************************************************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
```

Another way to test the execution is with the [kitchen test](https://kitchen.ci/) enviroment from [Chef](https://docs.chef.io/workstation/kitchen/).

### Requirements
- [kitchen.ci](https://kitchen.ci/)
- [ruby](https://www.ruby-lang.org/en/documentation/installation/)
- [bundler](https://rubygems.org/gems/bundler)

### Execution.

1. Install the packages in the `Gemfile`:
```shell
bundle install
```
2. Check that the kitchen package is installed and ready to run:
```shell
kitchen list
```
that will give the following output:
```shell
Instance             Driver  Provisioner      Verifier  Transport  Last Action    Last Error
default-ubuntu-2004  Docker  AnsiblePlaybook  Busser    Ssh        <Not Created>  <None>
```
3. Create the instance where the playbook will be apply.
```shell
kitchen create
```
will bring the following output:
```shell
... previous output ommitted
[SSH] Established
Finished creating <default-ubuntu-2004> (0m3.20s).
-----> Test Kitchen is finished. (0m5.33s)
```
4. Converge the ansible playbook.
```shell
kitchen converge
```

License
-------

Apache-2.0
