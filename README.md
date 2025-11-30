# cybersec
Desafio de Brute Force 
Programas e Ferramentas utilizadas: Kali Linux, Medusa e Metaexploitable 2.

1. Simulação Brute Force FTP
  a) Utilizando o comando Ping verificamos se a máquina vulnerável está responsiva:

      ping -c 3 192.168.15.30

   3 packets transmitted, 3 received, 0% packet loss, time 2040ms
      rtt min/avg/max/mdev - 0.023/0.029/0.042/0.009 ms

   Nenhum pacote enviado foi perdido o que demonstra que a máquina vulnerável está ativa para ser explorada.

  b) Enumeração utilizando Nmap
    Para identificarmos as vulnerabilidades FTP é necessário realizar a varredura de rede com Nmap:
    
    nmap -sV -p 21,22,80,445,138 192.168.15.30
    Com esse comando obtivemos o seguinte resultado:
    
<img width="863" height="272" alt="image" src="https://github.com/user-attachments/assets/8e9a35a4-125a-431e-b484-624ea04ef864" />


