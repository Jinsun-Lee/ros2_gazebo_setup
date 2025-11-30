### 1. fusion360이 window에 깔려있다면 윈도우에 아래 레포를 다운로드한다
- ROS2용 URDF를 내보내기 위한 Fusion 360 스크립트 [레포](https://github.com/dheena2k2/fusion2urdf-ros2) 링크  
- 다운로드한 레포의 압축을 푼다
</br>

### 2. Windows의 PowerShell 실행하고 아래 명령어를 실행한다 
```
cd .\Desktop\fusion2urdf-ros2-master\
```
압축을 푼 폴더의 경로로 이동한다(난 레포 폴더를 바탕화면으로 옮김)  
</br>

```
Copy-Item ".\URDF_Exporter_Ros2\" -Destination "${env:APPDATA}\Autodesk\Autodesk Fusion 360\API\Scripts\" -Recurse
```
명령어를 실행하여 설치를 진행한다   
</br>  
  
![image](https://github.com/user-attachments/assets/51d60fac-ed60-48ea-a7ac-32d078317eee)
설치가 완료되어도 특별한 표시는 없다  
</br>

### 3. Fusion 360에서 유틸리티→애드인→스크립트 및 애드인 
![image](https://github.com/user-attachments/assets/b95ce06b-a07d-4656-990c-c96c5a1a0b67)
![image](https://github.com/user-attachments/assets/b217d668-4fc9-4a54-b59f-5a37c3dd72e8)

</br>

![image](https://github.com/user-attachments/assets/afae8ff8-b689-4e2d-a707-0c54ff233d89)


### 4. ros2_ws에 옮겨 빌드 후 실행
