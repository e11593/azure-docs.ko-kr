### <a name="prepare-for-push-install-on-linux-servers"></a>Linux 서버에 강제 설치 준비

1. Linux 컴퓨터와 프로세스 서버 간에 네트워크가 연결되어 있는지 확인합니다.
2. 프로세스 서버가 컴퓨터에 액세스하는 데 사용할 수 있는 계정을 만듭니다. 계정은 원본 Linux 서버의 **루트** 사용자여야 하며, 강제 설치/업데이트에만 사용됩니다.
3. 원본 Linux 서버의 `/etc/hosts` 파일에 로컬 호스트 이름을 모든 네트워크 어댑터와 연결된 IP 주소에 매핑하는 항목이 포함되어 있는지 확인합니다.
4. 복제할 컴퓨터에 최신 openssh, openssh-server, openssl 패키지를 설치합니다.
5. 포트 22에 SSH를 사용하고 실행 중인지 확인합니다.
6. 다음과 같이 sshd_config 파일에서 SFTP 하위 시스템 및 암호 인증을 사용하도록 설정합니다.
  - **루트**로 로그인합니다.
  - /etc/ssh/sshd_config 파일에서 **PasswordAuthentication**으로 시작하는 줄을 찾습니다.
  - 줄의 주석 처리를 제거하고 값을 **아니요**에서 **예**로 변경합니다.
   6.4 **Subsystem**으로 시작하는 줄을 찾아서 주석 처리를 제거합니다.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

7. CSPSConfigtool에서 만든 계정을 추가합니다.

    - 구성 서버에 로그인 합니다.
    - **cspsconfigtool.exe**를 엽니다. 바탕 화면에서 바로 가기로 사용할 수 있으며, %ProgramData%\home\svsystems\bin 폴더에 있습니다.
    - **계정 관리** 탭에서 **계정 추가**를 클릭합니다.
    - 만든 계정을 추가합니다. 계정을 추가한 후 컴퓨터에 복제를 사용하도록 설정할 때 이 자격 증명을 제공해야 합니다.


<!--HONumber=Jan17_HO3-->


