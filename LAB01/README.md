# FORTIGATE v6.0 Hands On
## LAB 01

### ***Índice***

* [Objetivo](#objetivo)
* [Topologia do LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#topologia-do-lab)
	* [Credenciais de acesso](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#credenciais-de-acesso)
* [Comandos utilizados no LAB](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#comandos-utilizados-no-lab)
* [ATIVIDADES DO LAB 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#atividades-do-lab-01)
	* [Tarefa 01](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-01)
	* [Tarefa 02](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-02)
	* [Tarefa 03](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-03)
	* [Tarefa 04](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-04)
	* [Tarefa 05](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-05)
	* [Tarefa 06](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-06)
	* [Tarefa 07](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-07)
	* [Tarefa 08](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-08)
	* [Tarefa 09](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#tarefa-09)

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
***Username*** = *pinetech*  
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

## ATIVIDADES DO LAB 01
### Tarefa 01
1. O aluno deve acessar o LAB 01.

2. Depois deve clicar em ***More actions***, e depois ***Start all nodes***

3. Aguardar os roteadores mudarem da cor cinza para azul.

4. ***Clicar no icone do RTA***.

![RTA](https://raw.githubusercontent.com/leandropinheiro/BGP101/master/img/RTA_console.png)

5. Deve abrir uma sessão de terminal com o *prompt* de *login* do roteador ***RTA***.

6. Efetue o login com as credenciais fornecidas acima.

7. Execute o script abaixo:

>
	! para entrar no modo de configuração, a partir do prompt #
	!
	configure terminal
	! muda para o prompt (config)#
	!
	! cria o processo BGP no ASN 100
	!
	router bgp 100
	!
	! adiciona o peer IPv4 1.1.1.2 no ASN 100 (iBGP)
	!
	neighbor 1.1.1.2 remote-as 100
	!
	! adiciona o peer IPv6 1::2 no ASN 100 (iBGP)
	!
	neighbor 1::2 remote-as 100
	!
	! sai do modo de configuração, retorna ao prompt #
	!
	end
	!
	! salva a alteração de configuração
	!
	write

8. Verificar neighbors IPv4 no ***RTA***, execute o comando ***show ip bgp summary***, compare com a saída de exemplo abaixo:

>
	RTA#show ip bgp summary
	BGP router identifier 1.1.1.1, local AS number 100
	BGP table version is 1, main routing table version 1
	
	Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
	1.1.1.2         4          100       0       0        1    0    0 never    Idle
	RTA#
	
	! veja que no Up/Down = never.
	! isso significa que a conexão BGP nunca ficou ativa antes.
	
	! Veja que o State/PfxRcd = Idle.
	! isso significa que a conexão com peer está Down.
