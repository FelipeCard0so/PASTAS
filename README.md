# 📡 PASTAS DT — Gerador de Estrutura de Pastas para DriveTest SSV

Ferramenta desktop desenvolvida em Python para automatizar a criação da estrutura de pastas de campanhas de DriveTest (SSV) em redes móveis 2G, 3G, 4G e 5G.

---

## 🧠 O que o programa faz

Lê o arquivo `RELATORIO_SSV_COMPLETO.txt` gerado pelo NGSolution e, a partir das atividades listadas, cria automaticamente:

- As **pastas principais** de cada site/frequência com nomenclatura padronizada
- As **subpastas internas** organizadas por tecnologia (ROTA, STATICO, NSA, SA, S1/S2/S3, DL/UL/VOZ/EPSFB)
- Os **ícones personalizados** de cada pasta conforme a tecnologia e frequência
- Um **relatório final em janela pop-up** informando o que foi criado e o que já existia

---

## 📁 Estrutura de pastas gerada

### 4G (700, 1800, 2100, 2600, 2300A, 2300B)
```
INSTALADORA_SSV_SITE_4G_FREQ_CIDADE_UF
├── ROTA
└── STATICO
    ├── S1
    │   ├── DL
    │   ├── UL
    │   └── VOZ
    ├── S2
    │   ├── DL
    │   ├── UL
    │   └── VOZ
    └── S3
        ├── DL
        ├── UL
        └── VOZ
```

### 5G (3500, 2300)
```
INSTALADORA_SSV_SITE_5G_FREQ_CIDADE_UF
├── ROTA
│   ├── NSA
│   └── SA
└── STATICO
    ├── NSA
    │   ├── S1 → DL / UL
    │   ├── S2 → DL / UL
    │   └── S3 → DL / UL
    └── SA
        ├── S1 → DL / UL / EPSFB
        ├── S2 → DL / UL / EPSFB
        └── S3 → DL / UL / EPSFB
```

### 3G (850, 2100)
```
INSTALADORA_SSV_SITE_3G_FREQ_CIDADE_UF
├── ROTA - A
└── ROTA - B
```

### 2G (900, 1800, 1900)
```
INSTALADORA_SSV_SITE_2G_FREQ_CIDADE_UF
└── ROTA
```

---

## 🗂️ Nomenclatura das pastas

O nome de cada pasta segue o padrão:

```
INSTALADORA_SSV_SITE_TECNOLOGIA_FREQUÊNCIA_CIDADE_UF
```

**Exemplos:**
```
EOLEN_SSV_MG-UVS_4G_700_PARACATU_MG
EOLEN_SSV_MG-UVS_4G_1800_PARACATU_MG
EOLEN_SSV_MG-UVS_5G_3500_PARACATU_MG
TELEQUIPE_SSV_SP-CPJ_3G_850_BRAGANCA_PAULISTA_SP
STEIN_SSV_MG-FOY_4G_2600RS_FORMIGA_MG
```

**Instaladoras suportadas:** `TELEQUIPE`, `EOLEN`, `STEIN`

---

## 🎨 Ícones por tecnologia

| Tecnologia | Frequência | Ícone usado |
|---|---|---|
| 4G | 700 | `700.ico` |
| 4G | 1800 | `1800.ico` |
| 4G | 2100 | `2100.ico` |
| 4G | 2600 / 2600RS / 2600* | `2600.ico` |
| 4G | 2300A | `2300A.ico` |
| 4G | 2300B | `2300B.ico` |
| 5G | qualquer (3500, 2300) | `3500.ico` |
| 3G | qualquer (850, 2100) | `3G.ico` |
| 2G | qualquer (900, 1800, 1900) | `2G.ico` |

---

## ⚙️ Configuração

Abra o arquivo `PASTAS_DT.py` e ajuste os caminhos no topo do script:

```python
RELATORIO_PATH = r"C:\Users\User\Desktop\PYTHON\DT 3.0\OUT\RELATORIO_SSV_COMPLETO.txt"
STANDBY_PATH   = r"C:\Nemo Tools\Results\0 STANDBY"
ICON_PATH      = r"C:\Users\User\Desktop\PYTHON\DT 3.0\ICON"
```

| Variável | Descrição |
|---|---|
| `RELATORIO_PATH` | Caminho completo do relatório `.txt` gerado pelo NGSolution |
| `STANDBY_PATH` | Pasta de destino onde as pastas serão criadas |
| `ICON_PATH` | Pasta contendo os arquivos `.ico` |

---

## 🚀 Como usar

### Rodando pelo Python (PyCharm ou terminal)

```bash
python PASTAS_DT.py
```

### Gerando o executável `.exe`

```bash
pyinstaller --onefile --noconsole PASTAS_DT.py
```

O `.exe` gerado ficará em `dist/PASTAS_DT.exe` e pode ser distribuído para outros computadores **sem necessidade de Python instalado**.

---

## 📋 Requisitos

- Python 3.10+
- Biblioteca padrão apenas (`os`, `re`, `ctypes`, `tkinter`) — nenhuma instalação adicional necessária
- Sistema operacional: **Windows** (a aplicação de ícones via `desktop.ini` é exclusiva do Windows)

---

## 🔍 Lógica do parser

O programa é robusto contra variações no formato do relatório:

1. **Metadados do site** (instaladora, site, cidade) → extraídos da seção `Nome - LOGS`
2. **UF do estado** → extraída do log se presente (ex: `_PARACATU_MG_`); caso ausente (ex: `_PARACATU__`), buscada automaticamente no cabeçalho do bloco `(CIDADE - MG)`
3. **Tecnologias e frequências** → todas expandidas a partir da linha `4G:700/1800/2100/2600` do bloco, garantindo que nenhuma frequência seja omitida
4. **Verificação de duplicatas** → antes de criar, consulta o diretório `STANDBY` e ignora pastas já existentes

Isso garante que o programa funcione mesmo quando o relatório vier com formatação incompleta.

---

## 📊 Relatório final

Ao término, uma janela exibe o resumo da execução:

```
✔ Pastas criadas:  12
✦ Já existiam:     5

─── Criadas: ──────────────────────────────────────
  EOLEN_SSV_MG-UVS_4G_700_PARACATU_MG
  EOLEN_SSV_MG-UVS_4G_1800_PARACATU_MG
  ...

─── Já existiam (ignoradas): ──────────────────────
  EOLEN_SSV_MG-CVW_4G_700_CURVELO_MG
  ...
```

---

## 📂 Estrutura do projeto

```
DT 3.0/
├── PASTAS_DT.py          # Script principal
├── OUT/
│   └── RELATORIO_SSV_COMPLETO.txt   # Relatório de entrada (gerado pelo NGSolution)
└── ICON/
    ├── 700.ico
    ├── 1800.ico
    ├── 2100.ico
    ├── 2600.ico
    ├── 2300A.ico
    ├── 2300B.ico
    ├── 3500.ico
    ├── 3G.ico
    └── 2G.ico
```

---

## 👨‍💻 Autor

Desenvolvido para uso interno em campanhas de DriveTest SSV em redes móveis 2G/3G/4G/5G.
