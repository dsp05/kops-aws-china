README.md

This document addresses some issues for setting up k8s clusters in AWS China regions using kops. It has been tested for kops 1.14.0 and k8s 1.14.6.

Read https://github.com/nwcdlabs/kops-cn and https://github.com/kubernetes/kops/blob/master/docs/aws-china.md first.

* Find the version of kops/k8s you want to use (https://github.com/kubernetes/kops/blob/master/channels/stable)

* Install kops.

* Download kops/k8s files and copy into your own s3 bucket (https://github.com/kubernetes/kops/blob/master/docs/aws-china.md#offline-mode)
  * Set KUBERNETES_VERSION and KOPS_VERSION to the versions you choose in previous step
  * For MacOS, add "darwin/amd64/kops" and "linux/amd64/kops" to KOPS_ASSETS
  * Put k8s assets into kubernetes-release/, NOT kubernetes/

* Setup your own docker registry mirror.

* Find the image for your k8s version (https://github.com/kubernetes/kops/blob/master/channels/stable). Copy the ami to your region (https://github.com/kubernetes-incubator/kube-aws/pull/390#issue-212435055).

* Create cluster using `kops create cluster`.

* `kops edit cluster`, add the following into spec
```
spec:
  assets:
    containerRegistry: [YOUR_REGISTRY]
    fileRepository: [YOUR_S3_BUCKET_FOR_KOPS_AND_K8S_FILES]
  docker:
    registryMirrors:
      - [YOUR_REGISTRY_MIRROR]
```

* `kops update cluster --yes`.