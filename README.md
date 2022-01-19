# hello-minikube

GKEのサンプルアプリケーション[hello-app](https://github.com/GoogleCloudPlatform/kubernetes-engine-samples/blob/main/hello-app/README.md)をローカルのMinikubeにデプロイするための手順です。

## 前提条件

Minikubeがインストールされている必要があります。 \
まだインストールしていなければ、[こちら](https://minikube.sigs.k8s.io/docs/start/)を参考にインストールしてください。

## セットアップ

Minikubeを開始します。

```
minikube start
```

NGINX Ingressコントローラーを有効化します。

```
minikube addons enable ingress
```

次のように`nginx-ingress-controller-xxxxxxxxxx-xxxxx`がデプロイされていればOKです。

```
NAME                                        READY   STATUS      RESTARTS   AGE
ingress-nginx-admission-create--1-62d57     0/1     Completed   0          23h
ingress-nginx-admission-patch--1-zqvqf      0/1     Completed   1          23h
ingress-nginx-controller-5f66978484-fgjmt   1/1     Running     1          23h
```

## Hello Appをデプロイ

```
kubectl apply -k .
```

次のように`hello-web-xxxxxxxx-xxxxx`がデプロイされていればOKです。

```
NAME                         READY   STATUS    RESTARTS   AGE
hello-web-56b584bd88-fbdrs   1/1     Running   0          8m57s
hello-web-56b584bd88-nfq46   1/1     Running   0          8m57s
hello-web-56b584bd88-wwjhq   1/1     Running   0          8m57s
```

## ブラウザから確認する

SerivceをLoadBalancerタイプでデプロイしているので、`minikube service`コマンドで公開します。

```
minikube service hello-app -n hello-app
```

ブラウザが立ち上げり、次のようなページが見えれば完了です。

```
Hello, world!
Version: 2.0.0
Hostname: hello-app-76f778987d-kkpzl
```

何度かリロード（スーバーリロードが必要かも？）すると`Hostname`の値が変わり、3つのPodにラウンドロビンされていることが確認できると思います。

## Hello Appを削除

```
kubectl delete -k .
```
