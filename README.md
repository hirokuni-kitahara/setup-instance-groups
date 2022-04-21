# Usage

1. Prepare input files
   
Prepare input files to be setup on remote nodes.
The following is an example structure of the input file directory.

```
$ tree sample_config
sample_config/
`-- etc
    |-- containers
    |   |-- policy.json
    |   `-- registries.d
    |       `-- ghcrio-hiro-ee.yaml
    `-- pki
        `-- rpm-gpg
            `-- hiro-gpg-pub
```

2. Setup files on the remote nodes

- Instance Group case ( = non K8s nodes )

```
$ ansible-playbook setup-instance-group.yml -e directory=sample_config/
```

- Container Group case ( = K8s nodes)

```
$ ansible-playbook setup-container-group.yml -e directory=sample_config/
```

3. Validate the configuration is maintained on the nodes

- Instance Group case ( = non K8s nodes )

```
$ ansible-playbook check-instance-group.yml -e directory=sample_config/
```

If there are some changed files on remote node, the error message will be shown as below.

```
TASK [fail] ***********************************************************************************************************************************
fatal: [<hostmachine>]: FAILED! => {"changed": false, "msg": "some files on remote are not identical to the corresponding local files. the differrent files are: [\"/etc/containers/policy.json\"]"}
```


- Container Group case ( = K8s nodes)

```
$ ansible-playbook check-container-group.yml -e directory=sample_config/
```

If some configurations are different from local files, the error message will be shown as below.

```
TASK [fail] ***********************************************************************************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "msg": "MachineConfig configuration on the cluster is different from the current files on local"
```
