# chef-api-request

[OpenFaaS](https://github.com/openfaas) container function to query a Chef organisation using a supplied role name and returns the hosts to which the role is assigned.

Assumes a private key is required to work with the Chef server in use, and that the associated user account used to query Chef is called `openfaas`. 

Create a Chef private key as follows:

```bash
$ chef-server-ctl user-create openfaas Open FaaS <your email address> <chosen password> -f openfaas.pem
```

Also add the user to any Chef organisations you wish to query, e.g.:

```bash
$ chef-server-ctl org-user-add <Chef org name> openfaas --admin
```

Place the key in the `key` directory.

#### Function Usage

```bash
chef-api-request: Queries a Chef organisation using a supplied role name and returns the hosts to which the role is assigned.

Usage [via curl]: curl -s -X POST http://[<OpenFaaS Host>]:8080/function/chef-api-request -d "[<arguments>]"

Arguments:
chef role name     The role name to query for within Chef, e.g. my_app_pipeline
environment        The chef environment to be searched, e.g. DEV_STAGING for testing, SITE_A and SITE_B for production.
chef organization  The chef organization to be searched, either chef_dev or chef_prod.
```

#### Example

```bash
$ curl -s -X POST http://127.0.0.1:8080/function/chef-api-request -d ""my_app_pipeline" "DEV_STAGING" "chef_dev""
devsconsvc01.staging.dev.press.net
devsconsvc02.staging.dev.press.net
```
