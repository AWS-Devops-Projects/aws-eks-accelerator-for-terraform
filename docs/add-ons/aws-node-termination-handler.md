# AWS Node Termination Handler

This project ensures that the Kubernetes control plane responds appropriately to events that can cause your EC2 instance to become unavailable, such as EC2 maintenance events, EC2 Spot interruptions, ASG Scale-In, ASG AZ Rebalance, and EC2 Instance Termination via the API or Console. If not handled, your application code may not stop gracefully, take longer to recover full availability, or accidentally schedule work to nodes that are going down. For more information see [README.md][https://github.com/aws/aws-node-termination-handler#readme].

The aws-node-termination-handler (NTH) can operate in two different modes: Instance Metadata Service (IMDS) or the Queue Processor. In the SSP, we provision the NTH in Queue Processor mode. This means that NTH will monitor an SQS queue of events from Amazon EventBridge for ASG lifecycle events, EC2 status change events, Spot Interruption Termination Notice events, and Spot Rebalance Recommendation events. When NTH detects an instance is going down, NTH uses the Kubernetes API to cordon the node to ensure no new work is scheduled there, then drain it, removing any existing work.

The NTH will be deployed in the `kube-system` namespace. AWS resources required as part of the setup of NTH will be provisioned for you. These include:

1. Node group ASG tagged with `key=aws-node-termination-handler/managed`
2. AutoScaling Group Termination Lifecycle Hook
3. Amazon Simple Queue Service (SQS) Queue
4. Amazon EventBridge Rule
5. IAM Role for the aws-node-termination-handler Queue Processing Pods

## Usage

```hcl
aws_node_termination_handler = true
```

To validate that controller is running, ensure that controller deployment is in RUNNING state:

```sh
# Assuming controller is installed in kube-system namespace
$ kubectl get deployments -n kube-system
aws-node-termination-handler  1/1   1   1   5d9h
```
