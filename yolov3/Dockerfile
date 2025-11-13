# 1. 기본 이미지 설정 (PDF 28페이지 'FROM' 개념 활용) [cite: 488]
# 과제에서 C 코드 컴파일(make)이 필요하므로 [cite: 751] 개발 도구가 포함된 ubuntu 환경을 사용합니다.
FROM ubuntu:latest

# 2. 패키지 설치 (PDF 28페이지 'RUN' 개념 활용) [cite: 489-494]
# 과제 튜토리얼에 필요한 git [cite: 750], make [cite: 751] (build-essential에 포함),
# wget [cite: 753] 을 설치합니다.
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    git \
    wget \
    && rm -rf /var/lib/apt/lists/*

# 3. 소스 코드 복제 (PDF 28페이지 'RUN git clone' 개념 활용) [cite: 495]
# 과제에서 요구하는 darknet 코드를 클론합니다. [cite: 750]
RUN git clone https://github.com/pjreddie/darknet /darknet

# 4. 작업 디렉토리 변경 (PDF 28페이지 'WORKDIR' 개념 활용) [cite: 504]
# 이후 명령어들을 /darknet 폴더 내에서 실행하도록 설정합니다. [cite: 750]
WORKDIR /darknet

# 5. 프로젝트 빌드 (PDF 28페이지 'RUN make' 개념 활용) [cite: 499]
# 과제 튜토리얼의 'make' 명령을 실행합니다. [cite: 751]
RUN make

# 6. Yolo 가중치 파일 다운로드 (RUN 및 wget 활용)
# 과제 튜토리얼의 'wget' 명령을 실행합니다. [cite: 753]
RUN wget https://data.pjreddie.com/files/yolov3.weights

# 7. 실행 스크립트 생성 및 ENTRYPOINT 설정 (PDF 28페이지 'ENTRYPOINT' 개념 활용) [cite: 505]
# 과제의 최종 목표는 'docker run [이미지] [URL]' 형태로 실행하는 것입니다. 
# 이 URL을 인자로 받아 처리하기 위해, 내부에서 2단계(다운로드 > 탐지)로 동작하는 
# 셸 스크립트(/run_yolo.sh)를 만들고 이것을 ENTRYPOINT로 지정합니다.

# 7-1. 스크립트 생성
RUN echo '#!/bin/bash' > /run_yolo.sh && \
    # 첫 번째 인자($1)로 들어온 URL을 input.jpg로 다운로드 [cite: 770, 774]
    echo 'wget -O input.jpg $1' >> /run_yolo.sh && \
    # 다운로드한 파일로 darknet 탐지 실행 [cite: 754]
    echo './darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights input.jpg' >> /run_yolo.sh && \
    # 스크립트 실행 권한 부여
    chmod +x /run_yolo.sh

# 7-2. 엔트리포인트 지정
ENTRYPOINT ["/run_yolo.sh"]
