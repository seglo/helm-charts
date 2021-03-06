# Console Operator

_Note: This operator is experimental, and only supports a simple installation. It does not support
upgrades yet._

This directory provides files to install the Console Operator, which can be used to install
Console itself.

To install the operator, apply the `manifests/kustomization.yaml` file with `kubectl`:

``` sh
kubectl apply -k manifests
```

This will set up the Console operator. This requires at least kubectl 1.14.

For production usage, it is recommended you create your own `kustomization.yaml`, and use the
manifests directory as a base inside your own repository. See kustomize.io for more details.

## Install Console

The operator will install Console for you. It looks for a `Console` resource inside of its
namespace. Once this resource is created, it will spin up an instance of Console.

There is a [complete example available](manifests/console_cr.yaml).

To get started quickly, create a file `console.yaml` with contents:

``` yaml
apiVersion: app.lightbend.com/v1alpha1
kind: Console
metadata:
  name: my-console
spec:
  imageCredentials:
    username: my-lightbend-username
    password: my-lightbend-password
  exposeServices: NodePort
```

Replace username and password with your commercial credentials. Then apply this file, assuming the
operator is installed in the `lightbend` namespace:

``` sh
kubectl apply -n lightbend console.yaml
```

After this you should see an instance of Console created in the namespace. You can then access it as
normal. If on Minikube, you can use the NodePort service to access at `https://$(minikube ip):30080`.

### Uninstall

You can uninstall the console instance by deleting it using kubectl. Assuming you used the prior
example with name `my-console` in namespace `lightbend`:

``` sh
kubectl delete -n lightbend console my-console
```

## Directory Structure

- [kustomization.yaml](kustomization.yaml) is the resource used to install the operator.
- [manifests](manifests) is automatically generated by the operator-sdk kit, packaging up our helm chart into an
  operator. Files in here should not be modified manually.
- [src](src) contains customizations on top of the operator-sdk generated files.

## Development

Requires kubecfg and operator-sdk to generate the files. See the [Makefile](Makefile) for details on
how to build the project.

### OLM

The OLM bundle files live in [olm](olm). These were originally generated by
https://dev.operatorhub.io/bundle with some manual tweaks.

These files are added to https://github.com/operator-framework, in both the
upstream-community-operators and community-operators folders.

To test, you can follow the process at
https://github.com/operator-framework/community-operators/blob/master/docs/testing-operators.md -
the relevant files are located in [olm/test](olm/test).
