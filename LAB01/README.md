# FORTIGATE v6.0 Hands On
## LAB 01

### Índice

* [Objetivo](https://github.com/leandropinheiro/BGP101/tree/master/LAB%2001#objetivo)
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

### Objetivo
Neste lab o aluno será apresentado as configurações iniciais do ***FORTIGATE***, assim como aos comandos básicos para configurar a interface de gerencia via *CLI*.

O aluno deve seguir os passos abaixo para poder configurar os ***FG*** dos *Sites A* e *B*.

### Topologia do LAB

![topologia](https://raw.githubusercontent.com/leandropinheiro/FORTIGATE-HANDSON/master/Img/TOPOLOGIA%20-%20LAB%2001.png)

#### FORTIGATES
HOSTNAME | Port1 | Port2
:-------:|:----:|:---:
FG_A|**(WAN1)** *192.168.100.1/24*|**(LAN)** *10.1.40.2/24*<br/>**(DMZ)** *172.16.0.254/24*
FG_B|**(WAN1)** *192.168.200.1/24*|**(LAN)** *10.2.40.2/24*

#### ROTEADORES
HOSTNAME | Port1 | Port2

O roteador **RTC** está pré-configurado e o aluno não precisa realizar nenhuma intervenção de configuração neste equipamento, entretanto é permitido usar comandos *show*, *ping* e *tracerouter*.

Os roteadores **RTA** e **RTB** estão configurados apenas com as configurações básicas, como *hostname*, *description* e endereço *IPv4* e *IPv6* das *interfaces*. Nestes equipamentos o aluno pode fazer qualquer configuração, e executar quaisquer comandos.

#### Credenciais de acesso

***Username*** = *aluno*

***Password*** = *aluno*

### Comandos utilizados no LAB

Abaixo alguns dos comandos necessários para executar o LAB, com as devidas explicações:

COMANDO | DESCRIÇÃO
:-------|:---------
*no*|Nega/Desabilita/Inverte a função de um determinado comando
*do*|Executa um comando do modo *EXEC* a partir do modo de Configuração
*configure terminal*|Entra no modo de configuração global
*end*|Sai do mode de configuração global

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
