# Lab: Connecting to Control Planes with the Upbound `up` CLI

In this Lab we'll use the `up` CLI to set up `kubectl` access to the Control Plane's API endpoint and the MCP Connector. 

## Prerequisites

Ensure that you have completed the first CLI lab and can access Upbound Cloud.

Your organization should have a Configuration available, see the lab at   [xx-lab-up-cli-configurations](../xx-lab-up-cli-configurations/xx-lab-up-cli-configurations/README.md).

`up configuration list`

You should also have a Robot and a generated Token. See **TODO Lab for Robots and Tokens**.

## Confirm your Profile

Confirm you are using the correct Upbound Organization using `up profile list` and looking for the '*' character, or running `up profile current`.

## Create a Control Plane

We'll create a Control Plane using an existing Configuration.

```console
up controlplane create --configuration-name=upbound-training upbound-cloud-training 
```

You should see the following output:

```console
upbound-cloud-training created
```

## Populate the Kubeconfig File

[Kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) files are used to configure access to Kubernetes clusters. Upbound Cloud allows you to populate your local Kubeconfig file with Control Plane authentication information.

If you don't have a robot and token, you can create one now:

```console
up robot create upbound-training-robot
```
Create the token, the usage is `up robot token create --output=token.json <robot-name> <token-name>`:

```console
up robot token create --output=upbound-training-token.json upbound-training-robot upbound-training-token   
upbound/upbound-training-robot/upbound-training-token created
```

The generated token contains `accessId` and `token` fields:

```json
{
  "accessId": "78e47a6b-feb2-4515-8e7d-a9fd8f4b9d85",
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3OGU0N2E2Yi1mZWIyLTQ1MTUtOGU3ZC1hOWZkOGY0YjlkODUiLCJzdWIiOiJyb2JvdHxiMzk1YWFjZS0yMTMwLTQ3YWYtYTczZi0wOTZlYmZkYjFkOWEifQ...."
}
```

We can extract the token into our environment by using the `jq` command, which is available via [homebrew](https://brew.sh):

```console
UPBOUND_TOKEN=$(jq --raw-output .token upbound-training-token.json)
```

## Downloading Kubeconfig Control Plane Credentials

Now that we have a token that doesn't expire, we can create a Kubeconfig entry for our controlplane:

TODO: this doesn't work, everything comes back with a 404. 

```console
 up controlplane kubeconfig get lambda-example --token=$UPBOUND_TOKEN
 ```
