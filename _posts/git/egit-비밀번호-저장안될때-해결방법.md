# egit password 저장 안될 때
이클립스에서 아래 경로의 설정을 연다.

```
Preferences > General > Security > Secure Storage. Select "OSX Keystore Integration", then click "Change Password..."
```

다음가 같은 메세지를 본다면 "No."를 선택하자.

"An error occurred while decrypting stored values... Do you want to cancel password change?" 

No를 했기 때문에 OSX keystore에 있는 마스터 비밀번호를 변경한다. 그리고 이 과정에서 비밀번호를 찾기에 사용되는 추가적인 정보를 입력 하도록 요청 받는다.