# -Kubernetes-Day-9-Scaling-Rolling-Updates-Rollbacks

Today I continued my Kubernetes learning journey and focused on **Deployment scaling, rolling updates, rollout history, and rollback**.

## 🎯 What I Learned

* Understanding Kubernetes replicas
* Scaling Deployments up and down
* Rolling Updates
* Checking rollout status
* Viewing rollout history
* Inspecting Deployment revisions
* Rolling back to a previous version
* Verifying application health after rollback

---

## 1️⃣ Scaling a Deployment

My Deployment initially had **3 replicas**.

### Scale Up: 3 → 5

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Verified:

```bash
kubectl get deployment nginx-deployment
```

Result:

```text
READY   UP-TO-DATE   AVAILABLE
5/5     5            5
```

### Scale Down: 5 → 3

```bash
kubectl scale deployment nginx-deployment --replicas=3
```

Final result:

```text
READY   UP-TO-DATE   AVAILABLE
3/3     3            3
```

---

## 2️⃣ Rolling Update

I checked the existing container image:

```bash
kubectl get deployment nginx-deployment -o=jsonpath="{.spec.template.spec.containers[0].image}"
```

Current image:

```text
nginx
```

Then I updated the Deployment to:

```text
nginx:1.27
```

Command:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.27
```

### Check rollout status

```bash
kubectl rollout status deployment/nginx-deployment
```

Result:

```text
deployment "nginx-deployment" successfully rolled out
```

### Verify image

```bash
kubectl get deployment nginx-deployment -o=jsonpath="{.spec.template.spec.containers[0].image}"
```

Result:

```text
nginx:1.27
```

---

## 3️⃣ Rollout History

I checked the Deployment's revision history:

```bash
kubectl rollout history deployment/nginx-deployment
```

Result:

```text
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

I then inspected Revision 2:

```bash
kubectl rollout history deployment/nginx-deployment --revision=2
```

Revision 2 contained:

```text
Image: nginx:1.27
```

---

## 4️⃣ Rollback

To practice a real-world recovery scenario, I rolled the Deployment back to the previous revision.

```bash
kubectl rollout undo deployment/nginx-deployment
```

Then I verified the rollout:

```bash
kubectl rollout status deployment/nginx-deployment
```

Result:

```text
deployment "nginx-deployment" successfully rolled out
```

I checked the image again:

```bash
kubectl get deployment nginx-deployment -o=jsonpath="{.spec.template.spec.containers[0].image}"
```

Result:

```text
nginx
```

---

## 5️⃣ Final Health Check

```bash
kubectl get deployment nginx-deployment
```

Final output:

```text
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           41d
```

✅ 3 replicas running
✅ Deployment available
✅ Rollback completed successfully
✅ Previous image restored

---

## 💡 Key Takeaways

### Scaling

Kubernetes can easily increase or decrease the number of application replicas depending on workload.

```bash
kubectl scale deployment <deployment-name> --replicas=<number>
```

### Rolling Updates

Deployments can gradually replace old Pods with new Pods instead of replacing everything at once.

```bash
kubectl set image deployment/<deployment-name> <container>=<new-image>
```

### Rollout Monitoring

```bash
kubectl rollout status deployment/<deployment-name>
```

helps verify whether an update completed successfully.

### Rollback

If a deployment has a problem, Kubernetes can return to a previous revision:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

## 🧠 Interview Point

**Q: What is a Kubernetes Rolling Update?**

A Rolling Update gradually replaces old Pods with new Pods when a Deployment is updated. This helps keep the application available while the new version is being deployed.

**Q: Why is rollback useful?**

Rollback provides a recovery mechanism when a newly deployed version causes unexpected problems.

---
##Screenshots:
<img width="959" height="567" alt="Screenshot 2026-09-23 213910" src="https://github.com/user-attachments/assets/37284f44-eea2-4b16-9db6-eeb1bd012a08" />
<img width="959" height="563" alt="Screenshot 2026-09-23 213920" src="https://github.com/user-attachments/assets/0ba332fe-9d77-409b-9f1b-8b632608939f" />
<img width="959" height="571" alt="Screenshot 2026-09-23 213931" src="https://github.com/user-attachments/assets/20e70620-c177-41a9-aa33-cf1d020190f7" />
<img width="959" height="557" alt="Screenshot 2026-09-23 214048" src="https://github.com/user-attachments/assets/1832c943-c48b-4400-8903-1d832558fb70" />
<img width="949" height="536" alt="Screenshot 2026-09-23 213954" src="https://github.com/user-attachments/assets/7a0c4b0e-d6b2-4038-be27-a8ce1f2d7a8f" />

## 🚀 Day 9 Completed

Today I learned how to:

**Scale → Update → Monitor → Inspect → Rollback → Verify**

This was an important step toward understanding how Kubernetes Deployments are managed in real-world DevOps environments.
**Consistent and constant at work.** ☁️
