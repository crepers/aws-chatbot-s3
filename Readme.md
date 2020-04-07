# AWS Chatbot

AWS Chatbot은 Slack 채널 및 Amazon Chime 채팅방을 통해 AWS 리소스를 쉽게 모니터링하고 상호 작용하도록 지원하는 대화형 에이전트입니다. AWS Chatbot을 활용하면 알림을 수신하고 명령을 통해 진단 정보를 반환하고, AWS Lambda 함수를 호출하고 AWS 지원 케이스를 생성하여 팀이 더 빠르게 협업하고 이벤트에 대응할 수 있습니다.
[AWS Chatbot](https://aws.amazon.com/ko/chatbot/)

### S3 API에 대한 모니터링
- S3 특정 버킷에 작업이 있는 경우 해당 내용을 AWS Chatbot 을 이용하여 내용을 전달할 수 있습니다.
- Architecture
![AWS Chatbot](Chatbot-S3.jpg)

### 사용서비스
- AWS Chatbot
- AWS CloudTrail
- Amazon S3
- Amazon CloudWatch
- Amazon SNS
- Amazon Chime / Slack

### SNS
1. Create Topic
   1. Name
   2. Create topic 선택 버튼

### AWS Chatbot
1. Configuare new webhook
   1. Name
   2. Webhook URL
      1. 채팅룸/Slack 채널 생성
      2. Chime : [채팅룸에 Webhook 추가](https://docs.aws.amazon.com/ko_kr/chime/latest/ug/webhooks.html)
      3. Slack
         1. Slack에서 Permission 설정
         2. Channel type : public
         3. channel 선택
   3. Permissions 설정
      1. IAM Role
      2. Policy template
   4. SNS Topic 선택
   
### CloudTrail
Amazon S3은 Amazon S3에서 사용자, 역할 또는 AWS 서비스가 수행한 작업의 레코드를 제공하는 서비스인 AWS CloudTrail과 통합됩니다. CloudTrail은 Amazon S3 콘솔의 호출 및 Amazon S3 API에 대한 코드 호출의 호출을 포함하여 Amazon S3에 대한 API 호출의 하위 세트를 이벤트로 캡처합니다. 추적을 생성하면 CloudTrail 이벤트를 비롯하여 Amazon S3 이벤트를 Amazon S3 버킷으로 지속적으로 배포할 수 있습니다.
[AWS CloudTrail을 사용하여 Amazon S3 API 호출 로깅](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/dev/cloudtrail-logging.html)
1. Create trail
   1. Trail name 
   2. Data event : S3
   3. Add S3 bucket : **테스트를 위한 특정 버킷, Read, Write 선택**
   4. Storage location
      1. Create a new S3 bucket : No
      2. S3 bucket : 로그를 기록할 버킷 선택

### CloudWatch
1. Event Rules
   1. Event Pattern
      1. Service Name : Simple Storage Service(S3)
      2. Event Type : Object Level Operations
      3. Any Operation
      4. Specific bucket(s)  by Name : **테스트를 위한 특정 버킷 선택**

2. Alarm
   1. Step 1 : Specify metric and conditions
      1. Create alarm > select metric
      2. Events > By Rule Name : **생성한 Event Rule - Invocations 선택**
      3. Conditions
         1. Threshold : static
         2. Whenever Invocations is... : Greater
         3. than... : **0** 
         > 실행시마다 호출하기 위해 0으로 설정
   2. Step 2 : Confifure actions
      1. Send a norification to... : **SNS topic 선택**
   3. Step 3 : Add name and description
      1. Alarm name
   4. Step 4 : Preview and create 

### Test
1. S3
   1. Console > S3 > **특정 버킷**
   2. Upload
2. Chime / Slack
   ![대화 확인](Chime-Chatbot.jpg)