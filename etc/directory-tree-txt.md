# tree 명령어로 폴더 구조 확인하기
1. cmd에서 확인하고 싶은 해당 폴더로 이동한다. 
    + shift+오른쪽 마우스를 클릭하여 '여기에 cmd창 열기'를 눌러도 된다.
2. tree 명령어 실행
```
> tree /f > tree_view.txt
```
3. 해당 폴더에 tree_view 텍스트 파일이 생성됨
4. 확인

```
C:.
│  db.sqlite3
│  manage.py
│  tree_view.txt
│  
├─.idea
│  │  misc.xml
│  │  modules.xml
│  │  mysite.iml
│  │  workspace.xml
│  │  
│  └─inspectionProfiles
├─mysite
│      settings.py
│      settings.pyc
│      urls.py
│      urls.pyc
│      wsgi.py
│      wsgi.pyc
│      __init__.py
│      __init__.pyc
```