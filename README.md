What is Kyverno?

Kyverno is an open-source policy management solution for Kubernetes. It provides a policy engine designed to enforce custom policies in a Kubernetes cluster. 
With Kyverno, you can define policies that ensure resource configurations comply with your organization's requirements, security best practices, and other constraints.

**Key features of Kyverno include:**

Policy-Based Management: Kyverno allows you to define policies in a declarative manner, specifying rules that describe how resources should be configured.

Dynamic Policy Evaluation: Policies are evaluated dynamically as resources are created or updated in the cluster. Kyverno can validate and mutate resources in real-time.

Admission Controller Integration: Kyverno integrates with the Kubernetes admission control mechanism, allowing it to intercept resource creation and updates
to enforce policies before they are persisted in the cluster, etc.

**Common use cases include enforcing security policies, resource quotas, naming conventions, and other operational best practices.

Install Using Helm
Add helm repo for kyverno

      helm repo add kyverno https://kyverno.github.io/kyverno/
      helm repo update

Install kyverno in HA mode

     helm install kyverno kyverno/kyverno -n kyverno --create-namespace --set replicaCount=3

 then apply the following command to create the policy on the cluster for resource requests and limits
  
      kubectl apply -f kyverno-enforcepolicy-resource-request-limit.yml

then now try to deploy the deployment which actually tries to deploy pods

        kubectl apply -f sample-nginx-deploy.yml

now you shoule be able to see the error message says "validation error: CPU and memory resource requests
    and limits are required"

    and if you want to  Verify that the Kyverno policy is applied to the cluster. You can check this using the following command:

        kubectl get clusterpolicies
  
When you try to create Pods, check the events associated with the Pod, especially any Kyverno-related events. 
You can use the following command to get events for a Pod:

		    kubectl describe pod <pod-name>

Look for events indicating that the Kyverno policy was evaluated and if there were any failures.


Kyverno Controller Logs:

Check the logs of the Kyverno controller pods for any errors or issues. You can retrieve the logs using the following command:

          kubectl logs -l app.kubernetes.io/name=kyverno -n kyverno
