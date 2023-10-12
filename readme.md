### 231006
## azure 정리 공간
### <br/><br/><br/>

## microsoft web
### 계정에 로그인할 때 microsoft athenticator 앱을 핸드폰에 다운로드하고 로그인해달라고 한다.
### 진행이 다 되면 앱 맨 아래에 '확인된 ID' 탭에서 QR 코드 스캔하고 로그인하면 된다.
#### ![image](https://github.com/Shin-jongwhan/theragenbio_2202/assets/62974484/d3ba5e76-f6b5-4bc9-ac98-87f45ba3c299)
### <br/>

### 계정에 접속하고 컨테이너로 가면 blob 컨테이너라고 하나 만들어진 것이 있다.
#### ![image](https://user-images.githubusercontent.com/62974484/226248023-c854cd99-d4c7-4a2d-96df-67be7f1b6557.png)
### 들어가서 업로드 누르면 업로드가 된다.
#### ![image](https://user-images.githubusercontent.com/62974484/226248044-06efa055-74e5-4f49-83e4-acafc73c7e6b.png)
### <br/><br/><br/>

## azure cli on linux
### 리눅스 서버에 conda 로 azure cli 설치해보자
```
# conda 는 다 오류가 난다...
conda create -n azure_cli -c bioconda azure-cli
conda create -n azure_cli -c conda-forge azure-cli-core
conda activate azure_cli

# python pip 로 설치해보자.
$ python -m pip install azure-cli
# 이용 방법
$ az
```
### 설치 확인
#### ![image](https://user-images.githubusercontent.com/62974484/226249176-74923dfe-26bb-4ac6-9ca6-05edcda09077.png)
### <br/>

### 설치가 되면 메뉴얼을 참고하자.
#### https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli
### 명령어 메뉴얼
#### https://learn.microsoft.com/ko-kr/cli/azure/storage/blob?view=azure-cli-latest
### <br/><br/>

### `login`
```
azure login
```
### 명령어를 입력하면 다음과 같이 나오는데, 웹에서 아래 사이트 접속하면 된다.
#### To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code H24WF3NYT to authenticate.
#### ![image](https://user-images.githubusercontent.com/62974484/226252077-f32e0f48-d696-498e-9ddb-9a4e1b7ce88e.png)
### 접속 확인
#### ![image](https://github.com/Shin-jongwhan/azure/assets/62974484/46991f76-e0ec-461b-99ad-f959d5b275ae)
### <br/>

### 업로드
### 엑세스 키를 확인한다.
#### ![image](https://user-images.githubusercontent.com/62974484/226264656-0c4dc823-7c14-41f3-9795-2b61bf347c22.png)
### * --container uploadcontainer1/test 맨 뒤에 / 붙이면 되긴 하는데 안에 폴더가 더 생성되니 붙이면 안 됨
```
az storage blob upload --account-name uploadstorage1 --container-name uploadcontainer1 --file test.txt --name test.txt  --connection-string [key] --auth-mode key

# 폴더 생성하면서 업로드
az storage blob upload-batch --account-name uploadstorage1 --destination uploadcontainer1/test -s /TBI/People/tbi/jhshin/test/test/ --connection-string [key] --auth-mode key
```
#### ![image](https://github.com/Shin-jongwhan/azure/assets/62974484/0835aa36-492d-4144-a053-56e281383b72)
#### ![image](https://user-images.githubusercontent.com/62974484/226267429-5c6e9893-bc53-4ce8-b13d-9478d4d497d2.png)
### <br/>

### 로그인이 되어 있는 경우라면 key 발급을 안 하고 auth-mode login 을 이용하자.
```
az storage blob upload --account-name hpcsourcestorage --container-name sbe-container-001 --file test.txt --name test.txt --auth-mode login
```
### <br/>

### 폴더 업로드
```
az storage blob upload-batch --destination sbe-container-001 --account-name hpcsourcestorage --destination-path [dir] --source [dir] --auth-mode login
```
### <br/>

### sync 로 복사하는 방법
```
az storage blob sync --account-name uploadstorage1 --container uploadcontainer1 -s /TBI/People/tbi/jhshin/test/test/ --connection-string [key]
```
#### ![image](https://user-images.githubusercontent.com/62974484/226267429-5c6e9893-bc53-4ce8-b13d-9478d4d497d2.png)
### 폴더로 sync 복사
```
az storage blob sync --account-name uploadstorage1 --container uploadcontainer1/test -s /TBI/People/tbi/jhshin/test/test/ --connection-string [key]
```
#### ![image](https://user-images.githubusercontent.com/62974484/226267725-631a1c9b-21fb-48c9-89fd-806e51b35cd2.png)
### <br/>

### 데이터 삭제
### 최상위 디렉터리부터 쭉 삭제한다.
### --pattern 을 줘서 삭제할 수도 있다.
```
az storage blob delete-batch --account-name uploadstorage1 --source uploadcontainer1 --connection-string [key] --auth-mode key
```
