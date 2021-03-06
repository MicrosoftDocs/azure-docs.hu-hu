---
title: Egyéni NGINX bejövő vezérlő használata és HTTPS konfigurálása
services: azure-dev-spaces
ms.date: 12/10/2019
ms.topic: conceptual
description: Megtudhatja, hogyan konfigurálhatja az Azure Dev Spacest egy egyéni NGINX bejövő vezérlő használatára, és hogyan konfigurálhatja a HTTPS-t ezzel a bejövő vezérlővel
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, tárolók, Helm, szolgáltatási háló, szolgáltatási háló útválasztása, kubectl, k8s
ms.custom: devx-track-js
ms.openlocfilehash: b07d66e0031b907811c4ec251987aa020542b05a
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/20/2021
ms.locfileid: "107777508"
---
# <a name="use-a-custom-nginx-ingress-controller-and-configure-https"></a>Egyéni NGINX bejövő vezérlő használata és HTTPS konfigurálása

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Ez a cikk bemutatja, hogyan konfigurálhatja az Azure Dev Spacest egy egyéni NGINX bejövő vezérlő használatára. Ez a cikk azt is bemutatja, hogyan konfigurálhatja az egyéni bejövő forgalomvezérlőt HTTPS használatára.

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés. Ha még nincs fiókja, hozzon létre egy [ingyenes fiókot][azure-account-create].
* [Telepítette az Azure CLI-t.][az-cli]
* Azure Kubernetes Service (AKS-) fürt, engedélyezett Azure Dev Spaces-sel.
* [kubectl][kubectl] telepítve.
* [A Helm 3 telepítve van.][helm-installed]
* [Egyéni tartomány][custom-domain] DNS-zónával. [][dns-zone]  Ez a cikk feltételezi, hogy az egyéni tartomány és a DNS-zóna ugyanabban az erőforráscsoportban található, mint az AKS-fürt, de egy másik erőforráscsoportban is használhat egyéni tartományt és DNS-zónát.

## <a name="configure-a-custom-nginx-ingress-controller"></a>Egyéni NGINX bejövő forgalomvezérlő konfigurálása

Csatlakozzon a fürthöz a [kubectl][kubectl], a Kubernetes parancssori ügyfél használatával. Az [az aks get-credentials][az-aks-get-credentials] paranccsal konfigurálható `kubectl` a Kubernetes-fürthöz való csatlakozásra. Ez a parancs letölti a hitelesítő adatokat, és konfigurálja a Kubernetes parancssori felületét azok használatára.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

A fürthöz való csatlakozás ellenőrzéséhez használja a [kubectl get][kubectl-get] parancsot a fürtcsomópontok listájának lekéréséhez.

```console
kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Adja hozzá [a hivatalos stabil Helm-adattárat,][helm-stable-repo]amely az NGINX bejövő vezérlő Helm-diagramját tartalmazza.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Hozzon létre egy Kubernetes-névteret az NGINX bejövő forgalomvezérlőhöz, és telepítse a `helm` használatával.

```console
kubectl create ns nginx
helm install nginx stable/nginx-ingress --namespace nginx --version 1.27.0
```

> [!NOTE]
> A fenti példa létrehoz egy nyilvános végpontot a bejövő forgalom vezérlője számára. Ha ehelyett privát végpontot kell használnia a bejövő forgalomvezérlőhöz, adja hozzá a *--set controller.service.annotations halmazt." service \\ .beta \\ .kubernetes \\ .io/azure-load-balancer-internal"=true* paraméter a *Helm install parancshoz.* Például:
> ```console
> helm install nginx stable/nginx-ingress --namespace nginx --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true --version 1.27.0
> ```
> Ez a privát végpont az AKS-fürtöt üzembe helyező virtuális hálózaton belül van elérhető.

A kubectl get használatával szerezze be az NGINX bejövő forgalomvezérlő szolgáltatás [IP-címét.][kubectl-get]

```console
kubectl get svc -n nginx --watch
```

A mintakimenet az nginx névtérben az összes szolgáltatás *IP-címét* megjeleníti.

```console
NAME                                  TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     <pending>        80:31314/TCP,443:30521/TCP   10s
nginx-nginx-ingress-default-backend   ClusterIP      10.0.210.231   <none>           80/TCP                       10s
...
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     MY_EXTERNAL_IP   80:31314/TCP,443:30521/TCP   26s
```

Adjon hozzá *egy A* rekordot a DNS-zónához az NGINX szolgáltatás külső IP-címével az az network [dns record-set a add-record használatával.][az-network-dns-record-set-a-add-record]

```azurecli
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.nginx \
    --ipv4-address MY_EXTERNAL_IP
```

A fenti példa egy *A* rekordot ad hozzá a *MY_CUSTOM_DOMAIN* DNS-zónához.

Ebben a cikkben az [Azure Dev Spaces](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) Bike Sharing mintaalkalmazását használjuk az Azure Dev Spaces használatának szemléltető példáihoz. Klónozza az alkalmazást a GitHubról, és lépjen a könyvtárába:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

Nyissa [meg a values.yaml adatokat,][values-yaml] és tegye a következőket:
* Cserélje le  a<REPLACE_ME_WITH_HOST_SUFFIX>*összes példányát nginx-re. MY_CUSTOM_DOMAIN* a tartományt használja a *MY_CUSTOM_DOMAIN.* 
* Cserélje *kubernetes.io/ingress.class: traefik-azds # Dev Spaces-specific* helyére a *következő kubernetes.io/ingress.class: nginx # Egyéni bejövő forgalom*. 

Az alábbiakban egy példa látható egy frissített `values.yaml` fájlra:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
```

Mentse a módosításokat, és zárja be a fájlt.

Hozza létre *a Dev* Space-t a mintaalkalmazással a `azds space select` használatával.

```console
azds space select -n dev -y
```

A mintaalkalmazás üzembe helyezése a `helm install` használatával.

```console
helm install bikesharingsampleapp . --dependency-update --namespace dev --atomic
```

A fenti példa üzembe helyez egy mintaalkalmazást a *dev névtérben.*

Jelenítse meg a mintaalkalmazás a használatával való eléréséhez szükséges URL-címeket. `azds list-uris`

```console
azds list-uris
```

Az alábbi kimenet a következő példa URL-címeit jeleníti `azds list-uris` meg: .

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

Navigáljon *a bikesharingweb szolgáltatáshoz* a parancsból a nyilvános URL-cím `azds list-uris` megnyitásával. A fenti példában a *bikesharingweb* szolgáltatás nyilvános URL-címe `http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/` .

> [!NOTE]
> Ha a *bikesharingweb* szolgáltatás helyett hibalap jelenik  meg,  ellenőrizze, hogy frissítette-e a kubernetes.io/ingress.class jegyzetet és a gazdagépet a *values.yaml fájlban.*

Az paranccsal hozzon létre egy gyermekhelyet a Dev alatt, és listába sorolja a gyermek `azds space select` Dev Space eléréséhez szükséges URL-címeket. 

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

Az alábbi kimenetben a mintaalkalmazás azureuser1 gyermek Dev Space-beli eléréséhez szükséges `azds list-uris` URL-címeket  mutatjuk be.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

Navigáljon *a bikesharingweb szolgáltatáshoz* az *azureuser1* gyermek Dev Space-térben a parancs nyilvános URL-címének `azds list-uris` megnyitásával. A fenti példában az *azureuser1* gyermek Dev Space *bikesharingweb* szolgáltatásának nyilvános URL-címe `http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/` .

## <a name="configure-the-nginx-ingress-controller-to-use-https"></a>Az NGINX bejövő forgalomvezérlő konfigurálása HTTPS használatára

A [tanúsítványkezelővel][cert-manager] automatizálhatja a TLS-tanúsítvány kezelését, amikor az NGINX bejövő forgalomvezérlőt HTTPS használatára konfigurálja. A `helm` *certmanager* diagram telepítéséhez használja a következőt: .

```console
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml --namespace nginx
kubectl label namespace nginx certmanager.k8s.io/disable-validation=true
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace nginx --version v0.12.0 jetstack/cert-manager --set ingressShim.defaultIssuerName=letsencrypt --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Hozzon `letsencrypt-clusterissuer.yaml` létre egy fájlt, és frissítse az e-mail-mezőt az e-mail-címével.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: MY_EMAIL_ADDRESS
    privateKeySecretRef:
      name: letsencrypt
    solvers:
      - http01:
          ingress:
            class: nginx
```

> [!NOTE]
> A teszteléshez egy előkészítési kiszolgálót is használhat [a][letsencrypt-staging-issuer] *ClusterIssuerhez.*

Az `kubectl` alkalmazáshoz használja a következőt: `letsencrypt-clusterissuer.yaml` .

```console
kubectl apply -f letsencrypt-clusterissuer.yaml --namespace nginx
```

Frissítse [a values.yaml-t,][values-yaml] hogy tartalmazza a *tanúsítványkezelő* és a HTTPS használatának részleteit. Az alábbiakban egy példa látható egy frissített `values.yaml` fájlra:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-bikesharingweb-secret

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-gateway-secret
```

Frissítse a mintaalkalmazást a `helm` használatával:

```console
helm upgrade bikesharingsampleapp . --namespace dev --atomic
```

Keresse meg a mintaalkalmazást a *dev/azureuser1* gyermektérben, és figyelje meg, hogy a rendszer átirányítja a HTTPS használatára. Azt is figyelje meg, hogy az oldal betöltődik, de a böngésző néhány hibát mutat. A böngésző konzol megnyitásakor a hiba egy HTTP-erőforrásokat betöltő HTTPS-oldalhoz kapcsolódik. Például:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

A hiba kijavítása előtt frissítse a [BikeSharingWeb/azds.yaml][azds-yaml] fájlját az alábbihoz hasonló módon:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: nginx
        cert-manager.io/cluster-issuer: letsencrypt
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      tls:
      - hosts:
        - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
        secretName: dev-bikesharingweb-secret
...
```

Frissítse [a BikeSharingWeb/package.jsbe][package-json] az URL-csomag *függőségével.*

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

Frissítse a *getApiHostAsync* metódust a [BikeSharingWeb/lib/helpers.js][helpers-js] a HTTPS használatára:

```javascript
...
    getApiHostAsync: async function() {
        const apiRequest = await fetch('/api/host');
        const data = await apiRequest.json();
        
        var urlapi = require('url');
        var url = urlapi.parse(data.apiHost);

        console.log('apiHost: ' + "https://"+url.host);
        return "https://"+url.host;
    },
...
```

Lépjen a `BikeSharingWeb` könyvtárba, és futtassa a frissített `azds up` *BikeSharingWeb* szolgáltatást a használatával.

```console
cd ../BikeSharingWeb/
azds up
```

Keresse meg a mintaalkalmazást a *dev/azureuser1* gyermektérben, és figyelje meg, hogy a rendszer hiba nélkül átirányítja a HTTPS használatára.

## <a name="next-steps"></a>Következő lépések

További információ az Azure Dev Spacesről.

> [!div class="nextstepaction"]
> [Az Azure Dev Spaces működése](../how-dev-spaces-works.md)


[az-cli]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a#az_network_dns_record_set_a_add_record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-an-app-service-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[cert-manager]: https://cert-manager.io/
[helm-installed]: https://helm.sh/docs/intro/install/
[helm-stable-repo]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/lib/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[letsencrypt-staging-issuer]: https://cert-manager.io/docs/configuration/acme/#creating-a-basic-acme-issuer
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml
