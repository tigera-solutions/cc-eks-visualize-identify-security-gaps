# Module 4 - Enforcing security policy to stop C&C attacks

Calico provides methods to enable fine-grained access controls between microservices and external databases, cloud services, APIs, and other applications.

Using the `Example Voting Application` previously installed, we will create and test network security policies with different ingress and egress rules to demonstrate how you can enforce security policy to stop command and control and other attacks.

<img width="400" alt="example vote application" src="https://github.com/tigera-solutions/cc-eks-compliance/assets/104035488/142ea62b-be3e-4c37-b39c-b501e1834f89">

## Security Policies

Calico Security Policies provide a richer set of policy capabilities than the native Kubernetes network policies, including:  

- Policies that can be applied to any endpoint: pods/containers, VMs, and/or to host interfaces
- Policies that can define rules that apply to ingress, egress, or both
- Policy rules support:
  - Actions: allow, deny, log, pass
  - Source and destination match criteria:
    - Ports: numbered, ports in a range, and Kubernetes named ports
    - Protocols: TCP, UDP, ICMP, SCTP, UDPlite, ICMPv6, protocol numbers (1-255)
    - HTTP attributes (if using Istio service mesh)
    - ICMP attributes
    - IP version (IPv4, IPv6)
    - IP or CIDR
    - Endpoint selectors (using label expression to select pods, VMs, host interfaces, and/or network sets)
    - Namespace selectors
    - Service account selectors

### The Zero Trust approach

A global default deny policy ensures that unwanted traffic (ingress and egress) is denied by default. Pods without policy (or incorrect policy) are not allowed traffic until the appropriate network policy is defined. Although the staging policy tool will help you find the incorrect or the missing policy, a global deny policy helps mitigate other lateral malicious attacks.

By default, all traffic is allowed between the pods in a cluster. Let's start by testing connectivity between application components and across application stacks. All of these tests should succeed as there are no policies in place.

We recommend creating a global default deny policy after you complete writing the policy for the traffic you want to allow. Use the stage policy feature to get your allowed traffic working as expected, then lock down the cluster to block unwanted traffic.

1. Create a staged global default-deny policy. It will show all the traffic that would be blocked if it were enforced.

   - Go to the `Policies Board`
   - On the bottom of the tier box `default` click on `Add Policy`
     - In the `Create Policy` page enter the policy name: `default-deny`
     - On the `Applies To` session, click `Add Namespace Seletor`
       First, lets apply only to the `vote` namespace
       - Select Key... `kubernetes.io/metadata.name`
       - =
       - Select Value... `vote`
     - On the field `Type` select both checkboxes: Ingress and Egress.
     - You are done. Click `Stage` on the top-right of your page.

   The staged policy does not affect the traffic directly but allows you to view the policy impact if it were to be enforced. You can see the denied traffic in the staged policy.

2. Based on the application design, the `db` lists on port `5432` and receive connections from the `worker` and the `result` microservices. 
   Let's use the Calico Cloud UI to create a policy to microsegment this traffic.

   - Go to the `Policies Board`
   - On the bottom of the tier box `platform` click on `Add Policy`
     - In the `Create Policy` page enter the policy name: `db`
     - Change the `Scope` from `Global` to `Namespace` and select the namespace `vote`
     - On the `Applies To` session, click `Add Label Seletor`
       - Select Key... `app`
       - =
       - Select Value... `db`
       - Click on `SAVE LABEL SELECTOR`
     - On the field `Type` select the checkbox for Ingress.
     - Click on `Add Ingress Rule`
       - On the `Create New Policy Rule` window,
         - Click on the `dropdown` with `Any Protocol`
         - Change the radio button to `Protocol is` and select `TCP`
         - In the field `To:` click on `Add Port` 
         - `Port is` 5432 - Save
         - In the field `From:`, click `Add Label Seletor`
           - Select Key... `app`
           - =
           - Select Value... `worker`
           - Click on `SAVE LABEL SELECTOR`  
           - On `OR`, click `Add Label Seletor`
           - Select Key... `app`
           - =
           - Select Value... `result`
           - Click on `SAVE LABEL SELECTOR`
         - Click on the button `Save Rule`
     - You are done. Click `Enforce` on the top-right of your page.

3. Now that you learned how to create policies using Calico Cloud UI, go ahead and create microsegmentation policies for all other workloads in the application.

4. If you create all the policies correctly, at some point, you will start seeing zero traffic being denied by your default-deny staged policy. At that point, you can go ahead and enforce the default-deny policy. Voilà! The vote namespace is now secure.

### Bonus - About Tiers

Tiers are a hierarchical construct used to group policies and enforce higher precedence policies that other teams cannot circumvent, providing the basis for **Identity-aware micro-segmentation**. 

All Calico and Kubernetes security policies reside in tiers. You can start “thinking in tiers” by grouping your teams and the types of policies within each group, such as security, platform, etc.

Policies are processed in sequential order from top to bottom.

![policy-processing](https://user-images.githubusercontent.com/104035488/206433417-0d186664-1514-41cc-80d2-17ed0d20a2f4.png)

Two mechanisms drive how traffic is processed across tiered policies:

- Labels and selectors
- Policy action rules

For more information about tiers, please refer to the Calico Cloud documentation [Understanding policy tiers](https://docs.calicocloud.io/get-started/tutorials/policy-tiers)

**Congratulations! You completed this workshop!**

--- 

[:arrow_right: Module 5 - Clean up](/mod/module-5-clean-up.md)    <br>

[:arrow_left: Module 3 - Malware protection, access control and quarantine with Thread Defence](/mod/module-3-threat-defense.md)     
[:leftwards_arrow_with_hook: Back to Main](/README.md)  
