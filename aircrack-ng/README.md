# Aircrack-ng
é uma suíte de ferramentas de auditoria de segurança para redes Wi-Fi. Ele é amplamente utilizado por profissionais de segurança e entusiastas para testar a segurança de redes sem fio, especificamente no que diz respeito a criptografia e autenticação. Aircrack-ng permite que você recupere chaves de redes WEP e WPA/WPA2-PSK após capturar pacotes de dados.

### Principais Componentes do Aircrack-ng

1. **airmon-ng**: Habilita e desabilita a monitoração do modo em interfaces de rede sem fio.
2. **airodump-ng**: Captura pacotes de dados em redes Wi-Fi, salvando-os em arquivos de captura (PCAP) para análise posterior.
3. **aireplay-ng**: Injeta pacotes em uma rede Wi-Fi, útil para realizar ataques de replay e ataques de desautenticação.
4. **aircrack-ng**: Realiza a quebra de chaves WEP e WPA/WPA2-PSK usando dicionários ou métodos de ataque de força bruta.

### Instalação do Aircrack-ng

No Linux, você pode instalar o Aircrack-ng usando o gerenciador de pacotes da sua distribuição. Por exemplo, no Ubuntu, use o seguinte comando:
```bash
sudo apt-get install aircrack-ng
```

### Fluxo Básico de Uso

1. **Habilitar Modo Monitor**:
   ```bash
   sudo airmon-ng start wlan0
   ```
   Isso habilita o modo monitor na interface de rede `wlan0`, permitindo a captura de pacotes.

2. **Capturar Pacotes**:
   ```bash
   sudo airodump-ng wlan0mon
   ```
   Isso inicia a captura de pacotes na interface `wlan0mon`, mostrando todas as redes Wi-Fi próximas.

3. **Focar em uma Rede Específica**:
   ```bash
   sudo airodump-ng --bssid [BSSID] --channel [canal] --write [arquivo] wlan0mon
   ```
   Substitua `[BSSID]` pelo endereço MAC da rede alvo, `[canal]` pelo canal da rede e `[arquivo]` pelo nome do arquivo onde os pacotes capturados serão salvos.

4. **Realizar Ataques de Desautenticação** (opcional, para forçar um cliente a se reconectar e capturar a handshake):
   ```bash
   sudo aireplay-ng --deauth [número de pacotes] -a [BSSID] -c [CLIENTE_MAC] wlan0mon
   ```
   Substitua `[número de pacotes]` pelo número de pacotes de desautenticação a serem enviados, `[BSSID]` pelo endereço MAC do ponto de acesso e `[CLIENTE_MAC]` pelo endereço MAC do cliente.

5. **Quebrar a Chave**:
   ```bash
   sudo aircrack-ng -w [wordlist] -b [BSSID] [arquivo].cap
   ```
   Substitua `[wordlist]` pelo arquivo de dicionário de senhas, `[BSSID]` pelo endereço MAC da rede e `[arquivo].cap` pelo arquivo de captura.

### Usos Comuns

1. **Quebra de WEP**: O WEP (Wired Equivalent Privacy) é um protocolo de segurança antigo e vulnerável. O Aircrack-ng pode ser usado para capturar pacotes e realizar ataques que exploram vulnerabilidades conhecidas do WEP.
2. **Quebra de WPA/WPA2**: WPA/WPA2 é mais seguro que WEP, mas também pode ser comprometido se a senha for fraca. O Aircrack-ng utiliza ataques de dicionário para tentar descobrir a chave PSK (Pre-Shared Key) usada na rede.
3. **Testes de Segurança**: Usar o Aircrack-ng em redes que você possui ou tem permissão para testar pode ajudar a identificar e corrigir vulnerabilidades antes que sejam exploradas por atacantes.

### Considerações Legais e Éticas

É importante lembrar que o uso do Aircrack-ng para acessar redes sem a permissão do proprietário é ilegal e antiético. Ele deve ser usado apenas em redes para as quais você tem autorização explícita para realizar testes de segurança.

### Recursos Adicionais

Para obter mais informações e tutoriais detalhados, você pode consultar a documentação oficial do Aircrack-ng e outros recursos online:

- [Site Oficial do Aircrack-ng](https://www.aircrack-ng.org/)
- [Guia de Uso do Aircrack-ng](https://www.aircrack-ng.org/doku.php?id=getting_started)
