# fleet-infra

Changing replica from 1 to 2:

```
# before sync
$ flux get kustomizations
NAME       	READY	MESSAGE                                                        	REVISION                                     	SUSPENDED
flux-system	True 	Applied revision: main/5d4cf5994da570dd3ba43c16219e7348fe2f963c	main/5d4cf5994da570dd3ba43c16219e7348fe2f963c	False
$ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
alpine                     1/1     Running   4          4h16m
podinfo-7466f7f75b-lfb2p   1/1     Running   0          4h8m

# after sync
$ flux get kustomizations
NAME       	READY	MESSAGE                                                        	REVISION                                     	SUSPENDED
flux-system	True 	Applied revision: main/d7eeeb8376af1aca9726f244e6d65d72deeaaabe	main/d7eeeb8376af1aca9726f244e6d65d72deeaaabe	False
$ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
alpine                     1/1     Running   4          4h16m
podinfo-7466f7f75b-lfb2p   1/1     Running   0          4h8m
podinfo-7466f7f75b-wdj62   0/1     Running   0          5s

$ kubectl logs kustomize-controller-5dd4d4fd4f-2kdg2 -n flux-system --tail=1
{"level":"info","ts":"2021-03-25T05:26:40.569Z","logger":"controller.kustomization","msg":"Reconciliation finished in 1.931108582s, next run in 10m0s","reconciler group":"kustomize.toolkit.fluxcd.io","reconciler kind":"Kustomization","name":"flux-system","namespace":"flux-system","revision":"main/d7eeeb8376af1aca9726f244e6d65d72deeaaabe"}
```
