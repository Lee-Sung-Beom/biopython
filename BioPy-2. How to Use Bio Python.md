# 2. 바이오파이썬 설치
2-1. Anaconda 설치
 - 바이오파이썬 모듈을 사용하기 위해 Anaconda의 설치 필요
   - https://www.python.org/downloads 를 입력하고 파이썬 설치
     - Anaconda 설치 시 파이썬이 함께 다운로드 되기에 선택 요소
     - 별도의 설치 시 Python IDE Tool 사용 가능
 - https://www.anaconda.com/download 에서 Anaconda 다운로드
   - Install for: 단계에서 [All Users] 항목을 선택하여 관리자 권한으로 설치
   - 바이오파이썬 설치 시 경로를 알아야 하기에 Anaconda의 설치 경로 필요
     - 일반적으로 C:\ProgramData\Anaconda3 해당 경로가 기본으로 지정
   - Anaconda 설치 시 파이썬이 함께 다운로드
 - Anaconda 설치 후 Anaconda Navigator에서  JupyterLab, JupyterNotebook 실행
   - JupyterLab을 종료할 때, 좌측 상단의 File -> Shutdown으로 종료
   - JupyterNotebook 종료할 때, 우측 상단의 Quit 으로 종료

2-2. 바이오파이썬 설치
 - JupyterLab 실행 후 자신이 작성한 파일을 저장할 폴더 생성
 - 해당 폴더 진입 후 Notebook - Python3 실행
 - 바이오파이썬 설치의 확인을 위해 명령어 입력
 ```python
  import Bio
  ---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
Input In [1], in <cell line: 1>()
----> 1 import Bio

ModuleNotFoundError: No module named 'Bio'
  #현재 바이오파이썬의 설치가 되지 않아 다음과 같은 오류 발생
 ```
 - Anaconda Navigator를 사용하여 바이오파이썬 설치
   - Anaconda Navigator 실행 후 **Environments** 메뉴 클릭
   - 표시 목록을 **installed** -> **All** 로 변경 후 우측에 **biopython** 검색
   - **biopython** 항목 선택 후 우측 하단의 **Apply** 클릭
 - 바이오파이썬 설치 이후 다시 JupyterLab에서 명령어 입력
 ```python
 import bio
 ```