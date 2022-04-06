# Chapter 2. 운영체제 구조

## 2.1 운영체제 서비스 (Operating-System Services)

운영체제는 프로그램과 그 프로그램의 사용자에게 특정 서비스를 제공한다. 물론 운영체제마다 제공하는 서비스는 다르지만 공통된 부류들이 있다.

- **사용자 인터페이스(user interface):** 거의 모든 운영체제는 사용자 인터페이스(UI)를 제공한다. 일부 시스템들은 아래에서 둘 또는 세 가지 모두의 형태를 제공한다.
  - 그래픽 사용자 인터페이스(GUI, graphical user interface)
    - 마우스: I/O를 지시하고, 메뉴에서 선택하고, 화면을 선택하는 포인팅 장치
    - 키보드: 텍스트 입력장치
  - 터치 스크린 인터페이스(touch-screen interface)
    - 휴대전화 및 태블릿 같은 모바일 시스템에 제공되는 UI 서비스
  - 명령어 라인 인터페이스(CLI, command-line interface)
    - 명령과 이를 입력할 방법(특정 형식의 키보드)가 사용된다.
- 프로그램 수행(program execution): 시스템은 프로그램을 메모리에 적재(load)해 실행할 수 있어야 하며, 프로그램은 정상적이든 비정상적(오류표시)이든 실행을 끝낼 수 있어야 한다.
- **입출력 연산(I/O operation):** 수행 중인 프로그램은 입출력을 요구할 수 있다.
- **파일 시스템 조작(file system manipulation):** 프로그램은 파일을 읽고 쓰고 생성하고 삭제할 수 있다.
- **통신(communication):** 한 프로세스가 다른 프로세스와 정보를 교환할 수 있다.
- **오류 탐지(error detection):** 운영체제는 모든 가능한 오류를 항상 의식하고 있어야 한다.

사용자가 아닌 시스템 자체의 효율적인 동작을 보장하기 위한 운영체제 기능들도 존재한다.

- **자원 할당(resource allocation):** 다수의 프로세스나 다수의 작업이 동시에 실행될 때, 그들 각각에 자원을 할당해 주어야 한다.
  - CPU를 최대한 효율적으로 이용하기 위해 운영체제는 CPU 스케줄링 루틴이 여러가지 요인들을 고려하여 처리한다.
- **기록 작성(logging):** 어떤 프로그램이 어떤 종류의 컴퓨터 자원을 얼마나 많이 사용하는지를 기록한다.
- **보호(protection)와 보안(security):**
  - 보호는 시스템 자원에 대한 모든 접근이 통제되도록 보장하는 것
  - 보안은 네트워크 어댑터 등과 같은 외부 입출력 장치들을 부적합한 접근 시도로 부터 지키고, 침입의 탐지를 위해 모든 접속을 기록한다.

## 2.2 사용자와 운영체제 인터페이스 (User and Operating-System Interface)

사용자가 운영체제와 접촉하는 방식에는 3가지 기본 방식이 있다.

### 2.2.1 명령 인터프리터 (Command-Interpreter)

CLI 또는 명령 인터프리터는 셸(shell)이라고도 부른다.

- Bourne shell(bsh), C shell(csh), Bourne-Again shell(bash), Korn shell(ksh)

### 2.2.2 그래픽 기반 사용자 인터페이스 (Graphical User Interface)

사용자 친화적인 그래픽 기반 사용자 인터페이스(GUI)를 통하는 방식

- 데스크톱(desktop)이라고 특정지어지는 마우스를 기반으로 하는 윈도 메뉴 시스템을 사용
- 사용자는 마우스를 움직여 마우스 포인터를 화면상의 이미지(icons)에 위치시킨다.
- 마우스를 누름으로서 프로그램을 호출하거나 파일 혹은 디렉터리(폴더)를 선택할 수 있다.

### 2.2.3 터치스크린 인터페이스 (Touch-Screen Interface)

사용자는 터치스크린에서 손가락을 누르거나 스와이프 하는 등의 제스처(gesture)를 취하여 상호 작용한다.

## 2.3 시스템 콜 (System Calls)

시스템 콜은 운영체제에 의해 사용 가능하게 된 서비스에 대한 인터페이스를 제공한다.

### 2.3.2 응용 프로그래밍 인터페이스 (Application Programming Interface)

**API(application programming interface)** 는 각 함수에 전달되어야 할 매개변수들과 프로그래머가 기대할 수 있는 반환 값을 포함하여 응용 프로그래머가 사용 가능한 함수들의 집합을 말한다.

- C 언어로 작성된 프로그램을 위해서 Unix와 Linux 시스템에서 제공되는 라이브러리는 libc로 불린다.

단순히 원하는 시스템 콜이 무엇인지보다 더 많은 정보가 요구될 수도 있는데, 이러한 정보가 담긴 매개변수를 운영체제에 전달하기 위해서는 세 가지 일반적인 방법을 사용한다:

1.  매개변수를 레지스터 내에 전달한다. 어떤 경우에는 매개변수의 개수가 레지스터 수 보다 많을 수 있다.
2.  위와 같은 경우에 매개변수는 메모리 내의 블록이나 테이블에 저장되고, 메모리의 주소가 레지스터에 전달된다.
3.  매개변수는 프로그램에 의해 **스택(stack)** 에 **넣어질(push)** 수 있고, 운영체제에 의해 꺼내진다(pop off).

일부 운영체제는 2, 3번의 방식을 선호하는데, 이 이유는 2, 3번의 방식들은 전달되는 매개변수의 개수나 길이를 제한하지 않기 때문이다.

### 2.3.3 시스템 콜의 유형 (Types of System Calls)

시스템 콜은 여섯가지의 중요한 범주로 나눌 수 있다.

#### 2.3.3.1 프로세스 제어 (Process Control)

- 끝내기(end), 중지(abort)
- 적재(load), 실행(execute)
- 프로세스 생성(create process), 프로세스 종료(terminate process)
- 프로세스 속성 획득(get process attribute) 및 프로세스 속성 설정(set process attribute)
- 시간 대기 (wait time)
- 이벤트를 기다림(wait event), 이벤트를 알림(signal event)
- 메모리 할당 및 해제 (malloc, free)

#### 2.3.3.2 파일 조작 (File Manipulation)

- 파일 생성(create file), 파일 삭제(delete file)
- 열기(open), 닫기(close)
- 읽기(read), 쓰기(write), 위치 변경(reposition)
- 파일 속성 획득(get file attributes) 및 파일 속성 설정(set file attributes)

#### 2.3.3.3 장치 관리 (Device Management)

- 장치를 요구(request devices), 장치를 방출(release device)
- 읽기(read), 쓰기(write), 위치 변경(reposition)
- 장치 속성 획득(get device attributes) 및 장치 속성 설정(set device attributes)
- 장치의 논리적 부착(attach) 또는 분리(detach)

#### 2.3.3.4 정보 유지 (Information Maintenance)

- 시간과 날짜의 설정과 획득(get time or date, set time or date)
- 시스템 데이터의 설정과 획득(get system data, set system data)
- 프로세스, 파일, 장치 속성의 획득(get process, file, or device attributes)
- 프로세스, 파일, 장치 속성의 설정(set process, file, or device attributes)

#### 2.3.3.5 통신 (Communication)

- 통신 연결의 생성(create), 제거(delete)
- 메시지의 송신(send), 수신(receive)
- 상태 정보 전달(transfer status information)
- 원격 장치의 부착(attach) 및 분리(detach)

#### 2.3.3.6 보호 (Protection)

- get file permissions
- set file permissions

## 2.4 시스템 서비스 (System Services)

운영체제는 프로그램 개발과 실행을 위해 더 편리한 환경을 제공하도록 도움을 주는 시스템 유틸리티(system utility) 즉, 시스템 서비스를 제공한다. 다음과 같은 범주로 분류할 수 있다.

- **파일 관리:** 파일과 디렉터리를 생성(create), 삭제(delete), 복사(copy), 개명(rename), 인쇄(print), 열거(list) 등 일반적인 조작들을 제공한다.
- **상태 정보:** 시스템의 날짜(date), 시간(time), 사용 가능한 메모리(available memory)와 디스크의 공간의 양(disk space), 사용자 수 등과 같은 상태 정보를 제공한다.
- **파일 변경:** 디스크 혹은 다른 저장장치의 파일의 내용을 생성하고 변경한다. 문장 편집기(text editor)를 사용할 수 있다.
- **프로그래밍 언어 지원:** 일반적인 프로그래밍 언어들(C/C++, Java, Python)에 대한 컴파일러, 어셈블러, 디버거, 인터프리터가 종종 운영체제와 함께 사용자에게 제공되거나 별도로 다운받을 수 있다.
- **프로그램 적재와 실행:** 프로그램이 실행되기 위해서는 메모리에 적재되어야 한다. 그를 위한 절대 로더(absolute loader), 재배치 가능 로더(relocatable loder), 링키지 에디터(linkage editor)와 중첩 로드(overlay loader) 등을 제공할 수 있다.
- **통신:** e-mail, remote login 등을 통해 통신 관련 서비스를 제공한다.
- **백그라운드 서비스:**: 부트할 때 특정 시스템 프로그램을 시작시킨다.

## 2.5 링커와 로더 (Linkers and Loaders)

1. 소스 파일은 임의의 물리 메모리(physical memory) 위치에 적재되도록 설계된 오브젝트 파일(relocatable object file)로 컴파일 된다.
2. **링커(linker)** 가 relocatable object 파일을 하나의 이진 실행(execute) 파일로 결합한다.
3. **로더(loader)** 가 이진 실행 파일을 메모리에 적재(load)하여 CPU 코어에서 실행할 수 있는 상태로 만든다.

위와 같은 링크 및 로드와 관련된 활동은 **재배치(relocation)** 로, 프로그램 부분에 최종 주소를 할당하고 프로그램 코드와 데이터를 해당 주소와 일치하도록 조정하여 프로그램이 실행될 때 코드가 라이브러리 함수를 호출하고 변수에 접근할 수 있게 한다.

동적 링킹 라이브러리(DLL, dynamically linked library)는 프로그램이 적재될 때 라이브러리를 동적으로 링크할 수 있게 하여 같은 라이브러리를 사용하는 파일들은 불러온 라이브러리를 공유할 수 있게 된다. (메모리 사용을 크게 절약 가능)

## 2.8 운영체제 구조 (Operating-System Structures)

### 2.8.1 모놀리식 구조 (Monolithic Structure)

커널(kernel)과 시스템 프로그램(system programs) 두 부분으로 구성된다. 모든 명령들이 kernel이 많은 권한을 가지고 있기 때문에, 많은 명령어들이 커널에서 처리된다. 많은 부분이 커널로 이루어지기 때문에 구현 및 확장하기 힘들다. 하지만 시스템 콜 인터페이스에는 오버헤드가 거의 없고, 커널 안에서의 통신 속도가 빠르다는 성능 면에서의 뚜렷한 이점 때문에 여전히 UNIX, Linux, Windows 운영체제에서 이 구조를 발견할 수 있다.

### 2.8.2 계층적 접근 (Layered Approach)

시스템의 한 부분을 변경하면 다른 부분에 광범위한 영향을 주는 밀접하게 결합된(tightly coupled) 모놀리식에 대한 대안으로 느슨하기 결합된(loosely coupled) 시스템이다. 운영체제가 몇 개의 layer로 나뉘어져 있고, 각 layer마다 독립적으로 작동한다. layer들이 각자 독립적이기 때문에 구현과 디버깅이 간단하다. 하지만 각 계층의 기능을 적절히 정의해야 하는 문제가 있고 사용자 프로그램이 여러 계층을 통과해야 운영체제 서비스를 얻을 수 있어 성능면에서도 열악하다.

### 2.8.3 마이크로커널 (Microkernels)

모든 중요치 않은 구성요소를 커널로부터 제거하고, 그들을 별도의 주소 공간에 존재하는 사용자 수준 프로그램으로 구현하여 운영체제를 구현하는 방법이다. 모든 새로운 서비스는 사용자 공간에 추가되기 때문에 커널을 변경할 필요가 없어 운영체제의 확장이 쉽다. 또한 서비스 대부분이 사용자 프로세스로 실행되기 때문에 더욱 높은 보안성과 신뢰성을 제공한다.

- macOS, iOS의 커널 구성요소인 Darwin

### 2.8.4 모듈 (Moduless)

적재가능 커널 모듈(loadable kernel modules, LKM)기법에서는 커널은 핵심적인 구성요소의 집합을 가지고 있고, 부팅 때 또는 실행 중에 부가적인 서비스들을 모듈을 통하여 링크할 수 있다. 커널의 각 부분이 정의되고 보호된 인터페이스를 가진다는 점에서는 계층 구조를 닮았으나, 모듈에서 임의의 다른 모듈을 호출할 수 있다는 점에서 계층구조보다 유연하다. 중심 모듈은 단지 핵심 기능만을 가지고 있고 다른 모듈의 적재 방법과 모듈들과 어떻게 통신하는지 안다는 점에서 마이크로커널과 유사하지만, 통신하기 위해 메세지 전달을 호출할 필요가 없기 때문에 더 효율적이다.

### 2.8.5 하이브리드 시스템 (Hybrid System)

다양한 구조를 결합하여 성능, 보안 및 편리성 문제를 해결하려는 혼용 구조이다. 엄격하게 정의된 하나의 구조를 채택한 운영체제는 거의 없고 하이브리드 구조를 많이 사용한다.

## 2.9 운영체제 빌딩과 부팅 (Building and Booting an Operating System)

### 2.9.1 운영체제 생성 (Operating-System Generation)

운영체제를 처음부터 생성(또는 빌딩)하는 경우 다음 절차를 밟아야 한다.

1. 운영체제 소스 코드를 작성한다. (또는 이전에 작성된 소스코드를 확보한다.)
2. 운영체제가 실행될 시스템의 운영체제를 구성한다.
   - 어떤 기능이 포함되는지 명시한다.
3. 운영체제를 컴파일 한다.
4. 운영체제를 설치한다.
5. 컴퓨터와 새 운영체제를 부팅한다.

### 2.9.2 시스템 부트 (System Boot)

커널을 적재하여 컴퓨터를 시작하는 과정을 시스템 부팅(system booting)이라고 한다.

1. 부트스트랩 프로그램(bootstrap program) 또는 부트 로더(boot loader)라고 불리는 작은 코드가 커널의 위치를 찾는다.
2. 커널이 메모리에 적재되고 시작된다.
3. 커널은 하드웨어를 초기화한다.
4. 루트 파일 시스템이 마운트 된다.
