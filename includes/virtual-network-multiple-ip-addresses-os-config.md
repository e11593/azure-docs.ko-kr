## <a name="a-nameos-configaadd-ip-addresses-to-a-vm-operating-system"></a><a name="os-config"></a>VM 운영 체제에 IP 주소 추가

여러 개인 IP 주소를 사용하여 만든 VM에 연결 및 로그인 VM에 추가한 모든 개인 IP 주소(기본 포함)를 수동으로 추가해야 합니다. VM 운영 체제에 대한 다음 단계를 완료합니다.

### <a name="windows"></a>Windows

1. 명령 프롬프트에서 *ipconfig /all*을 입력합니다.  *기본* 개인 IP 주소(DHCP를 통한)만 표시됩니다.
2. 명령 프롬프트 창에 *ncpa.cpl*을 입력하여 **네트워크 연결** 창을 엽니다.
3. **로컬 영역 연결**에 대한 속성을 엽니다.
4. IPv4(인터넷 프로토콜 버전 4)를 두 번 클릭합니다.
5. **다음 IP 주소 사용**을 선택하고 다음 값을 입력합니다.

    * **IP 주소**: *기본* 개인 IP 주소를 입력합니다.
    * **서브넷 마스크**: 서브넷을 기준으로 설정됩니다. 예를 들어 서브넷이 /24이면 서브넷 마스크는 255.255.255.0입니다.
    * **기본 게이트웨이**: 서브넷의 첫 번째 IP 주소입니다. 서브넷이 10.0.0.0/24이면 게이트웨이 IP 주소는 10.0.0.1입니다.
    * **다음 DNS 서버 주소 사용** 을 클릭하고 다음 값을 입력합니다.
        * **기본 설정 DNS 서버:** 자체 DNS 서버를 사용하지 않는 경우 168.63.129.16을 입력합니다.  자체 DNS 서버를 사용하는 경우 서버의 IP 주소를 입력합니다.
    * **고급** 단추를 클릭하고 추가 IP 주소를 추가합니다. 8단계에 나열된 각 보조 개인 IP 주소를 기본 IP 주소에 대해 지정된 것과 동일한 서브넷을 갖는 NIC에 추가합니다.
    * **확인**을 클릭하여 TCP/IP 설정을 닫은 다음 **확인**을 다시 클릭하여 어댑터 설정을 닫습니다. RDP 연결이 다시 설정됩니다.
6. 명령 프롬프트에서 *ipconfig /all*을 입력합니다. 추가한 모든 IP 주소가 표시되고 DHCP는 해제됩니다.
    
### <a name="linux-ubuntu"></a>Linux(Ubuntu)

1. 터미널 창을 엽니다.
2. 루트 사용자인지 확인합니다. 아닌 경우 다음 명령을 입력합니다.

    ```bash
    sudo -i
    ```

3. 네트워크 인터페이스(‘eth0’이라고 가정)의 구성 파일을 업데이트합니다.

    * dhcp에 대한 기존 줄 항목을 유지합니다. 기본 IP 주소가 이전에 구성된 대로 유지됩니다.
    * 다음 명령을 사용하여 추가 정적 IP 주소에 대한 구성을 추가합니다.

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    .cfg 파일이 표시됩니다.
4. vi *filename*을(를) 실행하여 파일을 엽니다.

    파일 끝에 다음 줄이 있어야 합니다.

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. 이 파일에 있는 줄 뒤에 다음 줄을 추가합니다.

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    ```

6. 다음 명령을 실행하여 파일을 저장합니다.

    ```bash
    :wq
    ```

7. 다음 명령을 사용하여 네트워크 인터페이스를 다시 설정합니다.

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > 원격 연결을 사용하는 경우 같은 줄에서 ifdown 및 ifup을 둘 다 실행합니다.
    >

8. 다음 명령을 사용하여 네트워크 인터페이스에 IP 주소가 추가되는지 확인합니다.

    ```bash
    Ip addr list eth0
    ```

    목록의 일부로 추가한 IP 주소가 표시되어야 합니다.
    
### <a name="linux-redhat-centos-and-others"></a>Linux(Redhat, CentOS 및 기타)

1. 터미널 창을 엽니다.
2. 루트 사용자인지 확인합니다. 아닌 경우 다음 명령을 입력합니다.

    ```bash
    sudo -i
    ```

3. 암호를 입력하고 화면 지시를 따릅니다. 루트 사용자인 경우 다음 명령을 사용하여 네트워크 스크립트 폴더로 이동합니다.

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. 다음 명령을 사용하여 관련 ifcfg 파일을 나열합니다.

    ```bash
    ls ifcfg-*
    ```

    *ifcfg-eth0* 이 파일 중 하나로 표시되어야 합니다.

5. 다음 명령을 사용하여 *ifcfg-eth0* 파일을 복사하고 *ifcfg-eth0:0*이라고 이름을 지정합니다.

    ```bash
    cp ifcfg-eth0 ifcfg-eth0:0
    ```

6. 다음 명령을 사용하여 *ifcfg-eth0:0* 파일을 편집합니다.

    ```bash
    vi ifcfg-eth0:0
    ```

7. 다음 명령을 사용하여 이 파일에서 장치 이름을 적절히 변경합니다(이 경우 *eth0:0* ).

    ```bash
    DEVICE=eth0:0
    ```

8. IP 주소를 반영하도록 *IPADDR = YourPrivateIPAddress* 줄을 변경합니다.
9. 다음 명령을 실행하여 파일을 저장합니다.

    ```bash
    :wq
    ```

10. 다음 명령을 실행하여 네트워크 서비스를 다시 시작하고 변경이 성공적으로 수행되었는지 확인합니다.

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    반환된 목록에서 추가한 IP 주소 *eth0:0*이 표시되어야 합니다.


<!--HONumber=Dec16_HO1-->


