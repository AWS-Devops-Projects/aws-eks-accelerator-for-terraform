# AWS Load Balancer Controller

The [AWS Load Balancer Controller](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html) manages AWS Elastic Load Balancers for a Kubernetes cluster. The controller provisions the following resources:

* An AWS Application Load Balancer (ALB) when you create a Kubernetes Ingress.
* An AWS Network Load Balancer (NLB) when you create a Kubernetes Service of type LoadBalancer.

For more information about AWS Load Balancer Controller please see the [official documentation](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html).

## Usage

```hcl
aws_lb_ingress_controller_enable = true
```

To validate that controller is running, ensure that controller deployment is in RUNNING state:

```sh
# Assuming controller is installed in kube-system namespace
$ kubectl get deployments -n kube-system
NAME                                                       READY   UP-TO-DATE   AVAILABLE   AGE
aws-load-balancer-controller                               2/2     2            2           3m58s
```
#### AWS Service annotations for LB Ingress Controller

Here is the link to get the AWS ELB [service annotations](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/) for LB Ingress controller.

### GitOps Configuration

The following properties are made available for use when managing the add-on via GitOps.

```
awsLoadBalancerController = {
  enable             = true
  serviceAccountName = "<service_account_name>"
}
```
