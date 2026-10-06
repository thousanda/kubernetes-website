---
title: よく使われるラベル、アノテーション、Taint
content_type: concept
weight: 40
no_list: true
card:
  name: reference
  weight: 30
  anchors:
  - anchor: "#labels-annotations-and-taints-used-on-api-objects"
    title: ラベル、アノテーション、Taint
---

<!-- overview -->

Kubernetesは、`kubernetes.io`および`k8s.io`名前空間のすべてのラベル、アノテーション、Taintを予約しています。

このドキュメントは、値のリファレンスとしての役割と、値の割り当てを調整する場としての役割を兼ねています。

<!-- body -->

## APIオブジェクトで使われるラベル、アノテーション、Taint {#labels-annotations-and-taints-used-on-api-objects}

### apf.kubernetes.io/autoupdate-spec

種類: アノテーション

例: `apf.kubernetes.io/autoupdate-spec: "true"`

使用対象: [`FlowSchema`および`PriorityLevelConfiguration`オブジェクト](/docs/concepts/cluster-administration/flow-control/#defaults)

FlowSchemaまたはPriorityLevelConfigurationでこのアノテーションをtrueに設定すると、そのオブジェクトの`spec`はkube-apiserverによって管理されます。
APIサーバーが認識していないAPFオブジェクトに自動更新のアノテーションを付けると、APIサーバーはそのオブジェクト全体を削除します。
それ以外の場合、APIサーバーはオブジェクトのspecを管理しません。
詳細については、[必須および推奨の設定オブジェクトの保守](/docs/concepts/cluster-administration/flow-control/#maintenance-of-the-mandatory-and-suggested-configuration-objects)を参照してください。

### app.kubernetes.io/component

種類: ラベル

例: `app.kubernetes.io/component: "database"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

アプリケーションアーキテクチャ内のコンポーネントです。

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### app.kubernetes.io/created-by (非推奨) {#app-kubernetes-io-created-by-deprecated}

種類: ラベル

例: `app.kubernetes.io/created-by: "controller-manager"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

このリソースを作成したコントローラーまたはユーザーです。

{{< note >}}
v1.9以降、このラベルは非推奨です。
{{< /note >}}

### app.kubernetes.io/instance

種類: ラベル

例: `app.kubernetes.io/instance: "mysql-abcxyz"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

アプリケーションのインスタンスを識別する一意の名前です。
一意でない名前を割り当てる場合は、[app.kubernetes.io/name](#app-kubernetes-io-name)を使用します。

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### app.kubernetes.io/managed-by

種類: ラベル

例: `app.kubernetes.io/managed-by: "helm"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

アプリケーションの運用管理に使用しているツールです。

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### app.kubernetes.io/name

種類: ラベル

例: `app.kubernetes.io/name: "mysql"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

アプリケーションの名前です。

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### app.kubernetes.io/part-of

種類: ラベル

例: `app.kubernetes.io/part-of: "wordpress"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

このオブジェクトが属する上位のアプリケーションの名前です。

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### app.kubernetes.io/version

種類: ラベル

例: `app.kubernetes.io/version: "5.7.21"`

使用対象: すべてのオブジェクト(通常は[ワークロードリソース](/docs/reference/kubernetes-api/workload-resources/)で使用)

アプリケーションの現在のバージョンです。

よく使われる値の形式には、次のものがあります:

- [セマンティックバージョン](https://semver.org/spec/v1.0.0.html)
- ソースコードのGit[リビジョンハッシュ](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#_single_revisions)

[推奨ラベル](/docs/concepts/overview/working-with-objects/common-labels/#labels)の1つです。

### applyset.kubernetes.io/additional-namespaces (Alpha) {#applyset-kubernetes-io-additional-namespaces}

種類: アノテーション

例: `applyset.kubernetes.io/additional-namespaces: "namespace1,namespace2"`

使用対象: ApplySetの親として使用されるオブジェクト

このアノテーションの使用はAlpha段階です。
Kubernetesバージョン{{< skew currentVersion >}}では、Secret、ConfigMap、または定義元の{{< glossary_tooltip term_id="CustomResourceDefinition" text="CustomResourceDefinition" >}}に`applyset.kubernetes.io/is-parent-type`ラベルが付いているカスタムリソースで、このアノテーションを使用できます。

[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このアノテーションは、ApplySetを追跡するために使用する親オブジェクトに適用され、ApplySetの範囲を親オブジェクト自身の名前空間(存在する場合)の外にも拡張します。
値は、親の名前空間以外でオブジェクトが存在する名前空間の名前をカンマで区切ったリストです。

### applyset.kubernetes.io/contains-group-kinds (Alpha) {#applyset-kubernetes-io-contains-group-kinds}

種類: アノテーション

例: `applyset.kubernetes.io/contains-group-kinds: "certificates.cert-manager.io,configmaps,deployments.apps,secrets,services"`

使用対象: ApplySetの親として使用されるオブジェクト

このアノテーションの使用はAlpha段階です。
Kubernetesバージョン{{< skew currentVersion >}}では、Secret、ConfigMap、または定義元のCustomResourceDefinitionに`applyset.kubernetes.io/is-parent-type`ラベルが付いているカスタムリソースで、このアノテーションを使用できます。

[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このアノテーションは、ApplySetを追跡するために使用する親オブジェクトに適用され、ApplySetのメンバーオブジェクトの一覧取得を最適化します。
ツールはディスカバリーや別の最適化を利用できるため、ApplySetの仕様では任意です。
ただし、Kubernetesバージョン{{< skew currentVersion >}}では、kubectlがこのアノテーションを必須としています。
設定する場合、値は完全修飾名の形式、すなわち`<resource>.<group>`で表したgroup-kindをカンマで区切ったリストでなければなりません。

### applyset.kubernetes.io/contains-group-resources (非推奨) {#applyset-kubernetes-io-contains-group-resources}

種類: アノテーション

例: `applyset.kubernetes.io/contains-group-resources: "certificates.cert-manager.io,configmaps,deployments.apps,secrets,services"`

使用対象: ApplySetの親として使用されるオブジェクト

Kubernetesバージョン{{< skew currentVersion >}}では、Secret、ConfigMap、または定義元のCustomResourceDefinitionに`applyset.kubernetes.io/is-parent-type`ラベルが付いているカスタムリソースで、このアノテーションを使用できます。

[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このアノテーションは、ApplySetを追跡するために使用する親オブジェクトに適用され、ApplySetのメンバーオブジェクトの一覧取得を最適化します。
ツールはディスカバリーや別の最適化を利用できるため、ApplySetの仕様では任意です。
ただし、Kubernetesバージョン{{< skew currentVersion >}}では、kubectlがこのアノテーションを必須としています。
設定する場合、値は完全修飾名の形式、すなわち`<resource>.<group>`で表したgroup-kindをカンマで区切ったリストでなければなりません。

{{< note >}}
このアノテーションは現在非推奨であり、[`applyset.kubernetes.io/contains-group-kinds`](#applyset-kubernetes-io-contains-group-kinds)に置き換えられています。
ApplySetがBetaまたはGAとなる際に、このアノテーションのサポートは削除されます。
{{< /note >}}

### applyset.kubernetes.io/id (Alpha) {#applyset-kubernetes-io-id}

種類: ラベル

例: `applyset.kubernetes.io/id: "applyset-0eFHV8ySqp7XoShsGvyWFQD3s96yqwHmzc4e0HR1dsY-v1"`

使用対象: ApplySetの親として使用されるオブジェクト

このラベルの使用はAlpha段階です。
Kubernetesバージョン{{< skew currentVersion >}}では、Secret、ConfigMap、または定義元のCustomResourceDefinitionに`applyset.kubernetes.io/is-parent-type`ラベルが付いているカスタムリソースで、このラベルを使用できます。

[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このラベルは、オブジェクトをApplySetの親オブジェクトとして位置付けます。
値はApplySetの一意のIDであり、親オブジェクト自体を識別する情報から導出されます。
このIDは、ラベルが付けられたオブジェクトのgroup、kind、name、namespaceのハッシュを、次の形式でbase64エンコード(RFC4648のURLセーフなエンコードを使用)したもので**なければなりません**:
`<base64(sha256(<name>.<namespace>.<kind>.<group>))>`。
このラベルの値とオブジェクトのUIDに関係はありません。

### applyset.kubernetes.io/is-parent-type (Alpha) {#applyset-kubernetes-io-is-parent-type}

種類: ラベル

例: `applyset.kubernetes.io/is-parent-type: "true"`

使用対象: CustomResourceDefinition(CRD)

このラベルの使用はAlpha段階です。
[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
CustomResourceDefinition(CRD)にこのラベルを設定すると、そのCRDが定義するカスタムリソースの型(CRD自体ではありません)を、ApplySetの親として使用できる型として指定できます。
このラベルで許可される値は`"true"`のみです。
CRDをApplySetの有効な親として扱わない場合は、このラベルを省略してください。

### applyset.kubernetes.io/part-of (Alpha) {#applyset-kubernetes-io-part-of}

種類: ラベル

例: `applyset.kubernetes.io/part-of: "applyset-0eFHV8ySqp7XoShsGvyWFQD3s96yqwHmzc4e0HR1dsY-v1"`

使用対象: すべてのオブジェクト

このラベルの使用はAlpha段階です。
[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このラベルは、オブジェクトをApplySetのメンバーとして位置付けます。
ラベルの値は、親オブジェクトの`applyset.kubernetes.io/id`ラベルの値と一致している**必要があります**。

### applyset.kubernetes.io/tooling (Alpha) {#applyset-kubernetes-io-tooling}

種類: アノテーション

例: `applyset.kubernetes.io/tooling: "kubectl/v{{< skew currentVersion >}}"`

使用対象: ApplySetの親として使用されるオブジェクト

このアノテーションの使用はAlpha段階です。
Kubernetesバージョン{{< skew currentVersion >}}では、Secret、ConfigMap、または定義元のCustomResourceDefinitionに`applyset.kubernetes.io/is-parent-type`ラベルが付いているカスタムリソースで、このアノテーションを使用できます。

[kubectlのApplySetに基づくプルーニング](/docs/tasks/manage-kubernetes-objects/declarative-config/#alternative-kubectl-apply-f-directory-prune)を実装するための仕様の一部です。
このアノテーションは、ApplySetを追跡するために使用する親オブジェクトに適用され、そのApplySetを管理するツールを示します。
ツールは、他のツールが所有するApplySetの変更を拒否するべきです。
値は`<toolname>/<semver>`の形式でなければなりません。

### apps.kubernetes.io/pod-index (Beta) {#apps-kubernetes.io-pod-index}

種類: ラベル

例: `apps.kubernetes.io/pod-index: "0"`

使用対象: Pod

StatefulSetコントローラーがStatefulSet用のPodを作成すると、そのPodにこのラベルを設定します。
ラベルの値は、作成されるPodの序数インデックスです。

詳細については、StatefulSetの[Podインデックスラベル](/docs/concepts/workloads/controllers/statefulset/#pod-index-label)を参照してください。
Podにこのラベルを追加するには、[PodIndexLabel](/docs/reference/command-line-tools-reference/feature-gates/)フィーチャーゲートを有効にする必要があります。

### resource.kubernetes.io/pod-claim-name

種類: アノテーション

例: `resource.kubernetes.io/pod-claim-name: "my-pod-claim"`

使用対象: ResourceClaim

このアノテーションは、生成されたResourceClaimに割り当てられます。
値は、そのResourceClaimの作成対象となったPodの`.spec`内にあるリソースクレームの名前に対応します。
[動的リソース割り当て](/docs/concepts/resource-management/dynamic-resource-allocation/)では、検出可能なデバイスメタデータの機能がこのアノテーションを使用し、テンプレートに基づくクレームについて、生成されたResourceClaimをPodのクレーム名(`pod.spec.resourceClaims[].name`)に対応付けます。
Kubernetesがこのアノテーションを管理するため、変更しないでください。

### cluster-autoscaler.kubernetes.io/safe-to-evict

種類: アノテーション

例: `cluster-autoscaler.kubernetes.io/safe-to-evict: "true"`

使用対象: Pod

このアノテーションを`"true"`に設定すると、通常は他のルールで禁止される場合でも、クラスターオートスケーラーによるPodの退避が許可されます。
クラスターオートスケーラーは、このアノテーションが明示的に`"false"`に設定されたPodを退避させません。
実行を継続したい重要なPodに、この値を設定できます。
このアノテーションを設定しない場合、クラスターオートスケーラーはPod単位の動作に従います。

### config.kubernetes.io/local-config

種類: アノテーション

例: `config.kubernetes.io/local-config: "true"`

使用対象: すべてのオブジェクト

このアノテーションは、Kubernetes APIに送信すべきでないローカル設定としてオブジェクトを識別するために、マニフェストで使用します。

このアノテーションの値が`"true"`の場合、オブジェクトはクライアント側のツールのみが使用するものであり、APIサーバーに送信すべきでないことを宣言します。

値`"false"`は、通常ならローカル設定とみなされる場合でも、オブジェクトをAPIサーバーに送信すべきであることを宣言するために使用できます。

このアノテーションは、Kustomizeや同様のサードパーティーツールが使用するKubernetes Resource Model(KRM) Functions仕様の一部です。
例えば、Kustomizeはこのアノテーションが付いたオブジェクトを最終的なビルド出力から削除します。

### container.apparmor.security.beta.kubernetes.io/* (非推奨) {#container-apparmor-security-beta-kubernetes-io}

種類: アノテーション

例: `container.apparmor.security.beta.kubernetes.io/my-container: my-custom-profile`

使用対象: Pod

このアノテーションを使用すると、KubernetesのPod内のコンテナに対してAppArmorセキュリティプロファイルを指定できます。
Kubernetes v1.30以降では、代わりに`appArmorProfile`フィールドを使用して設定するべきです。
詳細については、[AppArmor](/docs/tutorials/security/apparmor/)チュートリアルを参照してください。
このチュートリアルでは、AppArmorを使用してコンテナの機能とアクセスを制限する方法を説明しています。

指定したプロファイルは、コンテナ化されたプロセスが従うべきルールと制限を定めます。
これにより、コンテナのセキュリティポリシーと分離を強制できます。

### csi.alpha.kubernetes.io/node-id (非推奨) {#csi-alpha-kubernetes-io-node-id}

種類: アノテーション

例: `csi.alpha.kubernetes.io/node-id: "node-12345"`

使用対象: VolumeAttachment

このアノテーションは、[CSINode](/docs/reference/kubernetes-api/storage/csi-node-v1/)オブジェクトを利用できない場合の代替手段として、CSIドライバーが使用するノード識別子を記録します。
CSIのexternal-attacherサイドカーコンテナが、ボリュームをアタッチする前に値を設定します。

適切なCSINodeが存在しない場合に、ボリュームをデタッチするための代替手段を提供します。

このアノテーションは非推奨のため、KubernetesプロジェクトはVolumeAttachmentを含むいかなるオブジェクトにも設定**しない**ことを推奨します。

### csi.volume.kubernetes.io/nodeid (非推奨) {#csi-volume-kubernetes-io-nodeid}

種類: アノテーション

例: `csi.volume.kubernetes.io/nodeid: "node-12345"`

使用対象: Node

このアノテーションは、Container Storage Interface(CSI)ドライバーが認識するノードの識別子を指定します。
`kubelet`はドライバーの登録時にCSIドライバーの`NodeGetInfo` gRPCメソッドを呼び出してノードIDを取得し、このアノテーションに設定します。
external-attacherサイドカーコンテナは、ボリュームをアタッチまたはデタッチする際に、このアノテーションを読み取ってノードIDを取得します。

このアノテーションは非推奨となり、[`spec.drivers[].nodeID`フィールド](/docs/reference/kubernetes-api/storage/csi-node-v1/#CSINodeDriver)を通じて同じ機能を提供するCSINodeオブジェクトに置き換えられています。

### deployment.kubernetes.io/desired-replicas

種類: アノテーション

例: `deployment.kubernetes.io/desired-replicas: "3"`

使用対象: ReplicaSet

このアノテーションは、Deploymentコントローラーが管理対象のReplicaSetに設定します。
値は、このReplicaSetを所有するDeploymentの希望するレプリカ数(`.spec.replicas`)を表します。
Deploymentコントローラーは、ローリングアップデートやスケーリングの操作中に希望する状態を追跡するため、このアノテーションを使用します。

これはDeploymentコントローラーが使用する内部アノテーションであり、手動で変更するべきではありません。

### deployment.kubernetes.io/max-replicas

種類: アノテーション

例: `deployment.kubernetes.io/max-replicas: "5"`

使用対象: ReplicaSet

このアノテーションは、Deploymentコントローラーが管理対象のReplicaSetに設定します。
値は、ローリングアップデート中にこのReplicaSetで許可される最大レプリカ数を表します。
これは、Deploymentのローリングアップデート戦略の`maxSurge`パラメーターを実装するために使用されます。
このパラメーターは、更新中に希望する数を超えて追加で作成できるPodの数を制御します。

これはDeploymentコントローラーが使用する内部アノテーションであり、手動で変更するべきではありません。

### deployment.kubernetes.io/revision

種類: アノテーション

例: `deployment.kubernetes.io/revision: "2"`

使用対象: ReplicaSet

このアノテーションは、Deploymentコントローラーが管理対象のReplicaSetに設定します。
値はDeploymentのリビジョン番号を表します。
DeploymentのPodテンプレート(`.spec.template`)が変更されるたびに、リビジョン番号が増加します。
このアノテーションはロールアウト履歴の追跡に使用され、`kubectl rollout undo`を使用した以前のリビジョンへのロールバックを可能にします。

リビジョン番号は、`kubectl rollout history deployment/<name>`を実行した際にも表示されます。

これはDeploymentコントローラーが使用する内部アノテーションであり、手動で変更するべきではありません。

### deployment.kubernetes.io/revision-history

種類: アノテーション

例: `deployment.kubernetes.io/revision-history: "1,3"`

使用対象: ReplicaSet

このアノテーションは、ロールバックによってReplicaSetが再利用される際に、DeploymentコントローラーがそのReplicaSetに設定します。
値は、そのReplicaSetがDeploymentで使用された過去のすべてのリビジョン番号をカンマで区切ったリストです。
`deployment.kubernetes.io/revision`アノテーションが新しいリビジョン番号に更新される際に、履歴として保持されます。

これはDeploymentコントローラーが使用する内部アノテーションであり、手動で変更するべきではありません。

### internal.config.kubernetes.io/* (予約済みプレフィックス) {#internal.config.kubernetes.io-reserved-wildcard}

種類: アノテーション

使用対象: すべてのオブジェクト

このプレフィックスは、Kubernetes Resource Model(KRM) Functions仕様に従ってオーケストレーターとして動作するツールの内部使用のために予約されています。
このプレフィックスを持つアノテーションはオーケストレーション処理の内部で使用され、ファイルシステム上のマニフェストには保存されません。
つまり、オーケストレーターツールは、ローカルファイルシステムからファイルを読み込む際にこれらのアノテーションを設定し、関数の出力をファイルシステムに書き戻す際に削除するべきです。

KRM関数は、個々のアノテーションで別途指定されていない限り、このプレフィックスを持つアノテーションを変更**してはいけません**。
これにより、オーケストレーターツールは既存の関数を変更することなく、内部アノテーションを追加できます。

### internal.config.kubernetes.io/path

種類: アノテーション

例: `internal.config.kubernetes.io/path: "relative/file/path.yaml"`

使用対象: すべてのオブジェクト

このアノテーションは、オブジェクトの読み込み元のマニフェストファイルへの、スラッシュ区切りのOSに依存しない相対パスを記録します。
パスは、オーケストレーターツールが決定するファイルシステム上の固定の場所からの相対パスです。

このアノテーションは、Kustomizeや同様のサードパーティーツールが使用するKubernetes Resource Model(KRM) Functions仕様の一部です。

KRM関数は、参照先のファイルを変更する場合を除き、入力オブジェクトのこのアノテーションを変更**するべきではありません**。
KRM関数は、生成するオブジェクトにこのアノテーションを含める**ことができます**。

### internal.config.kubernetes.io/index

種類: アノテーション

例: `internal.config.kubernetes.io/index: "2"`

使用対象: すべてのオブジェクト

このアノテーションは、オブジェクトの読み込み元のマニフェストファイル内で、そのオブジェクトを含むYAMLドキュメントの位置を、0から始まるインデックスで記録します。
YAMLドキュメントは3つのハイフン(`---`)で区切られ、それぞれ1つのオブジェクトを含むことができます。
このアノテーションを指定しない場合、値は0とみなされます。

このアノテーションは、Kustomizeや同様のサードパーティーツールが使用するKubernetes Resource Model(KRM) Functions仕様の一部です。

KRM関数は、参照先のファイルを変更する場合を除き、入力オブジェクトのこのアノテーションを変更**するべきではありません**。
KRM関数は、生成するオブジェクトにこのアノテーションを含める**ことができます**。

### kube-scheduler-simulator.sigs.k8s.io/bind-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/bind-result: '{"DefaultBinder":"success"}'`

使用対象: Pod

このアノテーションは、bindスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/filter-result

種類: アノテーション

例:

```yaml
kube-scheduler-simulator.sigs.k8s.io/filter-result: >-
      {"node-282x7":{"AzureDiskLimits":"passed","EBSLimits":"passed","GCEPDLimits":"passed","InterPodAffinity":"passed","NodeAffinity":"passed","NodeName":"passed","NodePorts":"passed","NodeResourcesFit":"passed","NodeUnschedulable":"passed","NodeVolumeLimits":"passed","PodTopologySpread":"passed","TaintToleration":"passed","VolumeBinding":"passed","VolumeRestrictions":"passed","VolumeZone":"passed"},"node-gp9t4":{"AzureDiskLimits":"passed","EBSLimits":"passed","GCEPDLimits":"passed","InterPodAffinity":"passed","NodeAffinity":"passed","NodeName":"passed","NodePorts":"passed","NodeResourcesFit":"passed","NodeUnschedulable":"passed","NodeVolumeLimits":"passed","PodTopologySpread":"passed","TaintToleration":"passed","VolumeBinding":"passed","VolumeRestrictions":"passed","VolumeZone":"passed"}}
```

使用対象: Pod

このアノテーションは、filterスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/finalscore-result

種類: アノテーション

例:

```yaml
kube-scheduler-simulator.sigs.k8s.io/finalscore-result: >-
      {"node-282x7":{"ImageLocality":"0","InterPodAffinity":"0","NodeAffinity":"0","NodeNumber":"0","NodeResourcesBalancedAllocation":"76","NodeResourcesFit":"73","PodTopologySpread":"200","TaintToleration":"300","VolumeBinding":"0"},"node-gp9t4":{"ImageLocality":"0","InterPodAffinity":"0","NodeAffinity":"0","NodeNumber":"0","NodeResourcesBalancedAllocation":"76","NodeResourcesFit":"73","PodTopologySpread":"200","TaintToleration":"300","VolumeBinding":"0"}}
```

使用対象: Pod

このアノテーションは、scoreスケジューラープラグインのスコアからスケジューラーが計算した最終スコアを記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/permit-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/permit-result: '{"CustomPermitPlugin":"success"}'`

使用対象: Pod

このアノテーションは、permitスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/permit-result-timeout

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/permit-result-timeout: '{"CustomPermitPlugin":"10s"}'`

使用対象: Pod

このアノテーションは、permitスケジューラープラグインが返したタイムアウトを記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/postfilter-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/postfilter-result: '{"DefaultPreemption":"success"}'`

使用対象: Pod

このアノテーションは、postfilterスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/prebind-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/prebind-result: '{"VolumeBinding":"success"}'`

使用対象: Pod

このアノテーションは、prebindスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/prefilter-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/prebind-result: '{"NodeAffinity":"[\"node-\a"]"}'`

使用対象: Pod

このアノテーションは、prefilterスケジューラープラグインのPreFilterの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/prefilter-result-status

種類: アノテーション

例:

```yaml
kube-scheduler-simulator.sigs.k8s.io/prefilter-result-status: >-
      {"InterPodAffinity":"success","NodeAffinity":"success","NodePorts":"success","NodeResourcesFit":"success","PodTopologySpread":"success","VolumeBinding":"success","VolumeRestrictions":"success"}
```

使用対象: Pod

このアノテーションは、prefilterスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/prescore-result

種類: アノテーション

例:

```yaml
    kube-scheduler-simulator.sigs.k8s.io/prescore-result: >-
      {"InterPodAffinity":"success","NodeAffinity":"success","NodeNumber":"success","PodTopologySpread":"success","TaintToleration":"success"}
```

使用対象: Pod

このアノテーションは、prefilterスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/reserve-result

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/reserve-result: '{"VolumeBinding":"success"}'`

使用対象: Pod

このアノテーションは、reserveスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/result-history

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/result-history: '[]'`

使用対象: Pod

このアノテーションは、スケジューラープラグインによる過去のすべてのスケジューリング結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/score-result

種類: アノテーション

```yaml
    kube-scheduler-simulator.sigs.k8s.io/score-result: >-
      {"node-282x7":{"ImageLocality":"0","InterPodAffinity":"0","NodeAffinity":"0","NodeNumber":"0","NodeResourcesBalancedAllocation":"76","NodeResourcesFit":"73","PodTopologySpread":"0","TaintToleration":"0","VolumeBinding":"0"},"node-gp9t4":{"ImageLocality":"0","InterPodAffinity":"0","NodeAffinity":"0","NodeNumber":"0","NodeResourcesBalancedAllocation":"76","NodeResourcesFit":"73","PodTopologySpread":"0","TaintToleration":"0","VolumeBinding":"0"}}
```

使用対象: Pod

このアノテーションは、scoreスケジューラープラグインの結果を記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kube-scheduler-simulator.sigs.k8s.io/selected-node

種類: アノテーション

例: `kube-scheduler-simulator.sigs.k8s.io/selected-node: node-282x7`

使用対象: Pod

このアノテーションは、スケジューリングサイクルで選択されたノードを記録します。
https://sigs.k8s.io/kube-scheduler-simulator で使用されます。

### kubernetes.io/arch

種類: ラベル

例: `kubernetes.io/arch: "amd64"`

使用対象: Node

kubeletは、Goで定義されている`runtime.GOARCH`をこのラベルの値に設定します。
これは、ARMノードとx86ノードが混在している場合に便利です。

### kubernetes.io/os

種類: ラベル

例: `kubernetes.io/os: "linux"`

使用対象: Node、Pod

ノードの場合、kubeletはGoで定義されている`runtime.GOOS`をこのラベルの値に設定します。
これは、クラスター内で複数のオペレーティングシステムが混在している場合(例えば、LinuxノードとWindowsノードが混在している場合)に便利です。

このラベルはPodにも設定できます。
Kubernetesではこのラベルに任意の値を設定できますが、使用する場合は、そのPodが実際に動作するオペレーティングシステムに対応するGoの`runtime.GOOS`文字列を設定するべきです。

Podの`kubernetes.io/os`ラベルの値がNodeのラベルの値と一致しない場合、そのノードのkubeletはPodを受け入れません。
ただし、kube-schedulerはこの一致を考慮しません。
また、PodのOSを指定した場合、そのOSがkubeletの動作しているノードのオペレーティングシステムと異なると、kubeletはそのPodの実行を拒否します。
詳細については、[PodのOS](/docs/concepts/workloads/pods/#pod-os)を参照してください。

### kubernetes.io/metadata.name

種類: ラベル

例: `kubernetes.io/metadata.name: "mynamespace"`

使用対象: Namespace

Kubernetes APIサーバー({{< glossary_tooltip text="コントロールプレーン" term_id="control-plane" >}}の一部)は、すべての名前空間にこのラベルを設定します。
ラベルの値には名前空間の名前が設定されます。
このラベルの値は変更できません。

ラベル{{< glossary_tooltip text="セレクター" term_id="selector" >}}で特定の名前空間を対象にする場合に便利です。

### kubernetes.io/limit-ranger

種類: アノテーション

例: `kubernetes.io/limit-ranger: "LimitRanger plugin set: cpu, memory request for container nginx; cpu, memory limit for container nginx"`

使用対象: Pod

Kubernetesはデフォルトではリソースの上限を設けません。
つまり、上限を明示的に定義しない限り、コンテナはCPUとメモリーを無制限に消費できます。
Podのデフォルトのリクエストや上限を定義できます。
これには、対象の名前空間にLimitRangeを作成します。
LimitRangeを定義した後にデプロイされたPodには、この上限が適用されます。
`kubernetes.io/limit-ranger`アノテーションは、Podにリソースのデフォルト値が指定され、正常に適用されたことを記録します。
詳細については、[LimitRange](/docs/concepts/policy/limit-range)を参照してください。

### kubernetes.io/config.hash

種類: アノテーション

例: `kubernetes.io/config.hash: "df7cc47f8477b6b1226d7d23a904867b"`

使用対象: Pod

kubeletは、指定されたマニフェストに基づいて静的Podを作成する際、その静的Podにこのアノテーションを付けます。
アノテーションの値はPodのUIDです。
kubeletは、Podがそのノードにスケジュールされた場合と同様に、`.spec.nodeName`にも現在のノード名を設定します。

### kubernetes.io/config.mirror

種類: アノテーション

例: `kubernetes.io/config.mirror: "df7cc47f8477b6b1226d7d23a904867b"`

使用対象: Pod

kubeletがノード上に作成した静的Podに対して、APIサーバー上に{{< glossary_tooltip text="ミラーPod" term_id="mirror-pod" >}}が作成されます。
kubeletは、このPodが実際にはミラーPodであることを示すアノテーションを追加します。
アノテーションの値は、PodのUIDである[`kubernetes.io/config.hash`](#kubernetes-io-config-hash)アノテーションからコピーされます。

このアノテーションが設定されたPodを更新する際、アノテーションを変更または削除することはできません。
このアノテーションが付いていないPodの場合、Podの更新時に追加することはできません。

### kubernetes.io/config.source

種類: アノテーション

例: `kubernetes.io/config.source: "file"`

使用対象: Pod

このアノテーションは、Podの取得元を示すためにkubeletが追加します。
静的Podの場合、Podマニフェストの場所に応じて、アノテーションの値は`file`または`http`となります。
APIサーバー上で作成され、現在のノードにスケジュールされたPodの場合、アノテーションの値は`api`です。

### kubernetes.io/config.seen

種類: アノテーション

例: `kubernetes.io/config.seen: "2023-10-27T04:04:56.011314488Z"`

使用対象: Pod

kubeletは、Podを初めて認識した際に、RFC3339形式の現在のタイムスタンプを値として、このアノテーションをPodに追加することがあります。

### addonmanager.kubernetes.io/mode

種類: ラベル

例: `addonmanager.kubernetes.io/mode: "Reconcile"`

使用対象: すべてのオブジェクト

アドオンの管理方法を指定するには、`addonmanager.kubernetes.io/mode`ラベルを使用できます。
このラベルには、`Reconcile`、`EnsureExists`、`Ignore`の3つの値のいずれかを設定できます。

- `Reconcile`: アドオンのリソースを期待する状態に定期的に調整。
  差異がある場合、アドオンマネージャーは必要に応じてリソースを再作成、再設定、または削除します。
  ラベルを指定しない場合のデフォルトのモードです。
- `EnsureExists`: アドオンのリソースの存在のみを確認し、作成後は変更しない。
  その名前のリソースのインスタンスが存在しない場合、アドオンマネージャーがリソースを作成または再作成します。
- `Ignore`: アドオンのリソースを無視。
  このモードは、アドオンマネージャーと互換性がないアドオンや、別のコントローラーが管理するアドオンに便利です。

詳細については、[Addon-manager](https://github.com/kubernetes/kubernetes/blob/master/cluster/addons/addon-manager/README.md)を参照してください。

### beta.kubernetes.io/arch (非推奨) {#beta-kubernetes-io-arch-deprecated}

種類: ラベル

このラベルは非推奨です。
代わりに[`kubernetes.io/arch`](#kubernetes-io-arch)を使用してください。

### beta.kubernetes.io/os (非推奨) {#beta-kubernetes-io-os-deprecated}

種類: ラベル

このラベルは非推奨です。
代わりに[`kubernetes.io/os`](#kubernetes-io-os)を使用してください。

### kube-aggregator.kubernetes.io/automanaged {#kube-aggregator-kubernetesio-automanaged}

種類: ラベル

例: `kube-aggregator.kubernetes.io/automanaged: "onstart"`

使用対象: APIService

`kube-apiserver`は、APIサーバーが自動的に作成したすべてのAPIServiceオブジェクトにこのラベルを設定します。
このラベルは、コントロールプレーンがそのAPIServiceをどのように管理するべきかを示します。
このラベルを自身で追加、変更、または削除するべきではありません。

{{< note >}}
自動管理されるAPIServiceオブジェクトは、そのAPIServiceのAPIグループとバージョンに対応する組み込みAPIまたはカスタムリソースAPIが存在しない場合、kube-apiserverによって削除されます。
{{< /note >}}

次の2つの値を設定できます:

- `onstart`: APIサーバーの起動時にのみAPIServiceを調整
- `true`: APIサーバーがこのAPIServiceを継続的に調整

### service.alpha.kubernetes.io/tolerate-unready-endpoints (非推奨) {#service-alpha-kubernetes-io-tolerate-unready-endpoints-deprecated}

種類: アノテーション

使用対象: Service

このアノテーションは以前、準備ができていないPodに対してEndpointsコントローラーがEndpointsを作成するべきであることを示すために使用されていました。
Kubernetes 1.11以降、この機能の推奨APIは{{< glossary_tooltip term_id="service" >}}の`.publishNotReadyAddresses`フィールドです。
このアノテーションはKubernetes {{< skew currentVersion >}}では効果がありません。

### autoscaling.alpha.kubernetes.io/behavior (非推奨) {#autoscaling-alpha-kubernetes-io-behavior}

種類: アノテーション

使用対象: HorizontalPodAutoscaler

このアノテーションは、以前のKubernetesバージョンでHorizontalPodAutoscaler(HPA)のスケーリング動作を設定するために使用されていました。
安定化ウィンドウやスケーリングポリシーの設定を含め、HPAがPodをどのようにスケールアップまたはスケールダウンするべきかを指定できました。
現在サポートされているKubernetesのどのリリースでも、このアノテーションを設定しても効果はありません。

### kubernetes.io/hostname {#kubernetesiohostname}

種類: ラベル

例: `kubernetes.io/hostname: "ip-172-20-114-199.ec2.internal"`

使用対象: Node

kubeletは、このラベルにノードのホスト名を設定します。
`kubelet`に`--hostname-override`フラグを渡すことで、「実際の」ホスト名とは異なる値に変更できます。

このラベルは、トポロジー階層の一部としても使用されます。
詳細については、[topology.kubernetes.io/zone](#topologykubernetesiozone)を参照してください。

### kubernetes.io/change-cause {#change-cause}

種類: アノテーション

例: `kubernetes.io/change-cause: "kubectl edit --record deployment foo"`

使用対象: すべてのオブジェクト

このアノテーションは、変更が行われた理由を可能な限り推定したものです。

オブジェクトを変更する可能性がある`kubectl`コマンドに`--record`を追加した際に設定されます。

### kubernetes.io/description {#description}

種類: アノテーション

例: `kubernetes.io/description: "Description of K8s object."`

使用対象: すべてのオブジェクト

このアノテーションは、特定のオブジェクトの具体的な動作を説明するために使用されます。

### kubernetes.io/enforce-mountable-secrets (非推奨) {#enforce-mountable-secrets}

種類: アノテーション

例: `kubernetes.io/enforce-mountable-secrets: "true"`

使用対象: ServiceAccount

{{< note >}}
`kubernetes.io/enforce-mountable-secrets`はKubernetes v1.32以降非推奨です。
マウントされるSecretへのアクセスを分離するには、別々の名前空間を使用してください。
{{< /note >}}

このアノテーションを有効にするには、値を**true**にする必要があります。
このアノテーションを"true"に設定すると、KubernetesはこのServiceAccountとして動作するPodに対して、次のルールを強制します:

1. ボリュームとしてマウントするSecretを、ServiceAccountの`secrets`フィールドに記載すること。
1. コンテナ(サイドカーコンテナとinitコンテナを含む)の`envFrom`で参照するSecretも、ServiceAccountのsecretsフィールドに記載すること。
   Pod内のいずれかのコンテナがServiceAccountの`secrets`フィールドに記載されていないSecretを参照すると、その参照が`optional`とされていてもPodは起動に失敗し、ルールに従っていないSecretの参照を示すエラーが生成されます。
1. Podの`imagePullSecrets`で参照するSecretを、ServiceAccountの`imagePullSecrets`フィールドに含めること。
   含まれていない場合、Podは起動に失敗し、ルールに従っていないイメージプル用Secretの参照を示すエラーが生成されます。

これらのルールは、Podを作成または更新する際に確認されます。
Podがルールに従っていない場合は起動せず、エラーメッセージが表示されます。
Podがすでに実行中の場合、`kubernetes.io/enforce-mountable-secrets`アノテーションをtrueに変更したり、関連付けられたServiceAccountを編集してPodが使用中のSecretへの参照を削除したりしても、Podは実行を継続します。

### node.alpha.kubernetes.io/ttl (非推奨) {#node-alpha-kubernetes-io-ttl-deprecated}

種類: ラベル

例: `node.alpha.kubernetes.io/ttl: "0"`

使用対象: Node

このラベルは、過去に一部のツール(minikubeなど)でノードの有効期間を設定するために使用されていました。
このラベルは非推奨であり、新しいデプロイメントでは使用するべきではありません。

{{< note >}}
このラベルは非推奨であり、現在のKubernetesバージョンでは効果がありません。
古いツールでは、後方互換性のために引き続き設定される場合があります。
{{< /note >}}

### node.kubernetes.io/exclude-from-external-load-balancers

種類: ラベル

例: `node.kubernetes.io/exclude-from-external-load-balancers`

使用対象: Node

特定のワーカーノードにラベルを追加して、外部ロードバランサーが使用するバックエンドサーバーの一覧から除外できます。
次のコマンドを使用すると、バックエンドセット内のバックエンドサーバーの一覧からワーカーノードを除外できます:

```shell
kubectl label nodes <node-name> node.kubernetes.io/exclude-from-external-load-balancers=true
```

### controller.kubernetes.io/pod-deletion-cost {#pod-deletion-cost}

種類: アノテーション

例: `controller.kubernetes.io/pod-deletion-cost: "10"`

使用対象: Pod

このアノテーションは、ユーザーがReplicaSetのスケールダウンの順序に影響を与えられるようにする[Pod削除コスト](/docs/concepts/workloads/controllers/replicaset/#pod-deletion-cost)を設定するために使用されます。
アノテーションの値は`int32`型として解析されます。

### cluster-autoscaler.kubernetes.io/enable-ds-eviction

種類: アノテーション

例: `cluster-autoscaler.kubernetes.io/enable-ds-eviction: "true"`

使用対象: Pod

このアノテーションは、ClusterAutoscalerがDaemonSetのPodを退避させるかどうかを制御します。
DaemonSetのマニフェスト内で、DaemonSetのPodにこのアノテーションを指定する必要があります。
このアノテーションを`"true"`に設定すると、通常は他のルールで禁止される場合でも、ClusterAutoscalerによるDaemonSetのPodの退避が許可されます。
ClusterAutoscalerによるDaemonSetのPodの退避を禁止するには、重要なDaemonSetのPodでこのアノテーションを`"false"`に設定できます。
このアノテーションを設定しない場合、ClusterAutoscalerは全体の動作に従います(つまり、その設定に基づいてDaemonSetを退避させます)。

{{< note >}}
このアノテーションはDaemonSetのPodにのみ影響します。
{{< /note >}}

### kubernetes.io/ingress-bandwidth

種類: アノテーション

例: `kubernetes.io/ingress-bandwidth: 10M`

使用対象: Pod

PodにQuality of Serviceのトラフィックシェーピングを適用し、利用可能な帯域幅を実際に制限できます。
Podへの受信トラフィックは、キューに入れたパケットをシェーピングすることで、データを効率的に処理します。
Podの帯域幅を制限するには、オブジェクト定義のJSONファイルを作成し、`kubernetes.io/ingress-bandwidth`アノテーションでデータの通信速度を指定します。
受信速度を指定する単位はビット毎秒で、[Quantity](/docs/reference/kubernetes-api/common-definitions/quantity/)として表します。
例えば、`10M`は毎秒10メガビットを意味します。

{{< note >}}
受信トラフィックのシェーピングアノテーションは実験的な機能です。
トラフィックシェーピングのサポートを有効にするには、CNI設定ファイル(デフォルトでは`/etc/cni/net.d`)に`bandwidth`プラグインを追加し、そのバイナリがCNIのバイナリディレクトリ(デフォルトでは`/opt/cni/bin`)に含まれていることを確認する必要があります。
{{< /note >}}

### kubernetes.io/egress-bandwidth

種類: アノテーション

例: `kubernetes.io/egress-bandwidth: 10M`

使用対象: Pod

Podからの送信トラフィックはポリシングによって処理され、設定した速度を超えるパケットは単純に破棄されます。
あるPodに設定した制限は、他のPodの帯域幅には影響しません。
Podの帯域幅を制限するには、オブジェクト定義のJSONファイルを作成し、`kubernetes.io/egress-bandwidth`アノテーションでデータの通信速度を指定します。
送信速度を指定する単位はビット毎秒で、[Quantity](/docs/reference/kubernetes-api/common-definitions/quantity/)として表します。
例えば、`10M`は毎秒10メガビットを意味します。

{{< note >}}
送信トラフィックのシェーピングアノテーションは実験的な機能です。
トラフィックシェーピングのサポートを有効にするには、CNI設定ファイル(デフォルトでは`/etc/cni/net.d`)に`bandwidth`プラグインを追加し、そのバイナリがCNIのバイナリディレクトリ(デフォルトでは`/opt/cni/bin`)に含まれていることを確認する必要があります。
{{< /note >}}

### beta.kubernetes.io/instance-type (非推奨) {#beta-kubernetes-io-instance-type-deprecated}

種類: ラベル

{{< note >}}
v1.17以降、このラベルは非推奨となり、[node.kubernetes.io/instance-type](#nodekubernetesioinstance-type)に置き換えられています。
{{< /note >}}

### node.kubernetes.io/instance-type {#nodekubernetesioinstance-type}

種類: ラベル

例: `node.kubernetes.io/instance-type: "m3.medium"`

使用対象: Node

kubeletは、クラウドプロバイダーが定義するインスタンスタイプをこのラベルの値に設定します。
クラウドプロバイダーを使用している場合にのみ設定されます。
この設定は特定のワークロードを特定のインスタンスタイプに配置したい場合に便利ですが、通常はKubernetesスケジューラーによるリソースに基づくスケジューリングに任せることを推奨します。
インスタンスタイプではなく、特性に基づいたスケジューリングを目指すべきです(例えば、`g2.2xlarge`を要求する代わりにGPUを要求します)。

### failure-domain.beta.kubernetes.io/region (非推奨) {#failure-domainbetakubernetesioregion}

種類: ラベル

{{< note >}}
v1.17以降、このラベルは非推奨となり、[topology.kubernetes.io/region](#topologykubernetesioregion)に置き換えられています。
{{< /note >}}

### failure-domain.beta.kubernetes.io/zone (非推奨) {#failure-domainbetakubernetesiozone}

種類: ラベル

{{< note >}}
v1.17以降、このラベルは非推奨となり、[topology.kubernetes.io/zone](#topologykubernetesiozone)に置き換えられています。
{{< /note >}}

### pv.kubernetes.io/bind-completed {#pv-kubernetesiobind-completed}

種類: アノテーション

例: `pv.kubernetes.io/bind-completed: "yes"`

使用対象: PersistentVolumeClaim

PersistentVolumeClaim(PVC)にこのアノテーションが設定されている場合、PVCのライフサイクルが初期のバインディング設定を通過したことを示します。
この情報が存在すると、コントロールプレーンによるPVCオブジェクトの状態の解釈が変わります。
Kubernetesにとって、このアノテーションの値は重要ではありません。

### pv.kubernetes.io/bound-by-controller {#pv-kubernetesioboundby-controller}

種類: アノテーション

例: `pv.kubernetes.io/bound-by-controller: "yes"`

使用対象: PersistentVolume、PersistentVolumeClaim

PersistentVolumeまたはPersistentVolumeClaimにこのアノテーションが設定されている場合、ストレージのバインディング(PersistentVolume → PersistentVolumeClaim、またはPersistentVolumeClaim → PersistentVolume)が{{< glossary_tooltip text="コントローラー" term_id="controller" >}}によって設定されたことを示します。
ストレージのバインディングが存在するにもかかわらず、このアノテーションが設定されていない場合、そのバインディングが手動で行われたことを意味します。
このアノテーションの値は重要ではありません。

### pv.kubernetes.io/provisioned-by {#pv-kubernetesiodynamically-provisioned}

種類: アノテーション

例: `pv.kubernetes.io/provisioned-by: "kubernetes.io/rbd"`

使用対象: PersistentVolume

このアノテーションは、Kubernetesによって動的にプロビジョニングされたPersistentVolume(PV)に追加されます。
値は、そのボリュームを作成したボリュームプラグインの名前です。
ユーザーにはPVの作成元を示し、Kubernetesには判断の際に動的にプロビジョニングされたPVを識別する手段を提供します。

### pv.kubernetes.io/migrated-to {#pv-kubernetesio-migratedto}

種類: アノテーション

例: `pv.kubernetes.io/migrated-to: pd.csi.storage.gke.io`

使用対象: PersistentVolume、PersistentVolumeClaim

このアノテーションは、`CSIMigration`フィーチャーゲートを通じて、対応するCSIドライバーが動的にプロビジョニングまたは削除することになっているPersistentVolume(PV)とPersistentVolumeClaim(PVC)に追加されます。
このアノテーションが設定されると、Kubernetesコンポーネントは処理を控え、`external-provisioner`がオブジェクトを処理します。

### statefulset.kubernetes.io/pod-name {#statefulsetkubernetesiopod-name}

種類: ラベル

例: `statefulset.kubernetes.io/pod-name: "mystatefulset-7"`

使用対象: Pod

StatefulSetコントローラーがStatefulSet用のPodを作成すると、コントロールプレーンはそのPodにこのラベルを設定します。
ラベルの値は、作成されるPodの名前です。

詳細については、StatefulSetの[Pod名ラベル](/docs/concepts/workloads/controllers/statefulset/#pod-name-label)を参照してください。

### scheduler.alpha.kubernetes.io/node-selector {#schedulerkubernetesnode-selector}

種類: アノテーション

例: `scheduler.alpha.kubernetes.io/node-selector: "name-of-node-selector"`

使用対象: Namespace

[PodNodeSelector](/docs/reference/access-authn-authz/admission-controllers/#podnodeselector)は、このアノテーションキーを使用して、名前空間内のPodにノードセレクターを割り当てます。

### topology.kubernetes.io/region {#topologykubernetesioregion}

種類: ラベル

例: `topology.kubernetes.io/region: "us-east-1"`

使用対象: Node、PersistentVolume

[topology.kubernetes.io/zone](#topologykubernetesiozone)を参照してください。

### topology.kubernetes.io/zone {#topologykubernetesiozone}

種類: ラベル

例: `topology.kubernetes.io/zone: "us-east-1c"`

使用対象: Node、PersistentVolume

**Nodeの場合**: `kubelet`または外部の`cloud-controller-manager`が、クラウドプロバイダーからの情報をこのラベルの値に設定します。
クラウドプロバイダーを使用している場合にのみ設定されます。
ただし、自身のトポロジーに適している場合は、ノードにこのラベルを設定することを検討できます。

**PersistentVolumeの場合**: トポロジーに対応したボリュームプロビジョナーが、`PersistentVolume`にノードアフィニティの制約を自動的に設定します。

ゾーンは論理的な障害ドメインを表します。
可用性を高めるため、Kubernetesクラスターは複数のゾーンにまたがることが一般的です。
ゾーンの厳密な定義はインフラストラクチャの実装に委ねられていますが、一般的な特性として、ゾーン内のネットワーク遅延が非常に小さいこと、ゾーン内のネットワーク通信に費用がかからないこと、他のゾーンと障害が独立していることが挙げられます。
例えば、同じゾーン内のノードはネットワークスイッチを共有する場合がありますが、異なるゾーンのノードは共有するべきではありません。

リージョンは、1つ以上のゾーンで構成される、より大きなドメインを表します。
Kubernetesクラスターが複数のリージョンにまたがることは一般的ではありません。
ゾーンやリージョンの厳密な定義はインフラストラクチャの実装に委ねられていますが、リージョンの一般的な特性として、リージョン間のネットワーク遅延がリージョン内よりも大きいこと、リージョン間のネットワーク通信に費用がかかること、他のゾーンやリージョンと障害が独立していることが挙げられます。
例えば、同じリージョン内のノードは電源設備(UPSや発電機など)を共有する場合がありますが、異なるリージョンのノードは通常共有しません。

Kubernetesは、ゾーンとリージョンの構造について、いくつかの前提を置いています:

1. リージョンとゾーンは階層構造であり、ゾーンはリージョンに厳密に含まれ、1つのゾーンが2つのリージョンに属することはない
2. ゾーン名はリージョンをまたいで一意である。
   例えば、リージョン"africa-east-1"は、ゾーン"africa-east-1a"と"africa-east-1b"で構成される場合があります。

トポロジーのラベルは変化しないと想定しても問題ないはずです。
厳密にはラベルは変更できますが、その利用者は、ノードが破棄されて再作成されることなくゾーン間を移動することはないと想定できます。

Kubernetesは、この情報をさまざまな方法で使用できます。
例えば、単一ゾーンのクラスターでは、スケジューラーはReplicaSet内のPodをノード間で自動的に分散しようとします(ノード障害の影響を減らすためです。[kubernetes.io/hostname](#kubernetesiohostname)を参照してください)。
複数ゾーンのクラスターでは、この分散動作はゾーンにも適用されます(ゾーン障害の影響を減らすためです)。
これは*SelectorSpreadPriority*によって実現されます。

*SelectorSpreadPriority*はベストエフォートの配置です。
クラスター内のゾーンが均質でない場合(例えば、ノード数、ノードの種類、Podのリソース要件が異なる場合)、この配置ではPodをゾーン間で均等に分散できないことがあります。
必要であれば、均質なゾーン(ノード数と種類が同じ)を使用して、不均等に分散する可能性を減らせます。

スケジューラーは、*VolumeZonePredicate*の述語を通じて、特定のボリュームを要求するPodが、そのボリュームと同じゾーンにのみ配置されることも保証します。
ボリュームはゾーンをまたいでアタッチできません。

`PersistentVolumeLabel`がPersistentVolumeへの自動ラベル付けをサポートしていない場合、手動でラベルを追加すること(または`PersistentVolumeLabel`のサポートを追加すること)を検討するべきです。
`PersistentVolumeLabel`を使用すると、スケジューラーはPodが異なるゾーンのボリュームをマウントすることを防ぎます。
インフラストラクチャにこの制約がない場合、ボリュームにゾーンのラベルを追加する必要はありません。

### volume.alpha.kubernetes.io/node-affinity (非推奨) {#volume-alpha-kubernetes-io-node-affinity}

種類: アノテーション

使用対象: PersistentVolume

このアノテーションは、PersistentVolumeのノードアフィニティのルールを、JSONにシリアライズされた`NodeAffinity`オブジェクトとして保存していました。
スケジューラーは、ボリュームにアクセスできるノードを制限するためにこれらのルールを使用していました。

このアノテーションはKubernetes v1.10以降非推奨です。
代わりにPersistentVolumeのspecの[`nodeAffinity`フィールド](/docs/concepts/storage/persistent-volumes/#node-affinity)を使用してください。

### volume.beta.kubernetes.io/storage-provisioner (非推奨) {#volume-beta-kubernetes-io-storage-provisioner-deprecated}

種類: アノテーション

例: `volume.beta.kubernetes.io/storage-provisioner: "k8s.io/minikube-hostpath"`

使用対象: PersistentVolumeClaim

このアノテーションはv1.23以降非推奨です。
[volume.kubernetes.io/storage-provisioner](#volume-kubernetes-io-storage-provisioner)を参照してください。

### volume.beta.kubernetes.io/storage-class (非推奨) {#volume-beta-kubernetes-io-storage-class-deprecated}

種類: アノテーション

例: `volume.beta.kubernetes.io/storage-class: "example-class"`

使用対象: PersistentVolume、PersistentVolumeClaim

このアノテーションは、PersistentVolume(PV)またはPersistentVolumeClaim(PVC)で[StorageClass](/docs/concepts/storage/storage-classes/)の名前を指定するために使用できます。
`storageClassName`属性と`volume.beta.kubernetes.io/storage-class`アノテーションの両方が指定されている場合、`volume.beta.kubernetes.io/storage-class`アノテーションが`storageClassName`属性よりも優先されます。

このアノテーションは非推奨です。
代わりに、PersistentVolumeClaimまたはPersistentVolumeの[`storageClassName`フィールド](/docs/concepts/storage/persistent-volumes/#class)を設定してください。

### volume.beta.kubernetes.io/mount-options (非推奨) {#mount-options}

種類: アノテーション

例: `volume.beta.kubernetes.io/mount-options: "ro,soft"`

使用対象: PersistentVolume

Kubernetes管理者は、PersistentVolumeをノードにマウントする際の追加の[マウントオプション](/docs/concepts/storage/persistent-volumes/#mount-options)を指定できます。

### volume.kubernetes.io/storage-provisioner  {#volume-kubernetes-io-storage-provisioner}

種類: アノテーション

使用対象: PersistentVolumeClaim

このアノテーションは、動的にプロビジョニングされる予定のPVCに追加されます。
値は、そのPVCのボリュームをプロビジョニングする予定のボリュームプラグインの名前です。

### volume.kubernetes.io/selected-node

種類: アノテーション

使用対象: PersistentVolumeClaim

このアノテーションは、スケジューラーを契機として動的にプロビジョニングされるPVCに追加されます。
値は、選択されたノードの名前です。

### volumes.kubernetes.io/controller-managed-attach-detach

種類: アノテーション

使用対象: Node

ノードに`volumes.kubernetes.io/controller-managed-attach-detach`アノテーションがある場合、そのストレージのアタッチとデタッチの操作は、_volume attach/detach_ {{< glossary_tooltip text="コントローラー" term_id="controller" >}}によって管理されています。

アノテーションの値は重要ではありません。

### node.kubernetes.io/windows-build {#nodekubernetesiowindows-build}

種類: ラベル

例: `node.kubernetes.io/windows-build: "10.0.17763"`

使用対象: Node

kubeletがMicrosoft Windows上で動作している場合、使用中のWindows Serverのバージョンを記録するため、Nodeに自動的にラベルを付けます。

ラベルの値は"MajorVersion.MinorVersion.BuildNumber"の形式です。

### storage.alpha.kubernetes.io/migrated-plugins {#storagealphakubernetesiomigrated-plugins}

種類: アノテーション

例:`storage.alpha.kubernetes.io/migrated-plugins: "kubernetes.io/cinder"`

使用対象: CSINode(拡張API)

このアノテーションは、CSIDriverがインストールされたノードに対応するCSINodeオブジェクトに自動的に追加されます。
このアノテーションは、移行されたプラグインのツリー内プラグイン名を示します。
値は、クラスターのツリー内クラウドプロバイダーのストレージタイプによって異なります。

例えば、ツリー内クラウドプロバイダーのストレージタイプが`CSIMigrationvSphere`である場合、そのノードのCSINodeインスタンスを次のように更新する必要があります:
`storage.alpha.kubernetes.io/migrated-plugins: "kubernetes.io/vsphere-volume"`

### service.kubernetes.io/headless {#servicekubernetesioheadless}

種類: ラベル

例: `service.kubernetes.io/headless: ""`

使用対象: EndpointSlice、Endpoints

所有元の{{< glossary_tooltip term_id="service" >}}がヘッドレスの場合、{{< glossary_tooltip term_id="control-plane" text="コントロールプレーン" >}}は、EndpointSliceとEndpointsオブジェクトにこの{{< glossary_tooltip term_id="label" text="ラベル" >}}を追加します(サービスプロキシに、これらのエンドポイントを無視できることを示すヒントとして使用します)。
詳細については、[ヘッドレスService](/docs/concepts/services-networking/service/#headless-services)を参照してください。

### service.kubernetes.io/topology-aware-hints (非推奨) {#servicekubernetesiotopology-aware-hints}

例: `service.kubernetes.io/topology-aware-hints: "Auto"`

使用対象: Service

これは、同じ機能を持つ[`service.kubernetes.io/topology-mode`](#service-kubernetes-io-topology-mode)アノテーションの非推奨の別名です。

### service.kubernetes.io/topology-mode

種類: アノテーション

例: `service.kubernetes.io/topology-mode: Auto`

使用対象: Service

このアノテーションは、Serviceによるネットワークトポロジーの扱いを定義する手段を提供します。
例えば、Kubernetesがクライアントとサーバー間のトラフィックを単一のトポロジーゾーン内に留めることを優先するよう、Serviceを設定できます。
これにより、コストの削減やネットワーク性能の向上につながる場合があります。

詳細については、[トポロジーを考慮したルーティング](/docs/concepts/services-networking/topology-aware-routing/)を参照してください。

### kubernetes.io/service-name {#kubernetesioservice-name}

種類: ラベル

例: `kubernetes.io/service-name: "my-website"`

使用対象: EndpointSlice

Kubernetesは、このラベルを使用して[EndpointSlice](/docs/concepts/services-networking/endpoint-slices/)と[Service](/docs/concepts/services-networking/service/)を関連付けます。

このラベルは、EndpointSliceが対応するServiceの{{< glossary_tooltip term_id="name" text="名前">}}を記録します。
すべてのEndpointSliceは、このラベルを関連付けられたServiceの名前に設定するべきです。

### kubernetes.io/service-account.name

種類: アノテーション

例: `kubernetes.io/service-account.name: "sa-name"`

使用対象: Secret

このアノテーションは、`kubernetes.io/service-account-token`型のSecretに保存されているトークンが表すServiceAccountの{{< glossary_tooltip term_id="name" text="名前">}}を記録します。

### kubernetes.io/service-account.uid

種類: アノテーション

例: `kubernetes.io/service-account.uid: da68f9c6-9d26-11e7-b84e-002dc52800da`

使用対象: Secret

このアノテーションは、`kubernetes.io/service-account-token`型のSecretに保存されているトークンが表すServiceAccountの{{< glossary_tooltip term_id="uid" text="一意のID" >}}を記録します。

### kubernetes.io/legacy-token-last-used

種類: ラベル

例: `kubernetes.io/legacy-token-last-used: 2022-10-24`

使用対象: Secret

コントロールプレーンは、`kubernetes.io/service-account-token`型のSecretにのみこのラベルを追加します。
ラベルの値は、クライアントがサービスアカウントトークンを使用して認証したリクエストを、コントロールプレーンが最後に受け取った日付(ISO 8601形式、UTCタイムゾーン)を記録します。

レガシートークンが最後に使用されたのが、この機能(Kubernetes v1.26で追加)がクラスターに導入される前である場合、このラベルは設定されません。

### kubernetes.io/legacy-token-invalid-since

種類: ラベル

例: `kubernetes.io/legacy-token-invalid-since: 2023-10-27`

使用対象: Secret

コントロールプレーンは、`kubernetes.io/service-account-token`型の自動生成されたSecretに、このラベルを自動的に追加します。
このラベルは、Secretに基づくトークンが認証に使用できないことを示します。
ラベルの値は、自動生成されたSecretが指定した期間(デフォルトは1年)使用されていないことを、コントロールプレーンが検出した日付(ISO 8601形式、UTCタイムゾーン)を記録します。

### endpoints.kubernetes.io/managed-by (非推奨) {#endpoints-kubernetes-io-managed-by}

種類: ラベル

例: `endpoints.kubernetes.io/managed-by: endpoint-controller`

使用対象: Endpoints

このラベルは、ユーザーや外部コントローラーではなくKubernetesが作成したEndpointsオブジェクトを識別するために、内部で使用されます。

{{< note >}}
[Endpoints](/docs/reference/kubernetes-api/service-resources/endpoints-v1/) APIは非推奨となり、[EndpointSlice](/docs/reference/kubernetes-api/service-resources/endpoint-slice-v1/)に置き換えられています。
{{< /note >}}

### endpointslice.kubernetes.io/managed-by {#endpointslicekubernetesiomanaged-by}

種類: ラベル

例: `endpointslice.kubernetes.io/managed-by: endpointslice-controller.k8s.io`

使用対象: EndpointSlice

このラベルは、EndpointSliceを管理するコントローラーやエンティティを示すために使用されます。
同じクラスター内で、異なるEndpointSliceオブジェクトを異なるコントローラーやエンティティが管理できるようにすることを目的としています。
値`endpointslice-controller.k8s.io`は、{{< glossary_tooltip text="セレクター" term_id="selector" >}}を持つServiceのためにKubernetesが自動的に作成したEndpointSliceオブジェクトを示します。

### endpointslice.kubernetes.io/skip-mirror {#endpointslicekubernetesioskip-mirror}

種類: ラベル

例: `endpointslice.kubernetes.io/skip-mirror: "true"`

使用対象: Endpoints

Endpointsリソースでこのラベルを`"true"`に設定すると、EndpointSliceMirroringコントローラーがそのリソースをEndpointSliceにミラーリングするべきでないことを示せます。

### service.kubernetes.io/service-proxy-name {#servicekubernetesioservice-proxy-name}

種類: ラベル

例: `service.kubernetes.io/service-proxy-name: "foo-bar"`

使用対象: Service

このラベルに値を設定すると、kube-proxyはこのServiceをプロキシ処理の対象から除外します。
これにより、このServiceで別のプロキシ実装を使用できます(例えば、独自の方法でnftablesを管理するDaemonSetを実行できます)。
このフィールドを使用すると、複数の代替プロキシ実装を同時に動作させることができます。
例えば、各代替プロキシ実装に固有の値を使用し、それぞれが対応するServiceを担当するようにできます。

### experimental.windows.kubernetes.io/isolation-type (非推奨) {#experimental-windows-kubernetes-io-isolation-type}

種類: アノテーション

例: `experimental.windows.kubernetes.io/isolation-type: "hyperv"`

使用対象: Pod

このアノテーションは、Hyper-V分離を使用してWindowsコンテナを実行するために使用されます。

{{< note >}}
v1.20以降、このアノテーションは非推奨です。
実験的なHyper-Vサポートは1.21で削除されました。
{{< /note >}}

### gateway.networking.k8s.io/generator

種類: アノテーション

例: `gateway.networking.k8s.io/generator: "ingress2gateway"`

使用対象: Gateway、HTTPRoute、その他のGateway APIリソース

このアノテーションは、[Gateway API](/docs/concepts/services-networking/gateway/)リソースを自動生成するツールによって追加されます。
値は、そのリソースを作成したツール(例えば、`ingress2gateway`)を識別します。
このアノテーションは情報を提供するためだけのものであり、どのGateway API実装の動作にも影響しません。

### ingressclass.kubernetes.io/is-default-class

種類: アノテーション

例: `ingressclass.kubernetes.io/is-default-class: "true"`

使用対象: IngressClass

IngressClassリソースでこのアノテーションが`"true"`に設定されている場合、クラスを指定せずに作成した新しいIngressリソースには、このデフォルトクラスが割り当てられます。

### kubernetes.io/ingress.class (非推奨) {#kubernetes-io-ingress-class-deprecated}

種類: アノテーション

使用対象: Ingress

{{< note >}}
v1.18以降、このアノテーションは非推奨となり、`spec.ingressClassName`に置き換えられています。
{{< /note >}}

### kubernetes.io/cluster-service (非推奨) {#kubernetes-io-cluster-service}

種類: ラベル

例: `kubernetes.io/cluster-service: "true"`

使用対象: Service

このラベルの値がtrueの場合、そのServiceがクラスターにサービスを提供することを示します。
`kubectl cluster-info`を実行すると、このラベルがtrueに設定されたServiceが検索されます。

ただし、いずれのServiceでもこのラベルを設定することは非推奨です。

### storageclass.kubernetes.io/is-default-class

種類: アノテーション

例: `storageclass.kubernetes.io/is-default-class: "true"`

使用対象: StorageClass

1つのStorageClassリソースでこのアノテーションが`"true"`に設定されている場合、クラスを指定せずに作成した新しいPersistentVolumeClaimリソースには、このデフォルトクラスが割り当てられます。

### alpha.kubernetes.io/provided-node-ip (Alpha) {#alpha-kubernetes-io-provided-node-ip}

種類: アノテーション

例: `alpha.kubernetes.io/provided-node-ip: "10.0.0.1"`

使用対象: Node

kubeletは、設定されたIPv4アドレスやIPv6アドレスを示すために、Nodeにこのアノテーションを設定できます。

kubeletを`--cloud-provider`フラグに何らかの値を設定して起動すると(外部および従来のツリー内クラウドプロバイダーの両方を含みます)、コマンドラインフラグ(`--node-ip`)で設定されたIPアドレスを示すために、Nodeにこのアノテーションを設定します。
このIPアドレスは、cloud-controller-managerがクラウドプロバイダーに確認して、有効であることを検証します。

### batch.kubernetes.io/job-completion-index

種類: アノテーション、ラベル

例: `batch.kubernetes.io/job-completion-index: "3"`

使用対象: Pod

kube-controller-manager内のJobコントローラーは、Indexed[完了モード](/docs/concepts/workloads/controllers/job/#completion-mode)で作成されたPodに、これをラベルおよびアノテーションとして設定します。

Podの**ラベル**として追加するには、[PodIndexLabel](/docs/reference/command-line-tools-reference/feature-gates/)フィーチャーゲートを有効にする必要があります。
有効でない場合は、アノテーションのみとなります。

### batch.kubernetes.io/cronjob-scheduled-timestamp

種類: アノテーション

例: `batch.kubernetes.io/cronjob-scheduled-timestamp: "2016-05-19T03:00:00-07:00"`

使用対象: CronJobによって制御されるJobとPod

このアノテーションは、CronJobに属するJobの本来の(予定された)作成タイムスタンプを記録するために使用されます。
コントロールプレーンは、そのタイムスタンプをRFC3339形式で値に設定します。
Jobがタイムゾーンを指定されたCronJobに属している場合、タイムスタンプはそのタイムゾーンとなります。
それ以外の場合、タイムスタンプはcontroller-managerのローカル時刻となります。

### cronjob.kubernetes.io/instantiate {#cronjob-kubernetes-io-instantiate}

種類: アノテーション

例: `cronjob.kubernetes.io/instantiate: "manual"`

使用対象: Job

`kubectl create job`に`--from=cronjob/<cronjob-name>`フラグを指定し、既存のCronJobテンプレートからJobを手動で作成すると、`kubectl`は新しく作成したJobにこのアノテーションを設定します。
このアノテーションの値は常に`manual`です。
このアノテーションによって、ユーザーが必要に応じて作成したJobと、CronJobコントローラーが予定時刻に自動的に作成したJobを区別できます。

### kubectl.kubernetes.io/default-container

種類: アノテーション

例: `kubectl.kubernetes.io/default-container: "front-end-app"`

このアノテーションの値は、このPodのデフォルトのコンテナ名です。
例えば、`kubectl logs`や`kubectl exec`で`-c`または`--container`フラグを指定しない場合、このデフォルトコンテナが使用されます。

### kubectl.kubernetes.io/default-logs-container (非推奨) {#kubectl-kubernetes-io-default-logs-container-deprecated}

種類: アノテーション

例: `kubectl.kubernetes.io/default-logs-container: "front-end-app"`

このアノテーションの値は、このPodでログ取得時のデフォルトとなるコンテナ名です。
例えば、`kubectl logs`で`-c`または`--container`フラグを指定しない場合、このデフォルトコンテナが使用されます。

{{< note >}}
このアノテーションは非推奨です。
代わりに[`kubectl.kubernetes.io/default-container`](#kubectl-kubernetes-io-default-container)アノテーションを使用するべきです。
Kubernetes 1.25以降では、このアノテーションは無視されます。
{{< /note >}}

### kubectl.kubernetes.io/last-applied-configuration

種類: アノテーション

例: *次のスニペットを参照*
```yaml
    kubectl.kubernetes.io/last-applied-configuration: >
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"example","namespace":"default"},"spec":{"selector":{"matchLabels":{"app.kubernetes.io/name":foo}},"template":{"metadata":{"labels":{"app.kubernetes.io/name":"foo"}},"spec":{"containers":[{"image":"container-registry.example/foo-bar:1.42","name":"foo-bar","ports":[{"containerPort":42}]}]}}}}
```

使用対象: すべてのオブジェクト

kubectlコマンドラインツールは、変更を追跡する従来の仕組みとして、このアノテーションを使用します。
その仕組みは、[サーバーサイドApply](/docs/reference/using-api/server-side-apply/)に置き換えられています。

### kubectl.kubernetes.io/restartedAt {#kubectl-k8s-io-restart-at}

種類: アノテーション

例: `kubectl.kubernetes.io/restartedAt: "2024-06-21T17:27:41Z"`

使用対象: Deployment、ReplicaSet、StatefulSet、DaemonSet、Pod

このアノテーションには、新しいPodを強制的に作成するためにkubectlがロールアウトを開始した、リソース(Deployment、ReplicaSet、StatefulSet、DaemonSet)の直近の再起動時刻が含まれます。
`kubectl rollout restart <RESOURCE>`コマンドは、リソースのすべてのPodのテンプレートメタデータにこのアノテーションをパッチすることで、再起動を開始します。
上の例では、直近の再起動時刻は2024年6月21日17時27分41秒(UTC)と示されています。

このアノテーションが最新の更新日時を表すと想定するべきではありません。
最後に手動で開始したロールアウト以降に、別の変更が行われている可能性があります。

Podにこのアノテーションを手動で設定しても、何も起こりません。
再起動という副作用は、ワークロード管理とPodテンプレートの仕組みによって生じます。

### endpoints.kubernetes.io/over-capacity (非推奨) {#endpoints-kubernetes-io-over-capacity}

種類: アノテーション

例: `endpoints.kubernetes.io/over-capacity: truncated`

使用対象: Endpoints

関連付けられた{{< glossary_tooltip term_id="service" >}}のバックエンドエンドポイントが1000個を超えると、{{< glossary_tooltip text="コントロールプレーン" term_id="control-plane" >}}は[Endpoints](/docs/concepts/services-networking/service/#endpoints)オブジェクトにこのアノテーションを追加します。
このアノテーションは、Endpointsオブジェクトが容量を超えており、エンドポイント数が1000個に切り詰められていることを示します。

バックエンドエンドポイント数が1000個未満になると、コントロールプレーンはこのアノテーションを削除します。

{{< note >}}
[Endpoints](/docs/reference/kubernetes-api/service-resources/endpoints-v1/) APIは非推奨となり、[EndpointSlice](/docs/reference/kubernetes-api/service-resources/endpoint-slice-v1/)に置き換えられています。
Serviceは複数のEndpointSliceオブジェクトを持つことができます。
そのため、EndpointSliceでは切り詰める必要がありません。
{{< /note >}}

### endpoints.kubernetes.io/last-change-trigger-time (非推奨) {#endpoints-kubernetes-io-last-change-trigger-time}

種類: アノテーション

例: `endpoints.kubernetes.io/last-change-trigger-time: "2023-07-20T04:45:21Z"`

使用対象: Endpoints

このアノテーションは[Endpoints](/docs/concepts/services-networking/service/#endpoints)オブジェクトに設定され、タイムスタンプを表します(タイムスタンプはRFC 3339形式の日時文字列で保存されます。例えば、'2018-10-22T19:32:52.1Z'です)。
これは、Endpointsオブジェクトの変更を引き起こした、PodまたはServiceオブジェクトの最後の変更のタイムスタンプです。

{{< note >}}
[Endpoints](/docs/reference/kubernetes-api/service-resources/endpoints-v1/) APIは非推奨となり、[EndpointSlice](/docs/reference/kubernetes-api/service-resources/endpoint-slice-v1/)に置き換えられています。
{{< /note >}}

### control-plane.alpha.kubernetes.io/leader (非推奨) {#control-plane-alpha-kubernetes-io-leader}

種類: アノテーション

例: `control-plane.alpha.kubernetes.io/leader={"holderIdentity":"controller-0","leaseDurationSeconds":15,"acquireTime":"2023-01-19T13:12:57Z","renewTime":"2023-01-19T13:13:54Z","leaderTransitions":1}`

使用対象: Endpoints

{{< glossary_tooltip text="コントロールプレーン" term_id="control-plane" >}}は以前、Kubernetesコントロールプレーンのリーダー割り当てを調整するために、[Endpoints](/docs/concepts/services-networking/service/#endpoints)オブジェクトを使用していました。
このEndpointsオブジェクトには、次の情報を持つアノテーションが含まれていました:

- 現在のリーダー
- 現在のリーダーシップを取得した時刻
- リーダーシップのリース期間(秒)
- 現在のリース(現在のリーダーシップ)を更新するべき時刻
- 過去にリーダーシップが切り替わった回数

現在、Kubernetesは[Lease](/docs/concepts/architecture/leases/)を使用して、Kubernetesコントロールプレーンのリーダー割り当てを管理します。

### batch.kubernetes.io/job-tracking (非推奨) {#batch-kubernetes-io-job-tracking}

種類: アノテーション

例: `batch.kubernetes.io/job-tracking: ""`

使用対象: Job

以前は、Jobにこのアノテーションが存在することは、コントロールプレーンが[ファイナライザーを使用してJobのステータスを追跡している](/docs/concepts/workloads/controllers/job/#job-tracking-with-finalizers)ことを示していました。
Kubernetes v1.27以降、このアノテーションを追加または削除しても効果はありません。
すべてのJobはファイナライザーを使用して追跡されます。

### job-name (非推奨) {#job-name}

種類: ラベル

例: `job-name: "pi"`

使用対象: Jobによって制御されるJobとPod

{{< note >}}
Kubernetes 1.27以降、このラベルは非推奨です。
Kubernetes 1.27以降では、このラベルを無視し、プレフィックス付きの`job-name`ラベルを使用します。
{{< /note >}}

### controller-uid (非推奨) {#controller-uid}

種類: ラベル

例: `controller-uid: "$UID"`

使用対象: Jobによって制御されるJobとPod

{{< note >}}
Kubernetes 1.27以降、このラベルは非推奨です。
Kubernetes 1.27以降では、このラベルを無視し、プレフィックス付きの`controller-uid`ラベルを使用します。
{{< /note >}}

### batch.kubernetes.io/job-name {#batchkubernetesio-job-name}

種類: ラベル

例: `batch.kubernetes.io/job-name: "pi"`

使用対象: Jobによって制御されるJobとPod

このラベルは、Jobに対応するPodを、ユーザーがわかりやすい方法で取得するために使用されます。
`job-name`はJobの`name`に由来し、そのJobに対応するPodを簡単に取得できるようにします。

### batch.kubernetes.io/controller-uid {#batchkubernetesio-controller-uid}

種類: ラベル

例: `batch.kubernetes.io/controller-uid: "$UID"`

使用対象: Jobによって制御されるJobとPod

このラベルは、Jobに対応するすべてのPodをプログラムから取得するために使用されます。
`controller-uid`は`selector`フィールドに設定される一意の識別子であり、Jobコントローラーが対応するすべてのPodを取得できるようにします。

### scheduler.alpha.kubernetes.io/defaultTolerations {#scheduleralphakubernetesio-defaulttolerations}

種類: アノテーション

例: `scheduler.alpha.kubernetes.io/defaultTolerations: '[{"operator": "Equal", "value": "value1", "effect": "NoSchedule", "key": "dedicated-node"}]'`

使用対象: Namespace

このアノテーションを使用するには、[PodTolerationRestriction](/docs/reference/access-authn-authz/admission-controllers/#podtolerationrestriction)アドミッションコントローラーを有効にする必要があります。
このアノテーションキーによって名前空間にTolerationを割り当てることができ、その名前空間内で新しく作成されるすべてのPodに、そのTolerationが追加されます。

### scheduler.alpha.kubernetes.io/tolerationsWhitelist {#schedulerkubernetestolerations-whitelist}

種類: アノテーション

例: `scheduler.alpha.kubernetes.io/tolerationsWhitelist: '[{"operator": "Exists", "effect": "NoSchedule", "key": "dedicated-node"}]'`

使用対象: Namespace

このアノテーションは、Alpha段階の[PodTolerationRestriction](/docs/reference/access-authn-authz/admission-controllers/#podtolerationrestriction)アドミッションコントローラーが有効な場合にのみ有用です。
アノテーションの値は、付与先の名前空間で許可されるTolerationの一覧を定義するJSONドキュメントです。
Podを作成したり、そのTolerationを変更したりする際、APIサーバーはTolerationが許可リストに含まれているかを確認します。
確認が成功した場合にのみ、Podは受け入れられます。

### scheduler.alpha.kubernetes.io/preferAvoidPods (非推奨) {#scheduleralphakubernetesio-preferavoidpods}

種類: アノテーション

使用対象: Node

このアノテーションを使用するには、[NodePreferAvoidPodsスケジューリングプラグイン](/docs/reference/scheduling/config/#scheduling-plugins)を有効にする必要があります。
このプラグインはKubernetes 1.22以降非推奨です。
代わりに[TaintとToleration](/docs/concepts/scheduling-eviction/taint-and-toleration/)を使用してください。

### node.kubernetes.io/not-ready

種類: Taint

例: `node.kubernetes.io/not-ready: "NoExecute"`

使用対象: Node

Nodeコントローラーは、Nodeの健全性を監視して準備ができているかどうかを検出し、それに応じてこのTaintを追加または削除します。

### node.kubernetes.io/unreachable

種類: Taint

例: `node.kubernetes.io/unreachable: "NoExecute"`

使用対象: Node

Nodeコントローラーは、[NodeCondition](/docs/concepts/architecture/nodes/#condition)の`Ready`が`Unknown`であるNodeに、このTaintを追加します。

### node.kubernetes.io/unschedulable

種類: Taint

例: `node.kubernetes.io/unschedulable: "NoSchedule"`

使用対象: Node

このTaintは、競合状態を避けるために、ノードの初期化時に追加されます。

### node.kubernetes.io/memory-pressure

種類: Taint

例: `node.kubernetes.io/memory-pressure: "NoSchedule"`

使用対象: Node

kubeletは、Nodeで観測された`memory.available`と`allocatableMemory.available`に基づいてメモリープレッシャーを検出します。
次に、観測された値をkubeletに設定可能な対応するしきい値と比較し、Nodeの状態とTaintを追加または削除するかどうかを決定します。

### node.kubernetes.io/disk-pressure

種類: Taint

例: `node.kubernetes.io/disk-pressure :"NoSchedule"`

使用対象: Node

kubeletは、Nodeで観測された`imagefs.available`、`imagefs.inodesFree`、`nodefs.available`、`nodefs.inodesFree`(Linuxのみ)に基づいてディスクプレッシャーを検出します。
次に、観測された値をkubeletに設定可能な対応するしきい値と比較し、Nodeの状態とTaintを追加または削除するかどうかを決定します。

### node.kubernetes.io/network-unavailable

種類: Taint

例: `node.kubernetes.io/network-unavailable: "NoSchedule"`

使用対象: Node

使用しているクラウドプロバイダーが追加のネットワーク設定を必要とする場合、kubeletが最初にこのTaintを設定します。
クラウド上の経路が適切に設定された場合にのみ、クラウドプロバイダーがこのTaintを削除します。

### node.kubernetes.io/pid-pressure

種類: Taint

例: `node.kubernetes.io/pid-pressure: "NoSchedule"`

使用対象: Node

kubeletは、`/proc/sys/kernel/pid_max`の値とノード上でKubernetesが消費しているPID数の差分を確認し、`pid.available`メトリクスとして参照される利用可能なPID数を取得します。
次に、そのメトリクスをkubeletに設定可能な対応するしきい値と比較し、ノードの状態とTaintを追加または削除するかどうかを決定します。

### node.kubernetes.io/out-of-service

種類: Taint

例: `node.kubernetes.io/out-of-service:NoExecute`

使用対象: Node

ユーザーは、NodeにこのTaintを手動で追加して、サービス停止中であることを示せます。
このTaintによってNodeがサービス停止中とされた場合、そのノード上のPodに一致するTolerationがなければ、Podは強制的に削除されます。
また、そのノード上で終了するPodのボリュームのデタッチ操作は直ちに行われます。
これにより、サービス停止中のノード上のPodを、別のノードで迅速に復旧できます。

{{< caution >}}
このTaintをいつ、どのように使用するかについては、[ノードの非正常終了](/docs/concepts/cluster-administration/node-shutdown/#non-graceful-node-shutdown)を参照してください。
{{< /caution >}}

### node.cloudprovider.kubernetes.io/uninitialized

種類: Taint

例: `node.cloudprovider.kubernetes.io/uninitialized: "NoSchedule"`

使用対象: Node

kubeletが"external"クラウドプロバイダーを指定して起動された場合、NodeにこのTaintを設定して、使用できないことを示します。
cloud-controller-manager内のコントローラーがNodeを初期化すると、このTaintを削除します。

### node.cloudprovider.kubernetes.io/shutdown

種類: Taint

例: `node.cloudprovider.kubernetes.io/shutdown: "NoSchedule"`

使用対象: Node

Nodeがクラウドプロバイダーによって指定されたシャットダウン状態にある場合、`node.cloudprovider.kubernetes.io/shutdown`が、効果`NoSchedule`を持つTaintとしてそのNodeに設定されます。

### feature.node.kubernetes.io/*

種類: ラベル

例: `feature.node.kubernetes.io/network-sriov.capable: "true"`

使用対象: Node

これらのラベルは、Node Feature Discovery(NFD)コンポーネントがノードの機能を通知するために使用します。
すべての組み込みラベルは`feature.node.kubernetes.io`ラベル名前空間を使用し、`feature.node.kubernetes.io/<feature-name>: "true"`の形式を取ります。
NFDには、ベンダーやアプリケーションに固有のラベルを作成するための拡張ポイントが多数あります。
詳細については、[カスタマイズガイド](https://kubernetes-sigs.github.io/node-feature-discovery/v0.12/usage/customization-guide)を参照してください。

### nfd.node.kubernetes.io/master.version

種類: アノテーション

例: `nfd.node.kubernetes.io/master.version: "v0.6.0"`

使用対象: Node

Node Feature Discovery(NFD)の[master](https://kubernetes-sigs.github.io/node-feature-discovery/stable/usage/nfd-master.html)がスケジュールされているノードでは、このアノテーションがNFD masterのバージョンを記録します。
情報を提供するためだけに使用されます。

### nfd.node.kubernetes.io/worker.version

種類: アノテーション

例: `nfd.node.kubernetes.io/worker.version: "v0.4.0"`

使用対象: Node

このアノテーションは、Node Feature Discoveryの[worker](https://kubernetes-sigs.github.io/node-feature-discovery/stable/usage/nfd-worker.html)がノード上で実行されている場合、そのバージョンを記録します。
情報を提供するためだけに使用されます。

### nfd.node.kubernetes.io/feature-labels

種類: アノテーション

例: `nfd.node.kubernetes.io/feature-labels: "cpu-cpuid.ADX,cpu-cpuid.AESNI,cpu-hardware_multithreading,kernel-version.full"`

使用対象: Node

このアノテーションは、[Node Feature Discovery](https://kubernetes-sigs.github.io/node-feature-discovery/)(NFD)が管理するノード機能ラベルの一覧を、カンマ区切りで記録します。
NFDはこれを内部の仕組みで使用します。
このアノテーションを自身で編集するべきではありません。

### nfd.node.kubernetes.io/extended-resources

種類: アノテーション

例: `nfd.node.kubernetes.io/extended-resources: "accelerator.acme.example/q500,example.com/coprocessor-fx5"`

使用対象: Node

このアノテーションは、[Node Feature Discovery](https://kubernetes-sigs.github.io/node-feature-discovery/)(NFD)が管理する[拡張リソース](/docs/concepts/configuration/manage-resources-containers/#extended-resources)の一覧を、カンマ区切りで記録します。
NFDはこれを内部の仕組みで使用します。
このアノテーションを自身で編集するべきではありません。

### nfd.node.kubernetes.io/node-name

種類: ラベル

例: `nfd.node.kubernetes.io/node-name: node-1`

使用対象: Node

NodeFeatureオブジェクトが対象とするノードを指定します。
NodeFeatureオブジェクトの作成者はこのラベルを設定しなければなりません。
オブジェクトの利用者は、このラベルを使用して、特定のノードに指定された機能を絞り込むことが想定されています。

{{< note >}}
これらのNode Feature Discovery(NFD)のラベルやアノテーションは、NFDが動作しているノードにのみ適用されます。
NFDとそのコンポーネントについては、公式の[ドキュメント](https://kubernetes-sigs.github.io/node-feature-discovery/stable/get-started/)を参照してください。
{{< /note >}}

### service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval (Beta) {#service-beta-kubernetes-io-aws-load-balancer-access-log-emit-interval}

例: `service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: "5"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてServiceのロードバランサーを設定します。
値は、ロードバランサーがログエントリーを書き込む頻度を決定します。
例えば、値を5に設定すると、ログは5秒間隔で書き込まれます。

### service.beta.kubernetes.io/aws-load-balancer-access-log-enabled (Beta) {#service-beta-kubernetes-io-aws-load-balancer-access-log-enabled}

例: `service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "false"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてServiceのロードバランサーを設定します。
アノテーションを"true"に設定すると、アクセスログが有効になります。

### service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name (Beta) {#service-beta-kubernetes-io-aws-load-balancer-access-log-s3-bucket-name}

例: `service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: example`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてServiceのロードバランサーを設定します。
ロードバランサーは、指定した名前のS3バケットにログを書き込みます。

### service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix (Beta) {#service-beta-kubernetes-io-aws-load-balancer-access-log-s3-bucket-prefix}

例: `service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: "/example"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてServiceのロードバランサーを設定します。
ロードバランサーは、指定したプレフィックスを持つログオブジェクトを書き込みます。

### service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags (Beta) {#service-beta-kubernetes-io-aws-load-balancer-additional-resource-tags}

例: `service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags: "Environment=demo,Project=example"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションの値にあるカンマ区切りのキーと値のペアに基づいて、ロードバランサーのタグ(AWSの概念)を設定します。

### service.beta.kubernetes.io/aws-load-balancer-alpn-policy (Beta) {#service-beta-kubernetes-io-aws-load-balancer-alpn-policy}

例: `service.beta.kubernetes.io/aws-load-balancer-alpn-policy: HTTP2Optional`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-attributes (Beta) {#service-beta-kubernetes-io-aws-load-balancer-attributes}

例: `service.beta.kubernetes.io/aws-load-balancer-attributes: "deletion_protection.enabled=true"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-backend-protocol (Beta) {#service-beta-kubernetes-io-aws-load-balancer-backend-protocol}

例: `service.beta.kubernetes.io/aws-load-balancer-backend-protocol: tcp`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションの値に基づいて、ロードバランサーのリスナーを設定します。

### service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled (Beta) {#service-beta-kubernetes-io-aws-load-balancer-connection-draining-enabled}

例: `service.beta.kubernetes.io/aws-load-balancer-connection-draining-enabled: "false"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
ロードバランサーのコネクションドレイニングの設定は、指定した値によって決まります。

### service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout (Beta) {#service-beta-kubernetes-io-aws-load-balancer-connection-draining-timeout}

例: `service.beta.kubernetes.io/aws-load-balancer-connection-draining-timeout: "60"`

使用対象: Service

`type: LoadBalancer`のServiceに[コネクションドレイニング](#service-beta-kubernetes-io-aws-load-balancer-connection-draining-enabled)を設定し、AWSクラウドを使用している場合、連携機能はこのアノテーションに基づいてドレイニングの期間を設定します。
設定した値によって、ドレイニングのタイムアウトが秒単位で決まります。

### service.beta.kubernetes.io/aws-load-balancer-ip-address-type (Beta) {#service-beta-kubernetes-io-aws-load-balancer-ip-address-type}

例: `service.beta.kubernetes.io/aws-load-balancer-ip-address-type: ipv4`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout (Beta) {#service-beta-kubernetes-io-aws-load-balancer-connection-idle-timeout}

例: `service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "60"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
ロードバランサーには、その接続に適用されるアイドルタイムアウト期間(秒)が設定されます。
アイドルタイムアウト期間が経過するまでにデータが送信も受信もされなかった場合、ロードバランサーは接続を閉じます。

### service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled (Beta) {#service-beta-kubernetes-io-aws-load-balancer-cross-zone-load-balancing-enabled}

例: `service.beta.kubernetes.io/aws-load-balancer-cross-zone-load-balancing-enabled: "true"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
このアノテーションを"true"に設定すると、各ロードバランサーノードは、有効なすべての[アベイラビリティーゾーン](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-availability-zones)の登録済みターゲットに、リクエストを均等に分配します。
クロスゾーン負荷分散を無効にすると、各ロードバランサーノードは、自身のアベイラビリティーゾーン内の登録済みターゲットにのみ、リクエストを均等に分配します。

### service.beta.kubernetes.io/aws-load-balancer-eip-allocations (Beta) {#service-beta-kubernetes-io-aws-load-balancer-eip-allocations}

例: `service.beta.kubernetes.io/aws-load-balancer-eip-allocations: "eipalloc-01bcdef23bcdef456,eipalloc-def1234abc4567890"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
値は、Elastic IPアドレスの割り当てIDをカンマで区切ったリストです。

このアノテーションは、ロードバランサーがAWS Network Load Balancerである`type: LoadBalancer`のServiceにのみ関係します。

### service.beta.kubernetes.io/aws-load-balancer-extra-security-groups (Beta) {#service-beta-kubernetes-io-aws-load-balancer-extra-security-groups}

例: `service.beta.kubernetes.io/aws-load-balancer-extra-security-groups: "sg-12abcd3456,sg-34dcba6543"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、ロードバランサーに設定する追加のAWS VPCセキュリティグループをカンマで区切ったリストです。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-healthy-threshold (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-healthy-threshold}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-healthy-threshold: "3"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、バックエンドがトラフィックを処理できる正常な状態とみなされるために必要な、連続して成功するヘルスチェックの回数を指定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-interval}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-interval: "30"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、ロードバランサーが行うヘルスチェックプローブの間隔を秒単位で指定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-path (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-papth}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-path: /healthcheck`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、HTTPヘルスチェックに使用するURLのパス部分を決定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-port (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-port}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-port: "24"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、ヘルスチェックの際にロードバランサーが接続するポートを決定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-protocol (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-protocol}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-protocol: TCP`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、ロードバランサーがバックエンドターゲットの健全性を確認する方法を決定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-timeout (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-timeout}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-timeout: "3"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、まだ成功していないプローブを自動的に失敗とみなすまでの秒数を指定します。

### service.beta.kubernetes.io/aws-load-balancer-healthcheck-unhealthy-threshold (Beta) {#service-beta-kubernetes-io-aws-load-balancer-healthcheck-unhealthy-threshold}

例: `service.beta.kubernetes.io/aws-load-balancer-healthcheck-unhealthy-threshold: "3"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
アノテーションの値は、バックエンドがトラフィックを処理できない異常な状態とみなされるために必要な、連続して失敗するヘルスチェックの回数を指定します。

### service.beta.kubernetes.io/aws-load-balancer-internal (Beta) {#service-beta-kubernetes-io-aws-load-balancer-internal}

例: `service.beta.kubernetes.io/aws-load-balancer-internal: "true"`

使用対象: Service

クラウドコントローラーマネージャーとAWS Elastic Load Balancingの連携は、このアノテーションに基づいてロードバランサーを設定します。
このアノテーションを"true"に設定すると、連携機能は内部ロードバランサーを設定します。

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)を使用している場合は、[`service.beta.kubernetes.io/aws-load-balancer-scheme`](#service-beta-kubernetes-io-aws-load-balancer-scheme)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-manage-backend-security-group-rules (Beta) {#service-beta-kubernetes-io-aws-load-balancer-manage-backend-security-group-rules}

例: `service.beta.kubernetes.io/aws-load-balancer-manage-backend-security-group-rules: "true"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-name (Beta) {#service-beta-kubernetes-io-aws-load-balancer-name}

例: `service.beta.kubernetes.io/aws-load-balancer-name: my-elb`

使用対象: Service

Serviceにこのアノテーションを設定し、同じServiceに`service.beta.kubernetes.io/aws-load-balancer-type: "external"`アノテーションも設定し、クラスター内で[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)を使用している場合、AWS Load Balancer Controllerはロードバランサーの名前を*この*アノテーションに設定した値にします。

AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-nlb-target-type (Beta) {#service-beta-kubernetes-io-aws-load-balancer-nlb-target-type}

例: `service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: "true"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-private-ipv4-addresses (Beta) {#service-beta-kubernetes-io-aws-load-balancer-private-ipv4-addresses}

例: `service.beta.kubernetes.io/aws-load-balancer-private-ipv4-addresses: "198.51.100.0,198.51.100.64"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-proxy-protocol (Beta) {#service-beta-kubernetes-io-aws-load-balancer-proxy-protocol}

例: `service.beta.kubernetes.io/aws-load-balancer-proxy-protocol: "*"`

使用対象: Service

KubernetesとAWS Elastic Load Balancingの公式の連携は、このアノテーションに基づいてロードバランサーを設定します。
許可される値は`"*"`のみで、ロードバランサーがバックエンドのPodへのTCP接続をPROXYプロトコルでラップするべきであることを示します。

### service.beta.kubernetes.io/aws-load-balancer-scheme (Beta) {#service-beta-kubernetes-io-aws-load-balancer-scheme}

例: `service.beta.kubernetes.io/aws-load-balancer-scheme: internal`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-security-groups (非推奨) {#service-beta-kubernetes-io-aws-load-balancer-security-groups}

例: `service.beta.kubernetes.io/aws-load-balancer-security-groups: "sg-53fae93f,sg-8725gr62r"`

使用対象: Service

AWS Load Balancer Controllerは、このアノテーションを使用して、AWSロードバランサーにアタッチするセキュリティグループをカンマ区切りのリストで指定します。
セキュリティグループの名前とIDの両方がサポートされます。
名前は`groupName`属性ではなく、`Name`タグと一致するものです。

このアノテーションをServiceに追加すると、ロードバランサーコントローラーは、アノテーションが参照するセキュリティグループをロードバランサーにアタッチします。
このアノテーションを省略すると、AWS Load Balancer Controllerは新しいセキュリティグループを自動的に作成して、ロードバランサーにアタッチします。

{{< note >}}
Kubernetes v1.27以降では、このアノテーションを直接設定または読み取りません。
ただし、Kubernetesプロジェクトの一部であるAWS Load Balancer Controllerは、引き続き`service.beta.kubernetes.io/aws-load-balancer-security-groups`アノテーションを使用します。
{{< /note >}}

### service.beta.kubernetes.io/load-balancer-source-ranges (非推奨) {#service-beta-kubernetes-io-load-balancer-source-ranges}

例: `service.beta.kubernetes.io/load-balancer-source-ranges: "192.0.2.0/25"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
代わりにServiceの`.spec.loadBalancerSourceRanges`を設定するべきです。

### service.beta.kubernetes.io/aws-load-balancer-ssl-cert (Beta) {#service-beta-kubernetes-io-aws-load-balancer-ssl-cert}

例: `service.beta.kubernetes.io/aws-load-balancer-ssl-cert: "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"`

使用対象: Service

AWS Elastic Load Balancingとの公式の連携は、このアノテーションに基づいて`type: LoadBalancer`のServiceのTLSを設定します。
アノテーションの値は、ロードバランサーのリスナーが使用するべきX.509証明書のAWS Resource Name(ARN)です。

(TLSプロトコルは、SSLと略される以前の技術に基づいています)。

### service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy (Beta) {#service-beta-kubernetes-io-aws-load-balancer-ssl-negotiation-policy}

例: `service.beta.kubernetes.io/aws-load-balancer-ssl-negotiation-policy: ELBSecurityPolicy-TLS-1-2-2017-01`

AWS Elastic Load Balancingとの公式の連携は、このアノテーションに基づいて`type: LoadBalancer`のServiceのTLSを設定します。
アノテーションの値は、クライアントとのTLSネゴシエーションに使用するAWSポリシーの名前です。

### service.beta.kubernetes.io/aws-load-balancer-ssl-ports (Beta) {#service-beta-kubernetes-io-aws-load-balancer-ssl-ports}

例: `service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "*"`

AWS Elastic Load Balancingとの公式の連携は、このアノテーションに基づいて`type: LoadBalancer`のServiceのTLSを設定します。
アノテーションの値は、ロードバランサーのすべてのポートでTLSを使用することを意味する`"*"`、またはポート番号をカンマで区切ったリストです。

### service.beta.kubernetes.io/aws-load-balancer-subnets (Beta) {#service-beta-kubernetes-io-aws-load-balancer-subnets}

例: `service.beta.kubernetes.io/aws-load-balancer-subnets: "private-a,private-b"`

KubernetesとAWSの公式の連携は、このアノテーションを使用してロードバランサーを設定し、マネージド負荷分散サービスをどのAWSアベイラビリティーゾーンにデプロイするかを決定します。
値は、サブネット名をカンマで区切ったリスト、またはサブネットIDをカンマで区切ったリストです。

### service.beta.kubernetes.io/aws-load-balancer-target-group-attributes (Beta) {#service-beta-kubernetes-io-aws-load-balancer-target-group-attributes}

例: `service.beta.kubernetes.io/aws-load-balancer-target-group-attributes: "stickiness.enabled=true,stickiness.type=source_ip"`

使用対象: Service

[AWS Load Balancer Controller](https://kubernetes-sigs.github.io/aws-load-balancer-controller/)がこのアノテーションを使用します。
AWS Load Balancer Controllerのドキュメントの[アノテーション](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)を参照してください。

### service.beta.kubernetes.io/aws-load-balancer-target-node-labels (Beta) {#service-beta-kubernetes-io-aws-target-node-labels}

例: `service.beta.kubernetes.io/aws-load-balancer-target-node-labels: "kubernetes.io/os=Linux,topology.kubernetes.io/region=us-east-2"`

KubernetesとAWSの公式の連携は、このアノテーションを使用して、クラスター内のどのノードをロードバランサーの有効なターゲットとみなすかを決定します。

### service.beta.kubernetes.io/aws-load-balancer-type (Beta) {#service-beta-kubernetes-io-aws-load-balancer-type}

例: `service.beta.kubernetes.io/aws-load-balancer-type: external`

KubernetesとAWSの公式の連携は、このアノテーションを使用して、AWSクラウドプロバイダー連携が`type: LoadBalancer`のServiceを管理するべきかどうかを決定します。

許可される値は次の2つです:

`nlb`
: クラウドコントローラーマネージャーがNetwork Load Balancerを設定

`external`
: クラウドコントローラーマネージャーはロードバランサーを設定しない

AWSで`type: LoadBalancer`のServiceをデプロイし、`service.beta.kubernetes.io/aws-load-balancer-type`アノテーションを設定しない場合、AWS連携は従来のElastic Load Balancerをデプロイします。
別途指定しない限り、アノテーションが存在しない場合のこの動作がデフォルトです。

`type: LoadBalancer`のServiceでこのアノテーションを`external`に設定し、クラスター内でAWS Load Balancer Controllerが正常に動作している場合、AWS Load Balancer ControllerはServiceの仕様に基づいてロードバランサーのデプロイを試みます。

{{< caution >}}
既存のServiceオブジェクトの`service.beta.kubernetes.io/aws-load-balancer-type`アノテーションを変更または追加しないでください。
詳細については、このトピックに関するAWSのドキュメントを参照してください。
{{< /caution >}}

### service.beta.kubernetes.io/azure-load-balancer-disable-tcp-reset (非推奨) {#service-beta-kubernetes-azure-load-balancer-disble-tcp-reset}

例: `service.beta.kubernetes.io/azure-load-balancer-disable-tcp-reset: "false"`

使用対象: Service

このアノテーションは、Azure Standard Load Balancerを使用するServiceでのみ機能します。
Serviceでこのアノテーションを使用して、アイドルタイムアウト時のTCPリセットをロードバランサーが無効にするか有効にするかを指定します。
TCPリセットを有効にすると、アプリケーションが接続の終了を検出し、期限切れの接続を削除して新しい接続を開始できるため、動作の予測可能性が高まります。
値はtrueまたはfalseに設定できます。

詳細については、[Load BalancerのTCPリセット](https://learn.microsoft.com/en-gb/azure/load-balancer/load-balancer-tcp-reset)を参照してください。

{{< note >}}
このアノテーションは非推奨です。
{{< /note >}}

### pod-security.kubernetes.io/enforce

種類: ラベル

例: `pod-security.kubernetes.io/enforce: "baseline"`

使用対象: Namespace

値は、[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)のレベルに対応する`privileged`、`baseline`、`restricted`のいずれかで**なければなりません**。
具体的には、`enforce`ラベルは、そのラベルが付いたNamespace内で、指定されたレベルの要件を満たさないPodの作成を*禁止*します。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### pod-security.kubernetes.io/enforce-version

種類: ラベル

例: `pod-security.kubernetes.io/enforce-version: "{{< skew currentVersion >}}"`

使用対象: Namespace

値は、`latest`または`v<major>.<minor>`形式の有効なKubernetesバージョンで**なければなりません**。
これは、Podの検証時に適用する[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)ポリシーのバージョンを決定します。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### pod-security.kubernetes.io/audit

種類: ラベル

例: `pod-security.kubernetes.io/audit: "baseline"`

使用対象: Namespace

値は、[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)のレベルに対応する`privileged`、`baseline`、`restricted`のいずれかで**なければなりません**。
具体的には、`audit`ラベルは、そのラベルが付いたNamespace内で、指定されたレベルの要件を満たさないPodの作成を阻止せず、Podにアノテーションを追加します。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### pod-security.kubernetes.io/audit-version

種類: ラベル

例: `pod-security.kubernetes.io/audit-version: "{{< skew currentVersion >}}"`

使用対象: Namespace

値は、`latest`または`v<major>.<minor>`形式の有効なKubernetesバージョンで**なければなりません**。
これは、Podの検証時に適用する[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)ポリシーのバージョンを決定します。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### pod-security.kubernetes.io/warn

種類: ラベル

例: `pod-security.kubernetes.io/warn: "baseline"`

使用対象: Namespace

値は、[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)のレベルに対応する`privileged`、`baseline`、`restricted`のいずれかで**なければなりません**。
具体的には、`warn`ラベルは、そのラベルが付いたNamespace内で、指定されたレベルの要件を満たさないPodの作成を阻止せず、作成後にユーザーへ警告を返します。
Deployment、Job、StatefulSetなどのPodテンプレートを含むオブジェクトを作成または更新する際にも、警告が表示されます。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### pod-security.kubernetes.io/warn-version

種類: ラベル

例: `pod-security.kubernetes.io/warn-version: "{{< skew currentVersion >}}"`

使用対象: Namespace

値は、`latest`または`v<major>.<minor>`形式の有効なKubernetesバージョンで**なければなりません**。
これは、送信されたPodの検証時に適用する[Podセキュリティ標準](/docs/concepts/security/pod-security-standards)ポリシーのバージョンを決定します。
Deployment、Job、StatefulSetなどのPodテンプレートを含むオブジェクトを作成または更新する際にも、警告が表示されます。

詳細については、[名前空間レベルでのPodセキュリティの強制](/docs/concepts/security/pod-security-admission)を参照してください。

### rbac.authorization.kubernetes.io/autoupdate

種類: アノテーション

例: `rbac.authorization.kubernetes.io/autoupdate: "false"`

使用対象: ClusterRole、ClusterRoleBinding、Role、RoleBinding

APIサーバーが作成したデフォルトのRBACオブジェクトでこのアノテーションが`"true"`に設定されている場合、サーバーの起動時に自動更新され、不足している権限とサブジェクトが追加されます(追加の権限やサブジェクトはそのまま残ります)。
特定のロールやロールバインディングが自動更新されるのを防ぐには、このアノテーションを`"false"`に設定します。
自身でRBACオブジェクトを作成してこのアノテーションを`"false"`に設定した場合、{{< glossary_tooltip text="マニフェスト" term_id="manifest" >}}内の任意のRBACオブジェクトを調整できる`kubectl auth reconcile`は、このアノテーションを尊重し、不足している権限とサブジェクトを自動追加しません。

### kubernetes.io/psp (非推奨) {#kubernetes-io-psp}

種類: アノテーション

例: `kubernetes.io/psp: restricted`

使用対象: Pod

このアノテーションは、[PodSecurityPolicy](/docs/concepts/security/pod-security-policy/)オブジェクトを使用している場合にのみ関係していました。
Kubernetes v{{< skew currentVersion >}}はPodSecurityPolicy APIをサポートしていません。

PodSecurityPolicyアドミッションコントローラーがPodを受け入れると、そのPodにこのアノテーションを付けるよう変更していました。
アノテーションの値は、検証に使用されたPodSecurityPolicyの名前でした。

### seccomp.security.alpha.kubernetes.io/pod (機能しません) {#seccomp-security-alpha-kubernetes-io-pod}

種類: アノテーション

使用対象: Pod

v1.25より前のKubernetesでは、このアノテーションを使用してseccompの動作を設定できました。
Podにseccompの制限を指定するためのサポートされている方法については、[seccompでコンテナのシステムコールを制限する](/docs/tutorials/security/seccomp/)を参照してください。

### container.seccomp.security.alpha.kubernetes.io/[NAME] (機能しません) {#container-seccomp-security-alpha-kubernetes-io}

種類: アノテーション

使用対象: Pod

v1.25より前のKubernetesでは、このアノテーションを使用してseccompの動作を設定できました。
Podにseccompの制限を指定するためのサポートされている方法については、[seccompでコンテナのシステムコールを制限する](/docs/tutorials/security/seccomp/)を参照してください。

### snapshot.storage.kubernetes.io/allow-volume-mode-change

種類: アノテーション

例: `snapshot.storage.kubernetes.io/allow-volume-mode-change: "true"`

使用対象: VolumeSnapshotContent

値は`true`または`false`にできます。
これは、VolumeSnapshotからPersistentVolumeClaimを作成する際に、ユーザーがソースボリュームのモードを変更できるかどうかを決定します。

詳細については、[スナップショットのボリュームモードを変換する](/docs/concepts/storage/volume-snapshots/#convert-volume-mode)と[Kubernetes CSI開発者向けドキュメント](https://kubernetes-csi.github.io/docs/)を参照してください。

### scheduler.alpha.kubernetes.io/critical-pod (非推奨) {#scheduler-alpha-kubernetes-io-critical-pod-deprecated}

種類: アノテーション

例: `scheduler.alpha.kubernetes.io/critical-pod: ""`

使用対象: Pod

このアノテーションは、そのPodが重要なPodであることをKubernetesコントロールプレーンに伝え、deschedulerがそのPodを削除しないようにします。

{{< note >}}
v1.16でこのアノテーションは削除され、[Podの優先度](/docs/concepts/scheduling-eviction/pod-priority-preemption/)に置き換えられました。
{{< /note >}}

### jobset.sigs.k8s.io/jobset-name

種類: ラベル、アノテーション

例:  `jobset.sigs.k8s.io/jobset-name: "my-jobset"`

使用対象: Job、Pod

このラベルまたはアノテーションは、JobまたはPodが属するJobSetの名前を保存するために使用されます。
[JobSet](https://jobset.sigs.k8s.io)は、Kubernetesクラスターにデプロイできる拡張APIです。

### jobset.sigs.k8s.io/replicatedjob-replicas

種類: ラベル、アノテーション

例: `jobset.sigs.k8s.io/replicatedjob-replicas: "5"`

使用対象: Job、Pod

このラベルまたはアノテーションは、ReplicatedJobのレプリカ数を指定します。

### jobset.sigs.k8s.io/replicatedjob-name

種類: ラベル、アノテーション

例: `jobset.sigs.k8s.io/replicatedjob-name: "my-replicatedjob"`

使用対象: Job、Pod

このラベルまたはアノテーションは、このJobまたはPodが属するReplicatedJobの名前を保存します。

### jobset.sigs.k8s.io/job-index

種類: ラベル、アノテーション

例: `jobset.sigs.k8s.io/job-index: "0"`

使用対象: Job、Pod

このラベルまたはアノテーションは、JobSetコントローラーが子のJobとPodに設定します。
親のReplicatedJob内におけるJobレプリカのインデックスが含まれます。

### jobset.sigs.k8s.io/job-key

種類: ラベル、アノテーション

例: `jobset.sigs.k8s.io/job-key: "0f1e93893c4cb372080804ddb9153093cb0d20cefdd37f653e739c232d363feb"`

使用対象: Job、Pod

JobSetコントローラーは、JobSetの子のJobとPodに、このラベルと同じキーを持つアノテーションを設定します。
値は、名前空間で修飾したJob名のSHA256ハッシュです。

### alpha.jobset.sigs.k8s.io/exclusive-topology

種類: アノテーション

例: `alpha.jobset.sigs.k8s.io/exclusive-topology: "zone"`

使用対象: JobSet、Job

[JobSet](https://jobset.sigs.k8s.io)にこのラベルまたはアノテーションを設定すると、トポロジーグループごとにJobを排他的に配置できます。
ReplicatedJobのテンプレートにも、このラベルまたはアノテーションを定義できます。
詳細については、JobSetのドキュメントを参照してください。

### alpha.jobset.sigs.k8s.io/node-selector

種類: アノテーション

例: `alpha.jobset.sigs.k8s.io/node-selector: "true"`

使用対象: Job、Pod

このラベルまたはアノテーションは、JobSetに適用できます。
設定すると、JobSetコントローラーは、Jobと対応するPodにノードセレクターとTolerationを追加して変更します。
これにより、トポロジードメインごとのJobの排他的な配置が保証され、戦略に基づいて、これらのPodのスケジューリング先が特定のノードに制限されます。

### alpha.jobset.sigs.k8s.io/namespaced-job

種類: ラベル

例: `alpha.jobset.sigs.k8s.io/namespaced-job: "default_myjobset-replicatedjob-0"`

使用対象: Node

このラベルは、ノードに手動または自動で(例えば、クラスターオートスケーラーによって)設定されます。
`alpha.jobset.sigs.k8s.io/node-selector`が`"true"`に設定されている場合、JobSetコントローラーは、このノードラベルに対応するnodeSelectorを追加します(次に説明する`alpha.jobset.sigs.k8s.io/no-schedule` Taintに対するTolerationも追加します)。

### alpha.jobset.sigs.k8s.io/no-schedule

種類: Taint

例: `alpha.jobset.sigs.k8s.io/no-schedule: "NoSchedule"`

使用対象: Node

このTaintは、ノードに手動または自動で(例えば、クラスターオートスケーラーによって)設定されます。
`alpha.jobset.sigs.k8s.io/node-selector`が`"true"`に設定されている場合、JobSetコントローラーは、このノードのTaintに対するTolerationを追加します(前述の`alpha.jobset.sigs.k8s.io/namespaced-job`ラベルに対応するノードセレクターも追加します)。

### jobset.sigs.k8s.io/coordinator

種類: アノテーション、ラベル

例: `jobset.sigs.k8s.io/coordinator: "myjobset-workers-0-0.headless-svc"`

使用対象: Job、Pod

このアノテーションまたはラベルは、[JobSet](https://jobset.sigs.k8s.io)のspecで`.spec.coordinator`フィールドが定義されている場合に、コーディネーターPodに到達できる安定したネットワークエンドポイントを保存するため、JobとPodで使用されます。

## 監査に使用されるアノテーション {#annotations-used-for-audit}

<!-- アノテーション順 -->
- [`authorization.k8s.io/decision`](/docs/reference/labels-annotations-taints/audit-annotations/#authorization-k8s-io-decision)
- [`authorization.k8s.io/reason`](/docs/reference/labels-annotations-taints/audit-annotations/#authorization-k8s-io-reason)
- [`insecure-sha1.invalid-cert.kubernetes.io/$hostname`](/docs/reference/labels-annotations-taints/audit-annotations/#insecure-sha1-invalid-cert-kubernetes-io-hostname)
- [`missing-san.invalid-cert.kubernetes.io/$hostname`](/docs/reference/labels-annotations-taints/audit-annotations/#missing-san-invalid-cert-kubernetes-io-hostname)
- [`pod-security.kubernetes.io/audit-violations`](/docs/reference/labels-annotations-taints/audit-annotations/#pod-security-kubernetes-io-audit-violations)
- [`pod-security.kubernetes.io/enforce-policy`](/docs/reference/labels-annotations-taints/audit-annotations/#pod-security-kubernetes-io-enforce-policy)
- [`pod-security.kubernetes.io/exempt`](/docs/reference/labels-annotations-taints/audit-annotations/#pod-security-kubernetes-io-exempt)
- [`validation.policy.admission.k8s.io/validation_failure`](/docs/reference/labels-annotations-taints/audit-annotations/#validation-policy-admission-k8s-io-validation-failure)

詳細については、[監査アノテーション](/docs/reference/labels-annotations-taints/audit-annotations/)を参照してください。

## kubeadm

### kubeadm.alpha.kubernetes.io/cri-socket (非推奨) {#kubeadm-alpha-kubernetes-io-cri-socket}

種類: アノテーション

例: `kubeadm.alpha.kubernetes.io/cri-socket: unix:///run/containerd/container.sock`

使用対象: Node

{{< note >}}
v1.34以降、このアノテーションは非推奨であり、kubeadmはこれを能動的に設定したり使用したりしなくなります。
{{< /note >}}

### kubeadm.kubernetes.io/etcd.advertise-client-urls

種類: アノテーション

例: `kubeadm.kubernetes.io/etcd.advertise-client-urls: https://172.17.0.18:2379`

使用対象: Pod

kubeadmがローカルで管理するetcdのPodに設定し、etcdクライアントが接続するべきURLの一覧を追跡するためのアノテーションです。
主にetcdクラスターのヘルスチェックのために使用されます。

### kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint

種類: アノテーション

例: `kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: https://172.17.0.18:6443`

使用対象: Pod

kubeadmがローカルで管理する`kube-apiserver`のPodに設定し、そのAPIサーバーインスタンスが公開する通知用アドレスとポートのエンドポイントを追跡するためのアノテーションです。

### kubeadm.kubernetes.io/component-config.hash

種類: アノテーション

例: `kubeadm.kubernetes.io/component-config.hash: 2c26b46b68ffc68ff99b453c1d30413413422d706483bfa0f98a5e886266e7ae`

使用対象: ConfigMap

kubeadmがコンポーネントの設定のために管理するConfigMapに設定するアノテーションです。
特定のコンポーネントについて、ユーザーがkubeadmのデフォルトとは異なる設定を適用したかどうかを判断するためのハッシュ(SHA-256)が含まれます。

### node-role.kubernetes.io/control-plane

種類: ラベル

使用対象: Node

ノードがコントロールプレーンのコンポーネントを実行するために使用されることを示すラベルです。
kubeadmツールは、管理するコントロールプレーンのノードにこのラベルを適用します。
他のクラスター管理ツールも通常、このTaintを設定します。

コントロールプレーンのノードにこのラベルを付けることで、それらのノードにのみPodをスケジュールしたり、コントロールプレーン上でPodを実行するのを避けたりしやすくなります。
このラベルが設定されている場合、[EndpointSliceコントローラー](/docs/concepts/services-networking/topology-aware-routing/#implementation-control-plane)は、トポロジーを考慮したヒントを計算する際にそのノードを無視します。

### node-role.kubernetes.io/*

種類: ラベル

例: `node-role.kubernetes.io/gpu: gpu`

使用対象: Node

この任意のラベルは、ノードのロールを示したい場合にノードに適用します。
ノードのロール(ラベルキーの`/`に続く文字列)は、キー全体がオブジェクトのラベルの[構文](/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set)ルールに従っている限り、設定できます。

Kubernetesは、特定のノードロールとして**control-plane**を定義しています。
このノードロールを示すために使用できるラベルは、[`node-role.kubernetes.io/control-plane`](#node-role-kubernetes-io-control-plane)です。

### node-role.kubernetes.io/control-plane {#node-role-kubernetes-io-control-plane-taint}

種類: Taint

例: `node-role.kubernetes.io/control-plane:NoSchedule`

使用対象: Node

kubeadmがコントロールプレーンのノードに適用し、Podの配置を制限して、特定のPodのみをスケジュールできるようにするTaintです。

このTaintが適用されている場合、コントロールプレーンのノードには重要なワークロードのみをスケジュールできます。
特定のノードからこのTaintを手動で削除するには、次のコマンドを使用します。

```shell
kubectl taint nodes <node-name> node-role.kubernetes.io/control-plane:NoSchedule-
```

### node-role.kubernetes.io/master (非推奨) {#node-role-kubernetes-io-master-taint}

種類: Taint

使用対象: Node

例: `node-role.kubernetes.io/master:NoSchedule`

kubeadmが以前、コントロールプレーンのノードに重要なワークロードのみをスケジュールできるようにするために適用していたTaintです。
[`node-role.kubernetes.io/control-plane`](#node-role-kubernetes-io-control-plane-taint) Taintに置き換えられています。
kubeadmは、この非推奨のTaintを設定したり使用したりしなくなりました。

### resource.kubernetes.io/admin-access {#resource-kubernetes-io-admin-access-resource-kubernetes-io-admin-access}

種類: ラベル

例: `resource.kubernetes.io/admin-access: "true"`

使用対象: Namespace

名前空間内の特定のresource.k8s.io API型に対して、管理者アクセスを付与するために使用されます。
名前空間にこのラベルを値`"true"`(大文字と小文字を区別)で設定すると、名前空間に属するすべての`resource.k8s.io` API型で`adminAccess: true`を使用できるようになります。
現在、この権限は`ResourceClaim`オブジェクトと`ResourceClaimTemplate`オブジェクトに適用されます。

詳細については、[動的リソース割り当ての管理者アクセス](/docs/concepts/resource-management/dynamic-resource-allocation/dra-api/#admin-access)を参照してください。
