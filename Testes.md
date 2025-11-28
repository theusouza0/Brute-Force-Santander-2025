# Configurando Rede das Máquinas
Configurar rede entre as duas VMs (opcional, mas útil)

Para que as máquinas “se enxerguem”:

1. Vá em Configurações → Rede da VM.
2. Altere o modo de rede para “Rede Interna” ou “Host-Only”.
3. Faça isso nas duas máquinas.

Assim elas poderão se comunicar sem afetar sua rede real.

Endereços de rede de cada máquina:

### Metasploitable 2
<img width="726" height="396" alt="Captura de tela 2025-11-27 231806" src="https://github.com/user-attachments/assets/5f69d798-a3d3-4148-8e83-14104b8672ae" />

### Kali Linux
<img width="680" height="330" alt="image" src="https://github.com/user-attachments/assets/4d1748e2-eb5c-41f7-8707-5fc86a64dcf8" />

Todas as máquinas estão com a placa de rede definida em **HOST-ONLY**

## Confirmando o Funcionamento da rede HOST-ONLY

Pingue uma máquina para outra:

<img width="616" height="221" alt="image" src="https://github.com/user-attachments/assets/4dbfbafa-460a-4c3e-8ab7-f6cd6cafb4a0" />

<img width="587" height="178" alt="image" src="https://github.com/user-attachments/assets/5c71250d-96aa-4ed5-8ed7-4546ae1ecf57" />


## Primeiros passos:
Execute o comando:
nmap -sV -p 21,22,80,445,139 *ip de destino*

Exemplo:
<img width="1214" height="286" alt="image" src="https://github.com/user-attachments/assets/fe87aea5-9d62-488b-b09a-d9ea11faecda" />

Assim irá mostrar todas as portas que solicitamos. Todas estão abertas.


## Criando wordlists de usuários e senhas:

Basta criar arquivos txt e definir as chaves linha por linha.

<img width="650" height="514" alt="image" src="https://github.com/user-attachments/assets/303466f3-bfc0-4a24-ab97-45f102856327" />

<img width="1385" height="480" alt="image" src="https://github.com/user-attachments/assets/74755393-1282-4c57-9196-6c1c28085f49" />


É possível fazer por CLI(Command-Line Interface) também.

*echo -e "user\msfadmin\nadmin\nroot" > users.txt*

*echo -e "123456\npassword\nqwerty\msfadmin" > pass.txt*

## Medusa
*medusa -h 192.168.56.103 -U ./users.txt -P ./pass.txt -M ftp -t 6*

medusa = Comando
Parâmetros: 
- -h : destino
- -U : Arquivo de usuário
- -P : Arquivo de senhas
- -M : Protocolo
- -t : Trheads

<img width="1287" height="323" alt="image" src="https://github.com/user-attachments/assets/fd41af35-f67c-4044-b838-bdbcbb2dfff0" />

Agora basta validar manualmente:

<img width="333" height="195" alt="image" src="https://github.com/user-attachments/assets/64605d8c-adbd-422a-9cfe-76e354771a22" />

## Ataque com HTTP

1. No Kali Linux, abra o Firefox e acesse o endereço número.ip/dvwa/login.php para visualizar a página de login do DVWA. Essa etapa serve apenas para observar como funciona a comunicação entre o navegador e o servidor durante um processo de autenticação.

<img width="1909" height="900" alt="image" src="https://github.com/user-attachments/assets/7ae6c687-05df-4b3c-8f74-c3073e43b31f" />

2. O Medusa irá atingir esses parâmetros:
<img width="508" height="707" alt="image" src="https://github.com/user-attachments/assets/6bd9481a-3cf6-4ab6-b2b6-695341953e98" />

*medusa -h 192.168.56.103 -U users.txt -P pass.txt -M http \ -m PAGE:'/dvwa/login.php' \ -m FORM: 'username=^USER^&password=^PASS^&Login=Login' \ -m 'FAIL=Login failed' -t 6*
-m [TEXTO]: Permite enviar um ou mais parâmetros para o módulo em uso.
Você pode repetir essa opção várias vezes, cada uma com um valor diferente. Todos os valores informados serão repassados ao módulo.

<img width="1514" height="807" alt="image" src="https://github.com/user-attachments/assets/db4a1e3f-eb7d-47f1-aede-b6f75d3793d8" />

Após descobrir a combinação, basta acessar na WEB.

## Ataque com SMB (Server Message Block)
enum4linux é uma ferramenta utilizada para coletar informações de servidores Windows/Samba a partir de sistemas Linux.
Enumeração é a fase de um processo de análise ou auditoria em que coletamos informações detalhadas sobre um sistema, serviço, rede ou aplicação

*enum4linux -a 192.168.56.103 | tee enum4_output.txt* 
- -a: destino

comando tee:
Copia a entrada padrão para cada ARQUIVO, e também para a saída padrão.

<img width="916" height="772" alt="image" src="https://github.com/user-attachments/assets/28251a86-8cb3-49c2-82eb-c32102943a33" />

<img width="956" height="617" alt="image" src="https://github.com/user-attachments/assets/d3666ceb-1e03-4350-88ad-c02929a85ae7" />

<img width="877" height="499" alt="image" src="https://github.com/user-attachments/assets/fabc8b89-722e-47f8-b483-aa7e5779f9af" />

Agora basta criar dois arquivos de texto com a wordlist:

<img width="1388" height="537" alt="image" src="https://github.com/user-attachments/assets/e82128a8-24c0-4d69-8418-4e67c9724a07" />

*echo -e "user\nmsfadmin\nservice" > smb_users.txt*
*echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray*

Diferente do brute force, que tenta várias senhas em um único usuário, o password spraying faz o oposto: testa poucas senhas em muitos usuários. Isso reduz a chance de bloqueio de contas e explora o fato de que muitos usuários ainda utilizam senhas fracas ou previsíveis.

<img width="1378" height="297" alt="image" src="https://github.com/user-attachments/assets/4bd9dba4-2926-4fe4-92b3-37365edbfe3d" />

Para reduzir o risco de ataques de força bruta — inclusive aqueles realizados com ferramentas como o Medusa no Kali Linux — é essencial implementar uma estratégia de segurança em múltiplas camadas. Isso inclui políticas rigorosas de criação e renovação de senhas, controles de acesso reforçados, mecanismos de proteção na camada de rede e soluções de monitoramento capazes de identificar e bloquear tentativas suspeitas de autenticação de forma proativa.

## Requisitos Mínimos
- VirtualBox instalado

- 8GB RAM disponível (4GB por VM)

- 60GB espaço em disco

- Host com Windows/Linux/macOS


Downloads
_______________________________________

| Software | Versão | Fonte |
| -------- | ----- | ----------- |
| Kali Linux       | 2024.x     | https://www.kali.org/get-kali/     |
| Metasploitable       | 2     | https://sourceforge.net/projects/metasploitable/       |
|DVWA|Latest| https://github.com/digininja/DVWA |
|VirtualBox	| 7.0+ |	https://www.virtualbox.org/|
