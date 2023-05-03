# Desktop 파일 만들기 옵션

`fileName.desktop` 으로 하나만듬

```
[Desktop Entry]
Version=1.0
Name=MyApp
Comment=My Application
Exec=/path/to/myapp.sh (꼭 쉘이 아니라도 실행 파일 경로 )
Icon=/path/to/myapp.png
Terminal=false
Type=Application
Categories=Utility;Application;
```

# `실행파일` 권한

만약 root 라면

```
chmod u+x /path/fileName
```

아니라면 편하게 전부 주자

```
chmod 777 /path/fileName
```

## desktop 파일 권한

딱히? 근데 root아닐 경우 알림 피하고 싶으면 

```
chmod 777 /파일명
```
