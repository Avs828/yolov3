
FROM ubuntu:latest #ubuntu로 환경을 세팅.

#2. 패키지 사용에 필요한  git, wget 프로그램들을  다운로드 할 레시피 설정 (이 부분은 수업자료 ppt참고)
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

#yolov3에 필요한 git hub 끌고 오기 (darknet)
RUN git clone https://github.com/pjreddie/darknet /darknet

# darknet안으로 실행 위치 명시적으로 지정 .
WORKDIR /darknet

#darknet 빌드 .
RUN make

# Yolo 가중치 파일 다운로드
RUN wget https://data.pjreddie.com/files/yolov3.weights


# 이부분은 gemini를 활용해서 .sh파일을 제작했습니다.
RUN echo '#!/bin/bash' > /run_yolo.sh && \ #기존 리눅스 명령어인 echo를 사용해서 "run_yolo.sh"에 #!/bin/bash에 쓴다, 즉 bash 셀로 실행하는 명령어를 터미널 셀 스크립트 첫번째에 추가한다. 
   
    echo 'wget -O input.jpg $1' >> /run_yolo.sh && \ # 두번째 줄에 입력 받는 다음 인자를 다운로드 한다. 라는 의미의 명령어를 센 스크립트에 추가했다.  wget은 수업시간에 배운 다운로드 명령어,  input .jpg로 저장하고, 여기 명령어의 핵심은 '$1'이라는  첫번째 입력값을 의미하는 것이다.
    # 다운로드한 파일로 darknet 탐지 실행 한다.
    echo './darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights input.jpg' >> /run_yolo.sh && \
    # 스크립트 실행 권한 부여
    chmod +x /run_yolo.sh

# 7-2. 엔트리포인트 지정==> 무조건 실행 되야 할 고정 프로그램 .
ENTRYPOINT ["/run_yolo.sh"]
