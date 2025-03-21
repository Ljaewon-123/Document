# kub-data memo

## volumes

```
    spec:
      containers:
        - name: story
          image: nogaree/kub-data-demo:1
          volumeMounts: # 컨테이너 마운트될 볼륨 
            - mountPath: /app/story
              name: story-volume # 정의된 볼륨키로 맞춤 
      volumes: # 사용가능 볼륨 정의의
        - name: story-volume
          emptyDir: {}

```



          # emptyDir:

          # pod가 시작될 때 마다 단순히 빈 디렉토리 생성

          # pod가 살아있는한 이 디렉토리를 활성 상태로 유지하고 데이터로 채움

          # 그럼 컨테이너가 쓸수있음, 재시작 되거나 제거되도 데이터는 유지

          # pod가 제거되는경우 이 디렉토리 제거

          # pod가 살아있는한 컨테이너 재시작 동안 살아남는 디렉토리 제공



### hostpath

```
      volumes:
        - name: story-volume
          hostPath: # 마인드 마운트와 비슷함 
            path: /data  # 호스트머신 경로 
            type: DirectoryOrCreate  # 쿠버네티스에서 경로 처리 방법 정의 ex) 없는 경로면 만드냐 DirectoryOrCreate
    # type 예시 [DirectoryOrCreate: 없으면 생성, Directory: 없으면 실패]

# emptyDir은 멀티 pod시 서로 다른 트래픽에 대해서는 유지할수없음

# hostPath 단점
# 여러 워커노드가 있으면 노드 특정적이라 여러 pod 여러 노드에서 실행되는 여러 복제본은 
# 동일한 데이터에 엑세스할 수 없음  
# 장점 
# 이미 존재하는 데이터를 컨테이너에 공유시킬때 유용 

# 문제 
# pod가 종료되어 새로운 pod로 교체될 때
# pod종료시 볼륨도 파괴
# pod 스케일링하여 확장할때 유형에 따라 각 pod가 볼륨에 접근못할수도있음
# hostPath은 minikube같은 단일노드에서만 일함
# pod와 node에 독립적인 볼륨필요 
# ex) 데이터베이스 컨테이너 라든가 
# pod교체후에도 공유되어야하는 파일 
# 장기간 데이터는 스케일이나 교체로 종료되면 안됨 

# 영구 볼륨
# pod가 파괴되고 재생성되어도 손실되지않음
# pod와 독립적으로 볼륨을 정의하고 중앙위치에 정의 
# pod yaml파일을 편집하지 않고 볼륨과 다양한 posde에 사용하는것을 도움
# pod와 node에서 분리되어 영구 볼륨 클레임 이라는것을 만들수있음 
# 클레임은 pod 및 node에 속하지만 워커노드 외부에있는 영구 볼륨를 참조하여 연결해줌 
# 여러개의 개별적인 영구 볼륨에 대한 클레임을 가질수있음 
```



### 영구 볼륨 (pv , pvc)

```
# 단지 node용이 아니라 클러스터용 볼륨이 됨 

# 영구 볼륨 찾음 
#  kubectl get pv 
#  kubectl get pvc
```

```
  accessModes:
    - ReadWriteOnce
    # 3가지 모두도 가능 
    # ReadWriteMany 읽고 쓰고 여러 노드에서 접근가능 
    # 그래서 hostpath는 ReadWriteOnce이것만 사용할 수 있다.
    # [ReadWriteOnce, ReadOnlyMany, ReadWriteMany]
```








