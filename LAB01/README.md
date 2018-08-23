# FORTIGATE v6.0 Hands On
## LAB 01

### ***Índice***

* [Objetivo](#objetivo)
* [Topologia do LAB](#topologia-do-lab)
	* [Credenciais de acesso](#credenciais-de-acesso)
* [Comandos utilizados no LAB](#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 01](#atividades-do-lab-01)
	* [Tarefa 01](#tarefa-01)
	* [Tarefa 02](#tarefa-02)
	
### ***Objetivo***
Neste lab o aluno será apresentado as configurações iniciais do ***FORTIGATE***, assim como aos comandos básicos para configurar a interface de gerencia via *CLI*.

O aluno deve seguir os passos abaixo para poder configurar os ***FG*** dos *Sites A* e *B*.

### ***Topologia do LAB***

![topologia do lab 01 ](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/TOPOLOGIA%20-%20LAB%2001.png)

#### ***FORTIGATES***
HOSTNAME | Port1 | Port2 | Port3
:-------:|:-----:|:-----:|:-----:
FG_A|**(WAN1)** *192.168.100.1/24*|**(LAN)** *10.1.40.2/24*<br/>**(DMZ)** *172.16.0.254/24*|**(WAN2)** *192.168.110.1/24*
FG_B|**(WAN1)** *192.168.200.1/24*|**(LAN)** *10.2.40.2/24*

#### ***ROTEADORES***
HOSTNAME | e0/0 | e0/1 | 0/2 | e0/3
:-------:|:----:|:----:|:---:|:---:
ISP_A|*192.168.1.**253**/24*|*192.168.100.**254**/24*|*192.168.110.**254**/24*|-
ISP_B|*192.168.2.**253**/24*|*192.168.200.**254**/24*|-|-
INTERNET|*192.168.0.**254**/24*|*DHCP CLIENT*|*192.168.1.**254**/24*|*192.168.2.**254**/24*

#### ***SWITCHES***
HOSTNAME | e0/0 | e0/1 | 0/2 | e0/3|VLAN10|VLAN20|VLAN40
:-------:|:----:|:----:|:---:|:---:|:----:|:----:|:----:
CORE_A|***TRUNK***<br/>*NATIVE*(**V1**)<br/>*TAGGED*(***V30,40***)|***ACCESS***<br/>*VLAN **20***|***ACCESS***<br/>*VLAN **10***|***ACCESS***<br/>*VLAN **30***|*10.1.10.**254**/24*|*10.1.20.**254**/24*|*10.1.40.**1**/24*
CORE_B|***ACCESS***<br/>*VLAN **40***|***ACCESS***<br/>*VLAN **20***|-|-|-|*10.2.10.**254**/24*|*10.2.40.**1**/24*

#### ***HOSTS***
HOSTNAME | SISTEMA | SERVIÇO | e0 | NAT
:-------:|:-------:|:-------:|:--:|:---:
CLIENTE_A|MINT 19|-|*10.1.20.**101**/24*|-
SERVER1|UBUNTU 18.04|*HTTP*|*10.1.10.**10**/24*|-
SERVER2|UBUNTU 18.04|*HTTP*|*172.16.0.**10**/24*|*192.168.100.**10***
CLIENTE_B|MINT 19|-|*10.2.20.**101**/24*|-
CLIENTE_EXTERNO|MINT 19|-|*192.168.0.**101**/24*|-

Os roteadores **ISP_A**, **ISP_B** e **INTERNET**, os switches **CORE_A** e **CORE_B** estão pré-configurados e o aluno não precisa realizar nenhuma intervenção de configuração nestes equipamentos, entretanto é permitido usar comandos *show*, *ping* e *tracerouter*.

Os firewalls **FG_A** e **FG_B** não estão com qualquer configuração, nestes os alunos podem executar qualquer comando.

Os hosts **CLIENTE_A**, **CLIENTE_B**, **SERVER1**, **SERVER2** e **CLIENTE_EXTERNO** não tem nenhuma limitação, apesar de que a única configuração necessária em um LAB limpo, seria alterar o hostname padrão, nestes o cliente pode fazer atividade.

#### ***Credenciais de acesso***

##### ***ROTEADORES & SWITCHES***
***Username*** = *aluno*  
***Password*** = *aluno*

##### ***LINUX***
***Username*** = *pinetech*  
***Password*** = *P@ssw0rd!*

##### ***FORTIGATE***
***Username*** = *admin*  
***Password Padrão*** = *sem senha*  
***Password a configurar*** = *P@ssw0rd!*

### ***Comandos utilizados no LAB***

Abaixo alguns dos comandos necessários para executar o LAB, com as devidas explicações:

#### ***CISCO***
COMANDO | DESCRIÇÃO
:-------|:---------
*show running-config **view full***|Mostra a configuração atual do equipamento (o view full só é necessário quando utilizada a funcionalidade *Parser View*, que o caso deste LAB)
*ping **ip_address***|Executa um ping para o ***ip_address*** especificado
*traceroute **ip_address***|Executa um traceroute para o ***ip_address*** especificado
*show ip route*|Exibe a tabela de rotas do equipamento
*show ip interface brief*|Exibe o endereço IP das interfaces do equipamento
*show vlan*|Exibe as VLANs e as portas associadas as mesmas (somente switch)

#### ***LINUX***
COMANDO | DESCRIÇÃO
:-------|:---------
*sudo hostnamectl set-hostname **new_hostname***|Altera o hostname do equipamento para o ***new_hostname*** 
*ip a*|Exibe a configuração IP das interfaces do equipamento
*ping **ip_address***|Executa um ping para o ***ip_address*** especificado
*traceroute **ip_address***|Executa um traceroute para o ***ip_address*** especificado
*route*|Exibe a tabela de rotas do equipamento
*sudo systemctl restart NetworkManager.service*|Recarrega as configurações de rede no ***MINT 19***
*sudo netplan apply*|Recarrega as configurações de rede no ***UBUNTU 18.04***

#### ***FORTIOS***
COMANDO | DESCRIÇÃO
:-------|:---------
*execute factoryreset*|Apaga toda a configuração do equipamento, e volta ao estado padrão de fábrica
*execute reboot*|Reinicia o equipamento
*execute shutdown*|Desliga o equipamento
*config system global*|Entra na configuração Globa do Systema
*set hostname **new_hostname***|Altera o hostname do equipamento para ***new_hostname*** (dentro do System Global)
*end*|Sai e aplica as configurações
*abort*|Sai e descarta as alterações
*show*| (dentro do config) Exibe as configurações do módulo especifico
*show*| (fora do config) Exibe toda a configuração do equipamento
*show **módulo***| (fora do config) Exibe a configuração do ***módulo*** especificado
*get **módulo*** | (fora do config) Obtém e exibe valores de processos do equipamento

## ATIVIDADES DO LAB 01
### Tarefa 01

O aluno deve executar as configurações iniciais no ***FG_A*** via *CLI*, para obter acesso a interface *WEB* do equipamento.

1. O aluno deve acessar o LAB 01.

2. Depois deve clicar em ***More actions***, e depois ***Start all nodes***

3. Aguardar os equipamentos mudarem da cor cinza para azul.

4. ***Clicar no icone do FG_A***.

![FG_A tela de login](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/FG_A-login.png)

5. Deve abrir uma sessão de terminal com o *prompt* de *login* do roteador ***FortiGate-VM64-KVM login:***

6. Efetue o login com as credenciais fornecidas acima.

7. Execute o script abaixo para alterar o Hostname:

>
	# Entrar no modo de configuração Global do Systema
    #
	config system global
    #
	# Altera o hostname para FG_A
    #
    set hostname FG_A
    #
    # Aplicar a configuração e sai
    #
    end
    
8. Entra no modo de cofniguração de Interfaces e verificar quais portas estão disponíveis no equipamento

>
	# Entrar no modo de configuração de Interfaces
    #
    config system interface
    #
    # Exibir a configuração das interfaces
    #
    show

Compare com a saída de exemplo abaixo:

>
    # Compare com a saída abaixo:
    #
    FG_A (interface) # show
    config system interface
        edit "port1"
            set vdom "root"
            set mode dhcp
            set allowaccess ping https ssh http fgfm
            set type physical
            set snmp-index 1
        next
        edit "port2"
            set vdom "root"
            set type physical
            set snmp-index 2
        next
        edit "port3"
            set vdom "root"
            set type physical
            set snmp-index 3
        next
        edit "port4"
            set vdom "root"
            set type physical
            set snmp-index 4
        next
        edit "ssl.root"
            set vdom "root"
            set type tunnel
            set alias "SSL VPN interface"
            set snmp-index 5
        next
        end

9. Para configurar uma Interface ***LAN*** e associar a ***VLAN 40 - TRANSITO***, execute o script abaixo:

>
	# criar uma nova Interface LAN
    #
    edit LAN
    #
    # Adicionar a nova Interface no VDOM root
    #
    set vdom root
    #
    # Habilitar acessos Administrativods e ICMP
    #
    set allowaccess ping https ssh http fgfm
    #
    # Associar com a Porta física Port2
    #
    set interface port2
    #
    # Alterar o tipo de Interface para VLAN
    #
    set type vlan
    #
    # Associar a VLAN 40
    #
    set vlanid 40
    #
    # Atribuir o IP 10.1.40.2/24 na Interface
    #
    set ip 10.1.40.2/24
    #
    # Aplicar a configuração e sai
    #
    end

10. Verificar o Status e a Conectividade da Interface LAN:

>
    # Obtém o status das interfaces do Sistema
    #
    get system interface

Compare com a saída abaixo:

>
    FG_A # execute ping 10.1.40.1
    == [ port1 ]
    name: port1   mode: dhcp    ip: 0.0.0.0 0.0.0.0   status: up    netbios-forward: disable    <br\>type: physical   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    mtu-override: disable    wccp: disable    drop-overlapped-fragment: disable    drop-fragment: disable    
    == [ port2 ]
    name: port2   mode: static    ip: 0.0.0.0 0.0.0.0   status: up    netbios-forward: disable    <br\>type: physical   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    mtu-override: disable    wccp: disable    drop-overlapped-fragment: disable    drop-fragment: disable    
    == [ port3 ]
    name: port3   mode: static    ip: 0.0.0.0 0.0.0.0   status: up    netbios-forward: disable    <br\>type: physical   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    mtu-override: disable    wccp: disable    drop-overlapped-fragment: disable    drop-fragment: disable    
    == [ port4 ]
    name: port4   mode: static    ip: 0.0.0.0 0.0.0.0   status: up    netbios-forward: disable    <br\>type: physical   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    mtu-override: disable    wccp: disable    drop-overlapped-fragment: disable    drop-fragment: disable    
    == [ ssl.root ]
    name: ssl.root   ip: 0.0.0.0 0.0.0.0   status: up    netbios-forward: disable    <br/>type: tunnel   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    wccp: disable    
    == [ LAN ]
    name: LAN   mode: static    ip: 10.1.40.2 255.255.255.0   status: up    netbios-forward: disable    <br\>type: vlan   netflow-sampler: disable    sflow-sampler: disable    scan-botnet-connections: disable    src-check: enable    mtu-override: disable    wccp: disable    drop-overlapped-fragment: disable    drop-fragment: disable

Verificar a conectividade com o Switch ***CORE_A***:

>
    # Executa um ping para 10.1.40.1
    #
    execute ping 10.1.40.1
    #

Compare com a saída abaixo:

>
    FG_A # execute ping 10.1.40.1
    PING 10.1.40.1 (10.1.40.1): 56 data bytes
    64 bytes from 10.1.40.1: icmp_seq=0 ttl=255 time=0.7 ms
    64 bytes from 10.1.40.1: icmp_seq=1 ttl=255 time=1.0 ms
    64 bytes from 10.1.40.1: icmp_seq=2 ttl=255 time=0.9 ms
    64 bytes from 10.1.40.1: icmp_seq=3 ttl=255 time=0.6 ms
    64 bytes from 10.1.40.1: icmp_seq=4 ttl=255 time=0.6 ms

    --- 10.1.40.1 ping statistics ---
    5 packets transmitted, 5 packets received, 0% packet loss
    round-trip min/avg/max = 0.6/0.7/1.0 ms


Verificar a conectividade com o Host ***CLIENTE_A***:

>
    # Executa um ping para 10.1.20.101
    #
    execute ping 10.1.20.101
    #

Compare com a saída abaixo:

>
    FG_A # execute ping 10.1.20.101
    PING 10.1.20.101 (10.1.20.101): 56 data bytes
    sendto failed
    sendto failed
    sendto failed
    sendto failed
    sendto failed

    --- 10.1.20.101 ping statistics ---
    5 packets transmitted, 0 packets received, 100% packet loss

11. Para configurar uma rota para as redes locais do ***SITE A***, execute o script abaixo:

>
	# Entrar no modo de configuração de Rotas Estáticas
    #
    config router static
    #
    # Exibir as Rotas Estáticas existentes
    #
    show

Compare com a saída abaixo:

>
    FG_A (static) # show
    config router static
    end

No momento não temos nenhuma Rota Estática criada no equipamento, vamos criar uma:

>
    # Criar uma entrada no Índice de Rotas 
    #
    edit 0
    #
    # Especificar a rede destino
    #
    set dst 10.1.0.0/16
    #
    # Especificar a Interface de Saída
    #
    set device LAN
    #
    # Especificar o Gateway da Rota
    #
    set gateway 10.1.40.1
    #
    # Aplica a configuração e sai
    #
    end

Verificar a tabela de rotas do Sistema:

>
    # Obtem e exibe a tabela de rotas global do equipamento
    #
    get router info routing-table all

Compare com a saída abaixo:

>
    FG_A # get router info routing-table all 

    Routing table for VRF=0
    Codes: K - kernel, C - connected, S - static, R     - RIP, B - BGP
        O - OSPF, IA - OSPF inter area
        N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
        E1 - OSPF external type 1, E2 - OSPF external type 2
        i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
        * - candidate default

    S       10.1.0.0/16 [10/0] via 10.1.40.1, LAN
    C       10.1.40.0/24 is directly connected, LAN

Verificar a conectividade com o Host ***CLIENTE_A***:

>
    # Executa um ping para 10.1.20.101
    #
    execute ping 10.1.20.101
    #

Compare com a saída abaixo:

>
    FG_A # execute ping 10.1.20.101
    PING 10.1.20.101 (10.1.20.101): 56 data bytes
    64 bytes from 10.1.20.101: icmp_seq=0 ttl=63 time=2.3 ms
    64 bytes from 10.1.20.101: icmp_seq=1 ttl=63 time=2.4 ms
    64 bytes from 10.1.20.101: icmp_seq=2 ttl=63 time=0.9 ms
    64 bytes from 10.1.20.101: icmp_seq=3 ttl=63 time=1.1 ms
    64 bytes from 10.1.20.101: icmp_seq=4 ttl=63 time=0.6 ms

    --- 10.1.20.101 ping statistics ---
    5 packets transmitted, 5 packets received, 0% packet loss
    round-trip min/avg/max = 0.6/1.4/2.4 ms

### Tarefa 02

O aluno deve acessar a Interface *WEB* do ***FG_A***, e completar a configuração para permitir o acesso dos Hosts do ***SITE A*** a *Internet*.

1. O aluno deve acessar o ***CLIENTE_A***.

![CLIENTE_A Tela de Login](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/CLIENTE_A-login.png)

2. Efetue login com as credênciais fornecidas no inicio do LAB

3. Abra o Navegador Firefox

![CLIENTE_A Localização do Firefox](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/CLIENTE_A-Firefox.png)

4. Digite o endereço 10.1.40.2 no Firefox para acessar a tela de Login do FortiGate FG_A, e efetue login com o usuário ***admin*** sem senha.

![CLIENTE_A FG_WEB Login](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/CLIENTE_A-FG_WEB_LOGIN.png)

5. Será exibida uma mensagem alertando que a senha dever alterada.

![FORTIGATE Password Warning](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/FG_PASSWORD_WARNING.png)

6. Altere o password para ***P@ssw0rd!***.

![FORTIGATE PASSWORD CHANGE](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/FG_PASSWORD_CHANGE.png)

7. Efetue login com o usuário ***admin*** e a senha ***P@ssw0rd!***.

![FORTIGAGE FAZER LOGIN COM NOVAS CREDENCIAIS](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/CLIENTE_A-FG_WEB_LOGIN_2.png)

