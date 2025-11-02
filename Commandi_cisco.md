Comandi Cisco CLI 
dispositivi Cisco (Router e Switch), organizzati per funzionalità, utilizzando il formato: [PROMPT] [COMANDO] // [DESCRIZIONE].


## ROUTER       

Router> enable // Entra nella modalità utente privilegiato.
Router# configure terminal // Entra nella modalità di configurazione globale.
Router(config)# interface <TIPO/N.> // Entra nella configurazione di una specifica interfaccia (es. GigabitEthernet0/0).
Router(config-if)# no shutdown // Attiva l'interfaccia.
Router(config-if)# ip address <IP> <SUBNET_MASK> // Assegna un indirizzo IP e una subnet mask all'interfaccia.
Router(config-if)# exit // Esci dalla modalità di configurazione dell'interfaccia.
Router# write // Salva la configurazione corrente (RAM) nella NVRAM (equivalente di copy run start).

## Comandi di Visualizzazione delle Informazioni (show)
Router# show running-config // Mostra la configurazione attiva del dispositivo.
Router# show ip route // Visualizza la tabella di routing completa.
Router# show ip route rip // Visualizza solo le rotte apprese tramite il protocollo RIP.
Router# show ip protocols // Mostra i protocolli di routing attivi.
Router# show ip dhcp pool // Visualizza le informazioni dettagliate sui pool DHCP configurati.
Router# show ip dhcp binding // Mostra gli indirizzi IP assegnati in quel momento ai client DHCP.Router# show ip rip database // Mostra il database RIP.


 ## ROUTING STATICO 
  NOTA BENE  CON QUESTO METODO  SE  LA TIPOLOGIA DELLA RETE CAMBIA  IL ROUTING NON FUNZIONA PIU !!!!!!!!!!! 
  Router(config)#	ip route <RETE_DEST> <SUBNET_MASK> <NEXT_HOP_IP>	// Configura una rotta statica verso la rete di destinazione, specificando l'indirizzo IP del router successivo (next hop).

#### Routing Dinamico 
1.  DHCP (Dynamic Host Configuration Protocol)
Router(config)# ip dhcp pool <NOME> // Crea un pool DHCP 
Router(dhcp-config)# network <IP_RETE> <SUBNET_MASK> // Definisce l'indirizzo di rete e la subnet mask per il pool.
Router(dhcp-config)# default-router <INDIRIZZO_IP> // Definisce l'indirizzo del gateway predefinito per i client.
Router(dhcp-config)# dns-server <INDIRIZZO_IP> // Specifica l'indirizzo del server DNS (es. 4.2.2.2).
Router(config-if)#ip helper-address{indirizzo-IP-server-DHCP}  // la configuro sull' interfaccia del  router che fa da gw   e si specifica L'indirizzo IP del server DHCP che si trova nella rete remota. il router Intercettare i messaggi di broadcast DHCP  ricevuti su quell'interfaccia. li Trasforma in messaggi unicast ( e gli indirizza a un host specifico)all'indirizzo IP specificato nel comando, che è l'indirizzo del server DHCP remoto.


2.  RIP (Routing Information Protocol)
Router(config)# router rip // Entra in modalità di configurazione del protocollo RIP.
Router(config-router)# network <IP_DI_RETE> // Annuncia l'indirizzo di rete  in accesso diretto 
Router(config-router)# version 2 // Imposta la versione 2 del protocollo RIP.
Router(config-router)# no auto-summary // Disabilita la somarizzazione automatica delle reti (utile per VLSM " subnetting variabile " ).

3.  OSPF (Open Shortest Path First)
Router(config)# router ospf 1 // Crea un processo OSPF con ID 1.
Router(config-router)# net <IP_RETE> <WildCard> area 0 // Annuncia la rete  nell'Area 0 .


4. Router-on-a-Stick  ( Virtualizzazione della interfaccia router dove passerano le  vlan )

Router(config)# int fa0/0.30 // Crea la sottointerfaccia per la VLAN 30.
Router(config-subif)# encapsulation dot1q 30 // Abilita il protocollo dot1q (tagging) per la VLAN 30.
Router(config-subif)# ip add 200.200.200.254 255.255.255.0 // Assegna l'IP del Gateway per la VLAN 30.
Router(config-subif)# exit // Esci dalla sottointerfaccia. 
outer(config)#int fa0/0
Router(config-if)#no sh
Router(config-if)#exit

Router(config)#int fa0/0.11
Router(config-subif)#encapsulation dot1q 11
Router(config-subif)#ip add 192.168.0.254 255.255.255.0
Router(config-subif)#exit

Router(config)#int fa0/0.22
Router(config-subif)#encapsulation dot1q 22
Router(config-subif)#ip add 175.10.255.254 255.255.0.0
Router(config-subif)#exit

Router(config)#int fa0/0.33
Router(config-subif)#encapsulation dot1q 33
Router(config-subif)#ip add 200.200.200.254 255.255.255.0
Router(config-subif)#exit  


5. NAT STATICO 
Router0
Router(config)#int fa0/0
Router(config-if)#ip nat inside
Router(config-if)#exit
Router(config)#int fa1/0
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#ip nat inside source static 192.168.1.2 200.0.0.50
Router(config)#ip nat inside source static 192.168.1.3 200.0.0.51
Router(config)#ip nat inside source static 192.168.1.4 200.0.0.52

6. NAT STATICO (con port forwarding)
Router(config)#int fa0/0
Router(config-if)#ip nat inside
Router(config-if)#exit
Router(config)#int fa1/0
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#ip nat inside source static tcp 10.0.0.2 80 210.0.0.50 6000


7. PAT
Config del Router0
Router(config)#int fa0/0
Router(config-if)#ip nat inside
Router(config-if)#exit
Router(config)#int fa1/0
Router(config-if)#ip nat outside
Router(config-if)#exit
Router(config)#access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)#ip nat pool 5CI 200.0.0.100 200.0.0.100 netmask 255.255.255.0
Router(config)#ip nat inside source list 10 pool 5CI overload




-----------------------
### SWITCH 

1. Configurazione Switch - VLAN e VTP

Switch(config)# vlan 10 // Crea la VLAN con numero 10.

Switch(config-vlan)# name gialla // Assegna il nome 'gialla' alla VLAN.

Switch(config)# interface FastEthernet0/1 // Entra nella configurazione della porta F0/1.

Switch(config-if)# switchport access vlan 10 // Assegna la porta F0/1 come porta di accesso alla VLAN 10.

Switch(config-if)# switchport mode trunk // Imposta la porta come trunk (trasporta più VLAN).

Switch(config-if)# switchport trunk allowed vlan add 30 // Aggiunge la VLAN 30 al trunk.

Switch(config-if)# switchport trunk allowed vlan remove 30 // Rimuove la VLAN 30 dal trunk.

2. VTP (VLAN Trunking Protocol)

Switch(config)# vtp mode server // Imposta lo switch come server VTP (può creare/modificare/cancellare VLAN).

Switch(config)# vtp mode client // Imposta lo switch come client VTP (sincronizza le VLAN ma non può modificarle).

Switch(config)# vtp domain 5ci // Imposta il nome del dominio VTP.

Switch(config)# vtp password test // Imposta la password di VTP.

Configurazione del server

Switch>en
Switch#conf t
Switch(config)#vtp mode server
Switch(config)#vtp domain 5ci
Switch(config)#vtp password test

Configurazione dei client
Switch>en
Switch#conf t
Switch(config)#vtp mode client
Switch(config)#vtp domain 5ci
Switch(config)#vtp password test


--------------------------------------------------------------

1. Concetti Fondamentali di Servizi di Rete (Server)
DNS (A Record) //Fornisce la traduzione di un nome in un indirizzo IP.

DNS (CNAME) //Associa un nome a un altro nome (alias).

DHCP //ssegna in modo automatico gli indirizzi IP ai dispositivi.

HTTPS //Protocollo HTTP sicuro, criptato (non visibile in chiaro con Wireshark).

FTP //Protocollo per caricare e scaricare file.

EMAIL (SMTP) //Protocollo per l'invio della posta elettronica (smpt.gmail.com).

EMAIL (POP3) //Protocollo per la ricezione della posta elettronica (pop3.gmail.com).




 