# Auditoria de Senhas com Kali Linux e Medusa

![Kali Linux](https://img.shields.io/badge/Kali_Linux-Security-blue?logo=kalilinux)
![Medusa](https://img.shields.io/badge/Tool-Medusa-red)
![VirtualBox](https://img.shields.io/badge/VirtualBox-Virtualization-blue?logo=virtualbox)
![Status](https://img.shields.io/badge/Status-Em%20desenvolvimento-yellow)

## Sobre o Projeto

Este projeto foi desenvolvido como parte de um desafio prático da **Digital Innovation One (DIO)**, com o objetivo de estudar técnicas de auditoria de senhas, compreender ataques de força bruta (*Brute Force*) e *Password Spraying*, e identificar medidas de proteção contra essas ameaças.

## Ambiente Utilizado

| Ferramenta | Finalidade |
|---|---|
| Kali Linux | Sistema operacional para auditoria de segurança |
| Medusa | Ferramenta para testes de autenticação |
| Metasploitable 2 | Máquina virtual intencionalmente vulnerável |
| DVWA | Aplicação web vulnerável para estudos |
| VirtualBox | Virtualização das máquinas |
| Nmap | Identificação de serviços e portas abertas |

## 🧪 Cenários de Teste

![nmap usado para Verificar o IP e as portas](imagens/kali&metasploit1)
![nmap](imagens/kali&matasploit2)

### 1. Ataque de força bruta em FTP

O objetivo é compreender como tentativas repetidas de autenticação podem comprometer um serviço FTP quando são utilizadas senhas fracas.

**Ferramentas:**
- Kali Linux
- Medusa
- Metasploitable 2

**Procedimento:**

1. Identificar o endereço IP da máquina de destino.
2. Verificar se o serviço FTP está ativo.
3. Criar uma pequena wordlist de usuários e senhas fictícias.
4. Executar tentativas controladas com o Medusa.
5. Analisar os resultados.
6. Registrar se alguma credencial de teste foi validada.

**Exemplo de identificação do serviço:**

```bash
nmap -sV -p 21 192.168.56.102
```

**Exemplo de execução do Medusa:**

```bash
medusa -h 192.168.56.102 -U user.txt -P passwords.txt -M ftp -t 1 -T 1
```

O comando utiliza duas listas pequenas, limitando a quantidade de tentativas simultâneas.

![resultado](imagens/kali&metasploit3)
![resultado](imagens/kali&metasploit4)

### 2. Testes de autenticação em formulário web (DVWA)

O objetivo é estudar como formulários de login podem ser vulneráveis a tentativas automatizadas de autenticação.

**Ferramentas:**
- DVWA
- Kali Linux
- Navegador ou ferramenta de automação HTTP compatível

**Procedimento:**

1. Acessar a aplicação DVWA.
2. Configurar o nível de segurança para Low, exclusivamente no laboratório.
3. Acessar o módulo Brute Force.
4. Preparar uma pequena lista de senhas fictícias.
5. Realizar tentativas controladas de autenticação.
6. Observar as respostas da aplicação.
7. Comparar o comportamento com níveis de segurança superiores.

![usando medusa e listas de user e passwords](imagens/kali&metasploit5)
![usando senha descoberta](imagens/kali&metasploit6)

### 3. Password Spraying em SMB

O Password Spraying consiste em testar uma senha comum contra diferentes contas, em vez de testar muitas senhas para uma única conta.

O objetivo é compreender os riscos de senhas fracas ou compartilhadas entre usuários.

**Ferramentas:**
- Kali Linux
- Medusa
- Metasploitable 2
- Nmap

**Procedimento:**

1. Identificar se o serviço SMB está ativo.
2. Realizar enumeração de usuários no ambiente autorizado.
3. Criar uma lista pequena de usuários fictícios ou contas de laboratório.
4. Selecionar uma única senha de teste.
5. Realizar tentativas controladas de autenticação.
6. Analisar os resultados.

**Identificação das portas SMB:**

```bash
nmap -sV -p 139,445 192.168.56.102
```

**Exemplo de execução do Password Spraying:**

```bash
medusa -h 192.168.56.102 -U users.txt -P smb_passwords.txt -M smbnt -t 1 -T 1
```

![ultilizando enum4linux](imagens/kali&metasploit7)
![descoberta a senha e usario](imagens/kali&metasploit8)



## 🛡️ Medidas de Mitigação

Durante os estudos, foram identificadas as seguintes medidas que ajudam a prevenir ataques de força bruta e Password Spraying:

| Vulnerabilidade | Medida de proteção |
|---|---|
| Senhas fracas | Exigir senhas fortes e exclusivas |
| Tentativas ilimitadas | Implementar limitação de tentativas |
| Password Spraying | Monitorar tentativas distribuídas entre contas |
| FTP sem criptografia | Substituir por SFTP ou FTPS |
| Ausência de monitoramento | Implementar logs e alertas |
| Contas sem proteção adicional | Utilizar autenticação multifator (MFA) |
| Enumeração de usuários | Reduzir exposição de informações de contas |

Outras recomendações:

- Desabilitar serviços desnecessários.
- Aplicar atualizações de segurança regularmente.
- Utilizar políticas de bloqueio e limitação de tentativas.
- Monitorar eventos de autenticação suspeitos.
- Evitar reutilização de senhas.

