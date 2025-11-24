# 🏛️ Arquitetura Completa do Sistema SimplificaGov

## 📘 Visão Geral
O **SimplificaGov** é uma plataforma integrada que democratiza o acesso à informação legislativa no Brasil, oferecendo explicações em Linguagem Simples sobre Projetos de Lei (PLs), acompanhamento de parlamentares e interação direta via WhatsApp com o agente de IA **Simplinho**.  
A solução integra inteligência artificial, serviços de voz e automação para proporcionar uma experiência acessível, inclusiva e personalizada ao cidadão.

---

## 🧩 Arquitetura em Camadas

A arquitetura do SimplificaGov é composta por cinco blocos principais:

1. **Front-End Web (Portal SimplificaGov)**
2. **WhatsApp + Evolution API (Canal de Conversa)**
3. **Orquestração Inteligente (n8n)**
4. **Backend / API Central**
5. **Serviços de IA (Simplinho, STT e TTS via OpenAI)**

Cada componente é modular, escalável e independente.

---

# 🖥️ 1. Front-End Web (Portal SimplificaGov)

O Portal Web serve como a interface visual e interativa para o cidadão.

### ✨ Funcionalidades Principais
- **Mapa de Afetos:** O usuário define seus temas de interesse.  
- **Início de Conversa com o Simplinho:** Botão que abre uma conversa no WhatsApp.  
- **Acompanhamento de Projetos de Lei:** Consulta detalhada, histórico e explicação simples.  
- **Acompanhamento de Parlamentares:** Exibe proposições, histórico e atividades relevantes.  
- **Busca integrada com a API Central.**

### 🔗 Comunicação
```
Frontend → API Central (REST/JSON)
```

---

# 📱 2. Canal WhatsApp (via Evolution API)

O WhatsApp é o canal mais acessível para interação.

### 🔄 Fluxo
1. O usuário envia mensagem de texto ou áudio.  
2. Evolution API recebe e dispara um webhook para o n8n.  
3. O n8n processa a mensagem, aciona a IA e monta uma resposta.  
4. A resposta retorna para o WhatsApp via Evolution API.

---

# 🔄 3. Orquestração Inteligente – n8n

O **n8n** é o orquestrador da solução.

### 🧠 Responsabilidades
- Capturar mensagens do WhatsApp.
- Detectar tipo de mídia (texto/áudio).
- Transcrever áudio usando **OpenAI Whisper (STT)**.
- Extrair número do PL.
- Consultar perfil do cidadão.
- Chamar API Central para obter o PL.
- Acionar Simplinho (OpenAI ChatCompletion).
- Gerar áudio usando **OpenAI TTS**.
- Enviar resposta no canal correto.
- Salvar traduções simplificadas em cache.

---

# 🧠 4. Inteligência Artificial – Simplinho

**Simplinho** é o agente conversacional do SimplificaGov.

### 🎭 Características:
- Explica leis e PLs em **Linguagem Simples** seguindo a Lei nº 15.263.
- Apartidário e imparcial.
- Responde de forma amigável e clara.
- Oferece acessibilidade por texto e voz.

### 🧩 Modelos Utilizados
- **OpenAI ChatCompletion** – para explicações e resumos.  
- **OpenAI Whisper (STT)** – para transcrição de mensagens de áudio.  
- **OpenAI TTS ("nova")** – para geração de áudio claro e natural.  

---

# 🗄️ 5. Backend / API Central 

O Backend concentra lógica de negócios, integradores externos e armazenamento.

### 🎯 Objetivo
Fornecer acesso estruturado a dados legislativos, perfis de cidadãos, traduções em linguagem simples e materiais de comunicação.

---



## 📌 Estrutura do Projeto Backend
```
config      → configurações e variáveis
controllers → lógica de entrada
core        → roteador e núcleo
helpers     → utilidades
models      → regras e manipulação de dados
public      → ponto de entrada
routes      → definição das rotas REST
services    → integrações externas (Câmara, Senado, OpenAI)
```

---

# 🔌 Integrações Externas

A API consulta:

### 📚 Dados Legislativos
- **Câmara dos Deputados API**
- **Senado Federal API**

### 🤖 Inteligência Artificial
- **OpenAI GPT** (resumos, explicações, materiais de comunicação)
- **OpenAI Whisper** (STT)
- **OpenAI TTS** (áudio acessível)

---

# 🔊 Serviços de Voz (IA)

### 🎙️ Speech-to-Text (Transcrição)
Feito via **OpenAI Whisper**, permite que mensagens de áudio do cidadão sejam convertidas automaticamente para texto.

### 🔊 Text-to-Speech (Geração de Voz)
Feito via **OpenAI TTS**, permite gerar áudio acessível com linguagem simples, facilitando uso por pessoas com deficiência.

---

# 🧱 Fluxo Arquitetural Completo

```
[Cidadão]
   ↓ WhatsApp (texto/áudio)
[Evolution API]
   ↓ Webhook
[n8n Workflow]
   ├── STT (Whisper)
   ├── Extração PL
   ├── API Central
   ├── Simplinho (OpenAI)
   ├── Cache
   ├── TTS (quando necessário)
   ↓
[Evolution API]
   ↓
[WhatsApp do Cidadão]
```

---

# 🌐 Fluxo do Portal Web

```
[Usuário Web]
      ↓
[Front-End SimplificaGov]
      ↓ REST API
[API Central]
      ├── Projetos de Lei
      ├── Parlamentares
      ├── Mapa de Afetos
      └── Perfil do Cidadão
```

---

# 🛡️ Segurança

- Uso mínimo de dados (telefone do cidadão como identificação).
- Nenhum dado sensível armazenado.
- Retorno sempre em JSON padronizado.
- Logs anonimizáveis.
- IA integrada com limites éticos (sem opinião política).

---

# 🚀 Escalabilidade e Expansão

A solução é preparada para expansão futura:

- Notificações proativas de PL.
- Dashboard de parlamentares.
- API pública para pesquisa.
- Canal Telegram.
- Aplicativo móvel.
- Mapa de Afetos expandido.
- Suporte multilíngue.

---

# 🎉 Conclusão

O SimplificaGov unifica IA, acessibilidade, transparência e participação cidadã.  
Combinando WhatsApp, API legislativa, backend robusto e o agente Simplinho, o sistema traduz a política para uma linguagem clara, humana e acessível — aproximando o Congresso da população brasileira.
