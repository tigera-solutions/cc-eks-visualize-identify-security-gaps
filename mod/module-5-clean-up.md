# Module 5 - Clean up

1. Delete the example application stacks.

   ```bash
   kubectl delete -f pre/005-vote-app-manifest.yaml
   ```

2. Delete the AKS cluster.
   
   ```bash
   source ~/workshopvars.env
   eksctl delete cluster \
     --name $CLUSTERNAME \
     --region $REGION
   ```

3. Delete this repo

   ```bash
   cd .. && rm -Rf cc-eks-visualize-identify-security-gaps
   ```

4. Delete environment variables backup file.

   ```bash
   rm ~/workshopvars.env
   ```

---

[:leftwards_arrow_with_hook: Back to Main](/README.md)  <br>

[:arrow_left: Module 4 - Enforcing security policy to stop C&C attacks](/mod/module-4-security-policies.md)   