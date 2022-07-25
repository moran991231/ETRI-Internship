# 07/18 월
박사님이 교육가셔서 아침에는 할 일이 없었다.  
인사과에 전화해서 8월 휴가를 당겨쓰는 것에 대해서 물어봤다. 메일로 사유를 보내자 흔쾌히 휴가를 지급해 주셔서 결재상신했다.  
시뮬레이터 환경설정을 해보라는 업무를 받았다.

## Habitat-Sim
매년 대회가 열려서 도커 이미지를 배포하는 것 같다. 도커 이미지 설치는 비교적 쉽게 이루어졌다.
[Habitat-lab GitHub참고 (도커파일이용)](https://github.com/facebookresearch/habitat-lab)  
호스트에서 깃 클론 및 도커 이미지 빌드
```
git clone https://github.com/facebookresearch/habitat-lab.git
cd habitat-lab
docker build -t habitat-lab .
docker run -it  -e DISPLAY=unix$DISPLAY  -e USER=$USER -e XDG_RUNTIME_DIR=/tmp -v /root/.Xauthority:/root/.Xauthority -v /tmp/.X11-unix:/temp/.X11 --privileged --gpus all --net=host --runtime=nvidia --name habitat-lab habitat-lab
```

1. 도커 컨테이너 내 bash: 필요 패키지 설치 및 빌드
```
source activate habitat
cd /habitat-sim
./build.sh
pip install -r requirements.txt
python setup.py install

cd /habitat-lab
pip install -r requirements.txt
python setup.py develop --all
```


2. 도커 컨테이너 내 bash: 시뮬레이터 테스트  
[Habitat-sim GitHub 참고](https://github.com/facebookresearch/habitat-sim)
```
cd /habitat-sim
python examples/example.py
```
텍스트 출력이 왕창 뜬다.


3. 도커 컨테이너 내 bash: 시뮬레이터 테스트2
```
cd /habitat-sim
python -m habitat_sim.utils.datasets_download --uids habitat_test_scenes --data-path ./data/
python -m habitat_sim.utils.datasets_download --uids habitat_example_objects --data-path ./data/
python examples/viewer.py --scene /path/to/data/scene_datasets/habitat-test-scenes/skokloster-castle.glb
```  
`python examples/viewer.py ... `에서 오류가 뜰 수 있다. import 경로 에러인데 `examples/viewer.py`파일을 수정해야 한다.  
`from examples.settings` 를 `from settings`로 바꾸면 된다.



* Alternative Python
```
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
$ sudo update-alternatives --config python
```

# 07/19 화

AI2-THOR 시뮬레이터 설치하기
1. ai2thor 깃헙 참고해서 도커에 pip나 conda를 이용해서 설치하기 => 실패
2. ai2thor를 이용한 챌린지인 [alfred의 깃헙](https://github.com/askforalfred/alfred)을 참고해서 도커파일 빌드

* Dockerfile 에러: setuptools 버전 에러 Dockerfile에서 setuptools 설치 부분을 찾은 뒤 다음과 같이 바꾼다. `RUN pip install -U 'setuptools<45.0.0'`

* 깃헙 리드미를 따라가다 보면 갖은 오류를 발견할 수 있다.  
```$ sh download_data.sh json```
컨테이너에 7z가 깔려 있지 않다고 오류를 뿜는다.  
[https://www.7-zip.org/download.html](https://www.7-zip.org/download.html)에 들어가서 `64-bit Linux x86-64`용 파일을 wget으로 컨테이너 안에 받는다. 압축을 해제한 후 실행파일 `7zz`와 `7zzs`를 `/bin`폴더로 옮긴다.
```
wget https://www.7-zip.org/a/7z2201-linux-x64.tar.xz 
tar -xf 7z2201-linux-x64.tar.xz 
sudo mv 7zz* /bin/
```
그 다음에 download_data.sh을 편집해야 한다. 문서의 앞 부분에 `Install 7z` 단락을 모두 주석처리한다.  
그 다음에 본문에 `7z`를 이용해서 압축을 푸는 명령어 들을 모두 `7zz`로 바꾼다. 
예시: `7z x json_2.1.0.7z -y && rm json_2.1.0.7z` 을 `7zz x json_2.1.0.7z -y && rm json_2.1.0.7z`로 바꾼다.




* 패스워드: scripts/docker_build.py를 보면 컨테이너 내 계정을 만드는데 계정 이름은 우분투에 로그인된 사용자의 이름과 같고 비밀번호는 `password`이다.


* `apt-get update` 시  `Couldn't create temporary file /tmp/apt.conf.rVavhs for passing config to apt-key` 문제  
`/tmp`의 권한이 없어서 그렇다. `chmod 777 /tmp`해주면 된다.

* openssl 설치 (할 필요 없음)
```
# oppenssl.org/source에서 다운로드할 파일 링크 찾기
wget https://www.openssl.org/source/openssl-1.1.1q.tar.gz
tar xf openssl-1.1.1q.tar.gz 
cd openssl-1.1.1q
./config shared
make
make install
ldconfig
openssl
# 에러가 안 뜨고 출력이 아래와 같으면 성공
OpenSSL>

```



# 07/20 수
도커에서 alfred를 돌려보려고 했으나 뒤질것 같다.
서버 컴의 그래픽카드가 RTX A6000이다. (엄청 비싼 거)  
이 장비의 ARCH?는 SM_86이어서 cuda 10.2를 사용하는 torch로는 이를 실행할 수 없다.
그래서 cuda 11.3를 사용하는 torch를 써야 하는데 이것은 Python>=3.8을 사용한다.
우분투18.04의 기본 Python은 3.6이어서 Python3.9를 새로 깔았다.
마음대로 깔리지 않아서 소스로부터 빌드했다.
그런데 그렇게 해서 깐 python은 ssl 연결이 안 되어서 `python3.9 -m pip instal ...`이 모두 동작하지 않았다.

# 07/21 목
```
apt-get install libssl-dev
```
위 커맨드를 치니 python3.9의 pip 문제가 해결된 듯 하다.  
새로운 도커 컨테이너를 만들어서 깔끔하게 해결해 보자!

도커 이미지는 nvidia/cuda:11.3-cudnn8-devel-ubuntu18.04 사용
https://hub.docker.com/r/nvidia/cuda/tags?page=1&name=11.3.0-cudnn8-devel-ubuntu
1. 필요한 패키지 다운로드

<init_install.sh>
```
#!/bin/bash

set -euxo pipefail

apt-get update
DEBIAN_FRONTEND=noninteractive apt install --no-install-recommends \
  curl \
  terminator \
  tmux \
  vim \
  libsm6 \
  libxext6 \
  libxrender-dev \
  gedit \
  git \
  openssh-client \
  unzip \
  htop \
  libopenni-dev \
  apt-utils \
  usbutils \
  dialog \
  ffmpeg \
  nvidia-settings \
  libffi-dev \
  flex \
  bison \
  build-essential \
  git \
  wget \
  module-init-tools \
  pciutils \
  xserver-xorg \
  xserver-xorg-video-fbdev \
  xauth \
  libssl-dev \
  zlib1g-dev \
  libbz2-dev \
  liblzma-dev

apt-get update && apt-get install -y \
   mesa-utils libgl1-mesa-glx && \
rm -rf /var/lib/apt/lists/*
```

2. Python3.9 빌드
```
cd /tmp
wget "Python 소스 코드 다운 링크"
tar xf Python-3.9.X.tgz
cd Python-3.9.X

./configure
make
make altinstall

python3.9 -V # 버전 확인해서 잘 나오면 성공한 것!
python3.9 -m pip install --upgrade pip
```

3. 가상환경만들기
```
cd ~ # 혹은 원하는 디렉토리
python3.9 -m venv alfred_env
source ~/alfred_env/bin/activate # 가상환경 활성화

# deactivate # 가상환경 비활성화
```

3. Python 패키지 설치
가상환경을 활성화 한 상태에서 깐다.
```
python -V # Python 3.9 버전이 맞는지 확인한다.
python -m pip install --upgrade pip

python -m pip install <패키지...>
```
이렇게 하는 것을 권장한다.   

<requirements.txt>
```
numpy
pandas
opencv-python
networkx
h5py
tqdm
vocab
revtok
Pillow
ai2thor==2.1.0
tensorboardX
```

```
python -m pip install -r requirements.txt
```

```
python -m pip install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cu113
```

4. alfred 따라하기
7zz를 설치하자
```
wget https://www.7-zip.org/a/7z2201-linux-x64.tar.xz 
tar -xf 7z2201-linux-x64.tar.xz 
sudo mv 7zz* /bin/
```



# 07/22 금
