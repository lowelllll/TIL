# log
>  컴퓨터나 서버(Server) 등에서 유저(User)의 플레이 정보를 시간에 따라 남기는 기록을 뜻함.

 로그를 남기는 데에는 두 가지 목적이 있음.
 1. 진단용 로그  
    애플리케이션 작동에 관한 이벤트 기록. 
    예를 들어, 사용자가 오류 보고서를 남기면 로그에서 해당 오류의 문맥을 파악할 수 있음.
2. 감시용 로그  
    비지니스 분석에 사용되는 로그.  
    사용자의 거래를 추출하여 다른 세부정보와 결합하여 보고서를 작성하거나 업무를 최적화할 수 있음.

### 로그가 출력보다 좋은 이유
- 로그 레코드는 로그를 남기는 이벤트마다 생성되며, 해당 이벤트의 파일 이름, 전체 경로, 함수, 코드 줄 번호와 같이 진단에 도음이 되는 정보가 포함됨.
- 내장 모듈에서 발생한 이벤트도 로그가 남는데 이 로그들을 루트 로그 기록기를 통해 애플리케이션의 로그 스트림으로 자동으로 보낼 수 있음.
- `logging.Logger.setLevel()`을 사용해 로그를 선택적으로 남길 수 있음.  
로그 기록을 중지하려면 `logging.Logger.disabled`를 True로 설정함.