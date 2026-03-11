# TransPro 1.0 beta
### Sistema Profissional de Gravação e Transcrição de Áudio

> Transcrição offline com IA · Múltiplas categorias · Dicionários personalizados · Exportação profissional

---

## Índice

1. [Visão Geral](#visão-geral)
2. [Requisitos](#requisitos)
3. [Instalação](#instalação)
4. [Interface](#interface)
5. [Funcionalidades](#funcionalidades)
6. [Login e Controle de Acesso](#login-e-controle-de-acesso)
7. [Fluxo Pós-Transcrição](#fluxo-pós-transcrição)
8. [Arquivar Transcrição](#arquivar-transcrição-)
9. [Extração de Áudio de Vídeo](#extração-de-áudio-de-vídeo)
10. [Atalhos de Teclado](#atalhos-de-teclado)
12. [Categorias](#categorias)
13. [Dicionários Personalizados](#dicionários-personalizados)
14. [Transcrição em Tempo Real](#transcrição-em-tempo-real)
15. [Identificação de Falantes](#identificação-de-falantes)
16. [Timestamps](#timestamps)
17. [Modo Lote](#modo-lote)
18. [Resumo com IA](#resumo-com-ia)
19. [Exportação](#exportação)
20. [Qualidade de Áudio](#qualidade-de-áudio)
21. [Configurações](#configurações)
22. [Estrutura de Pastas](#estrutura-de-pastas)
23. [Perguntas Frequentes](#perguntas-frequentes)
24. [Solução de Problemas](#solução-de-problemas)

---

## Visão Geral

O **TransPro** é um sistema desktop para gravação e transcrição de áudio que roda **100% offline** usando o modelo Whisper da OpenAI. Desenvolvido para uso profissional em múltiplos nichos: saúde, direito, jornalismo, pesquisa, educação e negócios.

**Destaques:**
- Transcrição offline — nenhum dado enviado para a internet
- 8 categorias com templates e vocabulários específicos
- Dicionários de correção por categoria com edição inline
- Identificação automática de falantes por pausa
- Timestamps em cada segmento transcrito
- Modo lote: transcreve uma pasta inteira em fila
- Extração de áudio de vídeo (MP4, MKV, AVI, MOV, WEBM…) com um clique
- Exportação em 6 formatos: TXT, DOCX, PDF, SRT, DICOM SR, HL7
- **☁ Arquivar** com nome padronizado direto no Google Drive / OneDrive / Dropbox
- Banner pós-transcrição que guia o próximo passo no fluxo
- Resumo automático com IA (requer conexão e chave API)
- Interface com tema claro e escuro (Tokyo Night)
- Sistema de login com perfis Admin e Usuário
- Highlight de palavras com baixa confiança (sublinhado laranja)
- Agenda de gravações (programa gravações em horários específicos)
- Verificador de atualizações automático em background
- Gerador de executável `.exe` via PyInstaller (`gerar_exe.bat`)
- Relatório de conclusão do Modo Lote
- Silêncio para auto-stop configurável pela interface
- Sincronização de dicionários via pasta de rede/nuvem
- Persistência do SRT entre sessões
- Painel de administração com gerenciamento de usuários e log de acessos

---

## Requisitos

| Item | Mínimo |
|------|--------|
| Sistema Operacional | Windows 10 / 11 |
| Python | 3.10 ou superior |
| RAM | 4 GB (8 GB recomendado) |
| Armazenamento | 2 GB livres |
| Microfone | Qualquer dispositivo de entrada de áudio |
| ffmpeg | Necessário para arquivos MP3, M4A, AAC |
| GPU NVIDIA | Opcional — acelera 5–10x (requer CUDA) |

---

## Instalação

### Passo 1 — Instale o Python

Baixe em [python.org/downloads](https://www.python.org/downloads).
Durante a instalação marque obrigatoriamente: **"Add Python to PATH"**

### Passo 2 — Coloque os arquivos juntos

Extraia todos os arquivos mantendo a estrutura de pastas:

```
TransPro/
  ├── main.py
  ├── setup.bat
  ├── iniciar.bat
  ├── core/
  ├── ui/
  ├── exportadores/
  └── dados/
```

### Passo 3 — Execute o instalador

Clique com o botão direito em `setup.bat` → **Executar como administrador**

O instalador irá:
- Verificar Python e ffmpeg
- Criar ambiente virtual isolado (venv)
- Instalar todas as dependências automaticamente
- Criar o atalho `TransPro.bat` na Área de Trabalho

### Passo 4 — Uso diário

Dê dois cliques em `iniciar.bat` ou no atalho `TransPro.bat` da Área de Trabalho.

Na **primeira execução**, o modelo Whisper será baixado automaticamente (~500MB para o modelo `small`).

---

## Interface

```
┌─────────────────────────────────────────────────────────┐
│  ◈ TRANSPRO  1.0 beta   [Modelo] [Tempo real] [Auto-parar]  │  ← Header
├────────────┬────────────────────────────────────────────┤
│ CATEGORIA  │  Sessão: ______  Participantes: _______    │  ← Meta
│ ─────────  ├────────────────────────────────────────────┤
│ 📝 Livre   │                                            │
│ 🤝 Reunião │   Área de texto principal                  │  ← Editor
│ 🎤 Entrev. │   [00:01] [FALANTE A] texto transcrito...  │
│ ⚖️ Juríd.  │                                            │
│ 🏥 Médico  ├────────────────────────────────────────────┤
│ ─────────  │  ⏺ GRAVAR  📄 TRANSCREVER  🤖 RESUMO      │  ← Transport
│ HISTÓRICO  ├────────────────────────────────────────────┤
│ busca...   │  ~~~waveform animado~~~                    │  ← Waveform
│ lista...   ├────────────────────────────────────────────┤
│            │  💾TXT  📝DOCX  📋PDF  🎬SRT  🏥DICOM  ⚕HL7│  ← Exportar
│ 📂 Lote    ├────────────────────────────────────────────┤
│ 📖 Dicion. │  Status: Pronto. F2 gravar · F4 transcrever│  ← Status
└────────────┴────────────────────────────────────────────┘
```

---

## Funcionalidades

### Gravação de Áudio

- Captura direta do microfone com taxa de 16kHz (padrão Whisper)
- Waveform animado em tempo real mostrando o nível do áudio
- Timer de duração da gravação
- Auto-parar por silêncio: para automaticamente após 3 segundos sem fala
- Pipeline de melhoria automática antes de salvar:
  - Filtro passa-alta 80Hz (remove ruído de ventilação e AC)
  - Pré-ênfase 0.97 (realça frequências da fala)
  - Normalização de amplitude

### Transcrição

- Motor: OpenAI Whisper (totalmente offline)
- Idioma: Português brasileiro
- Modelos disponíveis: `tiny`, `base`, `small`, `medium`, `large`
- Filtros anti-alucinação automáticos
- Aplicação do dicionário de correções após cada transcrição
- Suporte a arquivos externos: WAV, MP3, M4A, OGG, FLAC, AAC
- Extração de áudio de vídeo: MP4, MKV, AVI, MOV, WEBM, FLV, WMV (via ffmpeg)
- Formatos aceitos exibidos em texto pequeno permanente na interface

### Histórico

- Armazena as últimas 80 transcrições automaticamente
- Busca full-text: encontre qualquer palavra em qualquer transcrição anterior
- Clique em qualquer item para recarregar o texto e os metadados
- Ícone e categoria preservados por sessão

---

## Atalhos de Teclado

| Tecla | Ação |
|-------|------|
| `F2` | Iniciar / Parar gravação |
| `F3` | Parar gravação |
| `F4` | Transcrever áudio |
| `F5` | Gerar resumo com IA |
| `Ctrl+S` | Salvar como TXT |
| `Ctrl+N` | Nova sessão |
| `Botão direito` no texto | Menu: adicionar palavra ao dicionário |
| `☁ Arquivar` (barra exportação) | Salvar com nome padronizado na pasta escolhida |

---

## Categorias

Cada categoria tem seu próprio template, vocabulário de transcrição e prompt de resumo.

| Ícone | Categoria | Uso típico |
|-------|-----------|------------|
| 📝 | Livre | Uso geral sem template |
| 🤝 | Reunião | Atas, deliberações, encaminhamentos |
| 🎤 | Entrevista | Jornalismo, pesquisa, RH |
| ⚖️ | Jurídico | Audiências, depoimentos, pareceres |
| 🏥 | Médico | Laudos, anamneses, evoluções |
| 🌿 | Pesquisa de Campo | Etnografia, linguagem regional, informal |
| 🎙️ | Podcast | Conversas, múltiplos falantes |
| 📚 | Aula | Aulas, palestras, treinamentos |

**Como funciona:** ao selecionar uma categoria, o sistema:
1. Carrega o template estrutural no editor (se o texto estiver vazio)
2. Ativa o vocabulário específico que guia o Whisper
3. Aplica o dicionário de correções daquela categoria
4. Usa o prompt de resumo adequado quando gerar com IA

Para **adicionar uma nova categoria**, edite o arquivo `dados/categorias.py` seguindo o padrão existente — não é necessário alterar nenhum outro arquivo.

---

## Dicionários Personalizados

Cada categoria possui seu próprio dicionário de correções independente.

### Como usar

**Adicionar uma palavra pelo editor visual:**
1. Clique em **📖 Dicionário** na sidebar
2. Preencha "Como foi transcrito" e "Como deve ficar"
3. Clique em **＋ Adicionar**
4. Clique em **💾 Salvar e Fechar**

**Adicionar uma palavra inline (mais rápido):**
1. Selecione a palavra errada no texto transcrito com o mouse
2. Clique com o botão direito → **📖 Adicionar ao dicionário**
3. O campo "Errado" já vem preenchido — preencha o correto
4. Salve

**Adicionar pelo botão da barra de exportação:**
1. Selecione a palavra no texto
2. Clique no botão laranja **📖＋ Palavra**
3. Preencha e salve

### Como funciona

- As correções são aplicadas automaticamente após cada transcrição
- A correspondência é case-insensitive
- A versão capitalizada também é corrigida automaticamente
- Exemplo: `"fubica" → "república"` também corrige `"Fubica" → "República"`
- Os dicionários ficam salvos em `Documentos/TransPro/dicionarios.json`

### Dica para linguagem regional

Para falantes com dicção baixa ou linguagem regional, o fluxo ideal é:
1. Transcrever normalmente
2. Ler o texto e identificar os erros recorrentes
3. Adicionar cada erro ao dicionário da categoria
4. Nas próximas transcrições do mesmo falante, os erros são corrigidos automaticamente

---

## Transcrição em Tempo Real

Ative o checkbox **"Tempo real"** no header antes de iniciar a gravação.

- O texto aparece durante a gravação em intervalos de ~8 segundos
- Cada trecho novo aparece destacado em azul por 3 segundos
- Funciona apenas se o modelo já estiver carregado
- Requer CPU razoável — use o modelo `small` para melhor desempenho

**Nota:** o delay inicial de ~10–15 segundos é normal na CPU, pois inclui o tempo de acúmulo do chunk mais o processamento do Whisper.

---

## Identificação de Falantes

Ative o checkbox **"Id. falantes"** no header.

- Detecta troca de falante quando há uma pausa de mais de 1,2 segundos
- Insere `[FALANTE A]` e `[FALANTE B]` coloridos no texto
- Falante A aparece em laranja, Falante B em verde
- Não identifica quem é quem — apenas que houve troca
- Útil para entrevistas, reuniões e podcasts com dois participantes principais

---

## Timestamps

Ativos por padrão em todas as transcrições.

- Cada segmento recebe o timestamp do início no formato `[MM:SS]`
- Aparece em azul no início de cada linha
- Exportado normalmente em TXT e DOCX
- Usado para gerar o arquivo SRT automaticamente

---

## Modo Lote

Transcreve automaticamente todos os arquivos de áudio de uma pasta.

**Como usar:**
1. Clique em **📂 Em lote** na sidebar
2. Clique em **Selecionar** e escolha a pasta com os áudios
3. A lista de arquivos aparece com status "Aguardando"
4. Clique em **▶ Iniciar Transcrição**
5. Acompanhe o progresso na barra e na coluna de status
6. Os arquivos transcritos são salvos em `Documentos/TransPro/` com o mesmo nome do áudio original

**Formatos suportados:** WAV, MP3, M4A, OGG, FLAC, AAC, WMA

---

## Resumo com IA

Gera automaticamente um resumo após a transcrição usando a API da Anthropic (Claude).

**Configuração:**
1. Acesse **⚙ Configurações** no header
2. Cole sua chave de API Anthropic no campo indicado
3. Clique em **💾 Salvar**

**Como usar:**
1. Transcreva um áudio normalmente
2. Pressione **F5** ou clique em **🤖 RESUMO**
3. O resumo aparece ao final do texto com fundo diferenciado

**Prompts por categoria:**
- Reunião: lista decisões, ações, responsáveis e prazos
- Entrevista: principais pontos e declarações relevantes
- Jurídico: partes, objeto, argumentos e decisões
- Médico: indicação, achados e conclusão diagnóstica
- Demais: resumo objetivo dos pontos principais

**Observação:** o resumo requer conexão com a internet. A gravação e transcrição continuam 100% offline.

---

## Exportação

| Formato | Extensão | Uso |
|---------|----------|-----|
| TXT | `.txt` | Texto puro, compatível com qualquer editor |
| DOCX | `.docx` | Word com formatação profissional e cabeçalho |
| PDF | `.pdf` | Documento finalizado com layout visual |
| SRT | `.srt` | Legendas para vídeos (requer transcrição com timestamps) |
| DICOM SR | `.dcm` | Integração com sistemas PACS (uso médico) |
| HL7 | `.hl7` | Integração com sistemas hospitalares |

**Dica SRT:** o arquivo SRT é gerado a partir dos segmentos da última transcrição. Se fechar o programa e reabrir, o SRT não estará disponível — exporte antes de fechar.

---

## Fluxo Pós-Transcrição

Após cada transcrição concluída, o sistema exibe um **banner verde** na interface:

```
✓ Transcrição salva no histórico.  Revise, exporte ou arquive antes de iniciar nova sessão.
```

O banner orienta o usuário sobre o próximo passo e some automaticamente quando qualquer exportação ou arquivamento é realizado.

Se o usuário tentar iniciar nova sessão sem ter arquivado ou exportado, o sistema exibe um aviso:

```
Esta transcrição ainda não foi arquivada ou exportada.
Sim → Descartar mesmo assim    Não → Voltar e arquivar
```

---

## Arquivar Transcrição (☁)

O botão **☁ Arquivar** fica na barra de exportação, ao lado dos botões de formato.
Permite salvar a transcrição com **nome padronizado** diretamente em qualquer pasta local —
incluindo pastas sincronizadas do Google Drive, OneDrive ou Dropbox.

### Padrão de nomeação automático

```
2025-03-07_Médico_Laudo-Ecocardiograma_Dr-Silva.docx
│           │       │                   │
data        categoria  sessão            participante
```

Todos os campos vêm preenchidos automaticamente com os dados da sessão. O nome é editável antes de salvar.

### Como usar

1. Revise o texto transcrito
2. Clique em **☁ Arquivar** na barra de exportação
3. Confirme ou edite o nome do arquivo
4. Selecione o formato: DOCX · PDF · TXT · SRT
5. Selecione a pasta destino (ou clique no atalho detectado automaticamente)
6. Clique em **☁ Arquivar**

### Detecção automática de nuvem

O sistema detecta automaticamente as pastas do Google Drive, OneDrive e Dropbox instalados no computador e exibe atalhos de um clique na janela de arquivamento.

**Importante:** o TransPro salva na pasta local. A sincronização com a nuvem é feita pelo aplicativo do Google Drive / OneDrive / Dropbox que já está instalado no PC — o TransPro não se comunica diretamente com a internet.

### Memória por categoria

A pasta de destino é lembrada separadamente por categoria. Transcrições médicas sempre vão para `/Laudos/`, jurídicas para `/Processos/`, etc. — sem precisar navegar toda vez.

---

## Extração de Áudio de Vídeo

O botão **🎬 Vídeo** na barra de transporte permite abrir diretamente um arquivo de vídeo.
O sistema extrai o áudio automaticamente usando ffmpeg e transcreve normalmente.

**Formatos de vídeo suportados:** MP4 · MKV · AVI · MOV · WEBM · FLV · WMV

**Pré-requisito:** ffmpeg instalado (o `setup.bat` instala automaticamente no Windows via winget; no macOS via Homebrew; no Linux via apt/dnf/pacman).

**Como usar:**
1. Clique em **🎬 Vídeo** na barra de transporte
2. Selecione o arquivo de vídeo
3. Aguarde a extração (aparece mensagem "⟳ Extraindo áudio…")
4. Pressione F4 para transcrever normalmente

---

## Login e Controle de Acesso

O TransPro possui sistema de autenticação com dois perfis de usuário.

### Primeiro acesso

Na primeira execução é criado automaticamente o usuário administrador padrão:

```
Usuário: admin
Senha:   admin123
```

O sistema exige troca de senha obrigatória no primeiro login.

### Perfis

| Perfil | Acesso |
|--------|--------|
| **Admin** | Tudo + Painel de administração + Gerenciar usuários + Ver log |
| **Usuário** | Gravação, transcrição, exportação, arquivamento |

### Painel de Administração (somente Admin)

Acessado pelo botão **Admin** no header. Contém duas abas:

**Aba Usuários:**
- Criar novos usuários com nome, login, senha e perfil
- Ativar ou desativar usuários sem excluí-los
- Redefinir senha de qualquer usuário (força troca no próximo login)
- Excluir usuários permanentemente
- Proteção: não permite desativar ou excluir o último administrador

**Aba Log de Acesso:**
- Histórico dos últimos 500 registros
- Registra: login, logout, criação de usuário, redefinição de senha, exclusão

### Alterar própria senha

Qualquer usuário pode alterar sua própria senha pelo botão **🔑** no header.

### Segurança

- Senhas armazenadas como SHA-256 com salt individual por usuário
- Nunca armazenadas em texto puro
- Arquivo de usuários: `Documentos/TransPro/usuarios.json`
- Log de acessos: `Documentos/TransPro/log_acesso.json`

---


## Highlight de Baixa Confiança

Durante a transcrição o Whisper calcula internamente a probabilidade de cada palavra.
O TransPro usa esse dado para **sublinhar em laranja** as palavras com baixa confiança —
sinalizando trechos que merecem revisão antes de exportar.

A barra de status informa quantas palavras foram marcadas:
```
✓ Transcrição concluída  ·  ⚠ 3 palavra(s) incerta(s) sublinhada(s)
```

Palavras sublinhadas são candidatas a correção no dicionário da categoria.

---

## Agenda de Gravações

O botão **⏰** na barra de transporte abre a janela de agendamento.
Permite programar gravações automáticas em datas e horários específicos.

**Como usar:**
1. Clique em **⏰** na barra de transporte
2. Informe data (DD/MM/AAAA), hora (HH:MM), nome da sessão e duração máxima
3. Clique em **⏰ Agendar**
4. O sistema monitora em background e inicia a gravação no horário

A gravação para automaticamente ao atingir a duração máxima ou pelo auto-stop de silêncio.
Agendamentos são persistidos em `Documentos/TransPro/agenda.json`.

---

## Atualização Automática

O sistema verifica atualizações disponíveis 3 segundos após a inicialização, em background.
Nunca atualiza sem confirmação do usuário — apenas exibe uma notificação com a versão nova e o link de download.

Para configurar a URL de verificação (útil em redes internas):
```
Configurações → URL de atualização
```

---

## Gerador de Executável (.exe)

Para distribuir o TransPro sem precisar que o destinatário instale Python:

1. Execute o `setup.bat` normalmente
2. Execute `gerar_exe.bat` como Administrador
3. Aguarde 3–5 minutos (empacota Python + Whisper + PyTorch)
4. O executável estará em `dist\TransPro.exe`

O arquivo gerado tem entre 2 e 4 GB (inclui o modelo Whisper e o PyTorch).
O destinatário só precisa do `.exe` — sem instalar nada.

Para usuários avançados: use o `transpro.spec` para controle fino do build.

---


## Qualidade de Transcrição em Áudio Difícil

O TransPro tem um conjunto de proteções que melhoram a qualidade em gravações com ruído, volume baixo ou fala rápida.

### Mecanismos de proteção ativos

**Filtro de caracteres não-latinos** — segmentos com caracteres cirílicos, japoneses, coreanos ou árabes são automaticamente descartados e substituídos por `[trecho inaudível]`. Esse tipo de caractere é a marca clássica do Whisper em colapso.

**Filtro de n-gramas repetidos** — detecta quando o modelo entra em loop (ex: "bumbum de ação bumbum de ação…") e descarta o segmento. Requer 3 ou mais repetições de um bigrama ou trigrama.

**Marcador de gap** — quando o Whisper descarta mais de 8 segundos de áudio, o sistema insere `[trecho inaudível]` com o timestamp do trecho perdido, em vez de simplesmente pular sem aviso.

**Fallback de temperatura** — a transcrição começa com `temperature=0` (máxima consistência). Se o resultado tiver mais de 30% de segmentos ruins, o sistema reprocessa automaticamente com temperature `(0.2, 0.4, 0.6)`, que torna o modelo mais exploratório.

**`no_speech_threshold=0.3`** (padrão) — menos agressivo que o padrão do Whisper (0.6), reduzindo o descarte de trechos com fala baixa ou ruidosa.

**`condition_on_previous_text=True`** — o modelo usa o contexto anterior para manter coerência no vocabulário ao longo da gravação.

### Configurações avançadas (⚙ → Avançado)

Todos os parâmetros são configuráveis pela interface. O botão **↺ Restaurar padrões** recupera os valores ideais com um clique.

### O que o script não resolve

Áudio com sobreposição de vozes, volume muito baixo ou ruído de fundo intenso supera a capacidade do modelo `small`. Nesses casos, considere gravar em ambiente mais silencioso ou usar o modelo `medium` para arquivos críticos.

---

## Verificador de Atualizações

### TransPro
Verifica automaticamente 3 segundos após abrir. Também disponível em **⚙ Configurações → Atualizações → Verificar agora**.

### Whisper
O motor de transcrição é atualizado regularmente pela OpenAI. O TransPro compara a versão instalada com a mais recente no PyPI e informa se há atualização disponível.

**Segurança:** o verificador apenas lê metadados do PyPI. Nunca baixa nem instala nada automaticamente. A atualização do Whisper, quando desejada, é feita manualmente:
```
venv\Scripts\activate
pip install openai-whisper --upgrade
```

Para redes internas sem acesso ao PyPI, configure a URL de verificação em **⚙ Configurações → Atualizações**.

---

## Configurações Avançadas

Acessadas em **⚙ → aba Avançado**. Destinadas a usuários técnicos.

| Parâmetro | Padrão | Efeito |
|-----------|--------|--------|
| `temperature` | 0 | 0 = conservador, 1 = criativo. Aumentar ajuda em áudio ruim, pode reduzir precisão em áudio bom |
| `no_speech_threshold` | 0.3 | Menor = menos trechos descartados. Aumentar reduz falsos positivos de fala |
| `beam_size` | 3 | Maior = mais preciso e mais lento |
| `best_of` | 3 | Maior = mais preciso e mais lento |
| `logprob_threshold` | -1.0 | Mais negativo = aceita resultados menos prováveis |
| `compression_ratio_threshold` | 2.4 | Maior = tolera mais repetição antes de considerar alucinação |

O botão **↺ Restaurar padrões** retorna todos os valores para o estado original.

---


## Qualidade de Áudio

### Melhores práticas para transcrição precisa

**Microfone:**
- Use microfone de lapela a 10–15 cm da boca
- Evite microfones embutidos de laptop — captam muito ruído ambiente
- Ajuste o ganho no Windows: Painel de Som → Dispositivos de Gravação → Propriedades → Níveis

**Ambiente:**
- Grave em ambiente silencioso
- Ar condicionado e ventiladores prejudicam a transcrição
- Feche janelas se houver barulho externo

**Postura:**
- Fale em velocidade normal — nem muito rápido, nem muito devagar
- Articule bem, especialmente termos técnicos
- Faça pausas naturais entre frases

**Modelo:**
- `small`: mais rápido, boa precisão — recomendado para CPU
- `medium`: mais preciso, ~2–3 min por minuto de áudio na CPU
- `large`: máxima precisão, lento sem GPU

### Pipeline automático

O sistema aplica automaticamente antes de cada transcrição:
1. **Filtro passa-alta 80Hz** — remove ruído de baixa frequência
2. **Pré-ênfase 0.97** — realça a faixa da voz humana
3. **Normalização** — maximiza o volume sem distorcer

---

## Configurações

Acesse pelo botão **⚙** no canto superior direito.

| Configuração | Descrição |
|-------------|-----------|
| Modelo Whisper | Define o modelo de IA (tiny/base/small/medium/large) |
| Tempo real | Transcreve durante a gravação em chunks |
| Auto-parar | Para gravação automaticamente após 3s de silêncio |
| Id. falantes | Identifica e diferencia falantes no texto |
| Chave API | Chave Anthropic para o resumo automático com IA |
| Tema | Alterna entre escuro (Tokyo Night) e claro |
| Pasta Google Drive/OneDrive/Dropbox | Detectada automaticamente; usada pelo ☁ Arquivar |

As configurações são salvas automaticamente em `Documentos/TransPro/config.json`.

---

## Estrutura de Pastas

```
TransPro/                         ← Pasta do programa
  ├── main.py                     ← Ponto de entrada
  ├── setup.bat                   ← Instalador
  ├── iniciar.bat                 ← Atalho de inicialização
  ├── core/
  │   ├── auth.py                        ← Autenticação, perfis, log
  │   ├── atualizador.py                 ← Verificador de versão
  │   ├── gravador.py             ← Captura de áudio
  │   ├── transcritor.py          ← Motor Whisper + timestamps + lote
  │   └── processador.py          ← Melhoria de áudio
  ├── ui/
  │   ├── app.py                  ← Interface principal
  │   ├── tema.py                 ← Sistema de temas
  │   ├── dialogo_dicionario.py   ← Editor de dicionários
  │   └── dialogo_lote.py         ← Interface de lote
  ├── exportadores/
  │   └── exportar.py             ← TXT, DOCX, PDF, SRT, DICOM, HL7
  ├── dados/
  │   └── categorias.py
  ├── gerar_exe.bat                      ← Gerador de .exe (Windows)
  └── transpro.spec                      ← Configuração PyInstaller           ← Templates e prompts por categoria
  └── venv/                       ← Ambiente virtual (criado pelo setup)

Documentos/TransPro/              ← Dados do usuário (gerados em uso)
  ├── historico.json              ← Histórico de transcrições
  ├── dicionarios.json            ← Dicionários por categoria
  ├── config.json                 ← Configurações salvas
  ├── rec_YYYYMMDD_HHMMSS.wav     ← Gravações de áudio
  └── _tmp/                       ← Arquivos temporários (tempo real)
```

---

## Perguntas Frequentes

**O sistema precisa de internet?**
Não. Gravação, transcrição, dicionários, exportação — tudo offline. Apenas o resumo com IA usa internet, e é opcional.

**Posso usar em mais de um computador?**
Sim, mas o `venv` não é portátil. Execute o `setup.bat` em cada máquina. Os dados do usuário (`Documentos/TransPro/`) podem ser copiados entre máquinas.

**O modelo Whisper vai baixar toda vez?**
Não. O download acontece apenas na primeira execução de cada modelo. Após isso fica em cache local.

**Por que aparece "Legendas pela comunidade de Amara.org"?**
É uma alucinação conhecida do Whisper que ocorre quando o áudio tem muito silêncio ou qualidade muito baixa. O sistema filtra esse texto automaticamente e exibe um aviso.

**Posso transcrever áudio em outros idiomas?**
O sistema está configurado para português. Para outros idiomas, edite `language="pt"` nas chamadas de transcrição em `core/transcritor.py`.

**Qual a diferença entre os modelos?**

| Modelo | Velocidade (CPU) | Precisão |
|--------|-----------------|---------|
| tiny   | ~15s/min        | Básica  |
| small  | ~45s/min        | Boa ✓   |
| medium | ~3min/min       | Muito boa |
| large  | ~6min/min       | Excelente |

---

**Esqueci a senha do admin**
Execute o sistema com o usuário `admin` e peça a outro administrador para redefinir via Painel Admin. Se for o único admin, apague o arquivo `Documentos/TransPro/usuarios.json` — o sistema recria o admin padrão na próxima execução.

## Solução de Problemas

**"No module named 'whisper'"**
Execute no CMD com o venv ativado:
```
pip install openai-whisper --no-cache-dir
```

**"'python' não é reconhecido como comando"**
O Python não está no PATH. Reinstale o Python marcando "Add Python to PATH".

**Áudio gravando mas sem transcrição**
- Verifique se o modelo terminou de carregar (aparece "✓ modelo pronto" no header)
- Teste com um áudio curto e claro
- Verifique o nível do microfone no Windows

**Waveform não se move durante gravação**
O microfone selecionado no Windows pode estar errado. Acesse Painel de Som → Dispositivos de Gravação e defina o microfone correto como padrão.

**Transcrição muito lenta**
Use o modelo `small` em vez de `medium`. Se tiver GPU NVIDIA, instale o PyTorch com CUDA para acelerar 5–10x.

**Auto-parar disparando muito cedo**
Ajuste o valor `SILENCIO_SEG = 3.0` em `core/gravador.py` para um número maior (ex: `5.0`).

---

## Dependências

| Pacote | Versão | Uso |
|--------|--------|-----|
| openai-whisper | latest | Motor de transcrição |
| sounddevice | 0.5+ | Captura de microfone |
| scipy | 1.10+ | Processamento de áudio |
| numpy | 1.24+ | Operações numéricas |
| python-docx | 1.0+ | Exportação Word |
| reportlab | 4.0+ | Exportação PDF |
| pydicom | 3.0+ | Exportação DICOM SR |
| tkinter | built-in | Interface gráfica |

---

## Licença e Privacidade

O TransPro é um sistema local. Nenhum dado de áudio, texto ou configuração é enviado a servidores externos, exceto quando o resumo com IA está ativo e uma chave API está configurada — nesse caso, apenas o texto transcrito (não o áudio) é enviado à API da Anthropic.

---

*TransPro 1.0 beta · Desenvolvido com OpenAI Whisper · Interface Python/Tkinter*
#   t r a n s p r o  
 