# nca-rest-server
A Flask-based REST-API server for Network-Config-Analyzer (NCA).

## Installation
```shell
git clone git@github.com:np-guard/nca-rest-server.git
cd nca-rest-server
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
```


## Usage
### Running the server
```shell
source venv/bin/activate
python -m nca_rest_server
```
The server can now be accessed via port 5000.

### Setting cluster namespaces
Use the `namespace_list` path to post a list of namespace resources.
For example, loading namespaces from a JSON file with a namespaces list can be done as follows.
```shell
curl -X POST -H Content-Type:application/json -d @tests/basic/ns_list.json localhost:5000/namespace_list
```

### Setting cluster pods
Use the `pod_list` path to post a list of pod resources.
For example, loading pods from a JSON file with a pod list can be done as follows.
```shell
curl -X POST -H Content-Type:application/json -d @tests/basic/pods_list.json localhost:5000/pod_list
```

### Setting cluster network policies
Use the `policy_sets` path to post a list of network policies.
Each post returns a unique identifier for the posted set of network policies.
This identifier can be used to later query NCA findings on this set.
For example, loading network policies from a JSON file with a list of network policies can be done as follows.
```shell
curl -X POST -H Content-Type:application/json -d @tests/basic/testcase2-networkpolicies.json localhost:5000/policy_sets
```
A valid response code should be 201, and the response body should include the set id, e.g., `set_0`.

A GET request from the `policy_sets` path returns the available policy sets.
Adding the policy-set identifier to the path returns details for this set. For example
```shell
curl localhost:5000/policy_sets/set_1
```

### Listing findings for a set of network policies
For a given set of network policies with set id `<set_id>`, a GET request from the path `policy_sets/<set_id>/findings`
returns a JSON response with lint findings for each one of the policies.
For example
```shell
curl localhost:5000/policy_sets/set_0/findings | jq
```
