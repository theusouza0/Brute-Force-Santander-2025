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
