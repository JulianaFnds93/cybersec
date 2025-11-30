# cybersec
Desafio de Brute Force 
Programas e Ferramentas utilizadas: Kali Linux, Medusa e Metaexploitable 2.

1. Simulação Brute Force FTP
  a) Utilizando o comando Ping verificamos se a máquina vulnerável está responsiva:

      ping -c 3 192.168.15.43

   3 packets transmitted, 3 received, 0% packet loss, time 2040ms
      rtt min/avg/max/mdev - 0.023/0.029/0.042/0.009 ms

   Nenhum pacote enviado foi perdido o que demonstra que a máquina vulnerável está ativa para ser explorada.

  b) Enumeração utilizando Nmap
    Para identificarmos as vulnerabilidades FTP é necessário realizar a varredura de rede com Nmap:
    
    nmap -sV -p 21,22,80,445,138 192.168.15.43
    Com esse comando obtivemos o seguinte resultado:
    
<img width="863" height="272" alt="image" src="https://github.com/user-attachments/assets/8e9a35a4-125a-431e-b484-624ea04ef864" />

Conseguimos encontrar três portas vulneráveis

Porta     Estado      Serviço
21/tcp    Aberta      FTP
22/tcp    Aberta      SSH
80/tcp    Aberta      HTTP

Para validar se o serviço está mesmo ativo vamos testá-lo diretamente com o comando:

ftp 192.168.15.43

Onde obtivemos o seguinte retorno:
<img width="273" height="114" alt="image" src="https://github.com/user-attachments/assets/af5eb11c-4620-4153-afde-443070d3fe7c" />
