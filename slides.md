---
theme: seriph
layout: cover
class: text-center
background: /images/PLACEHOLDER-capa.png
title: Arranhando a Superfície de Ataque
info: |
  ## Arranhando a Superfície de Ataque
  Escopos desconhecidos e os novos riscos impulsionados pela IA.

  Talk técnica para desenvolvedores.
transition: slide-left
colorSchema: dark
duration: 40min
---

# Arranhando a Superfície de Ataque

## Escopos desconhecidos e os novos riscos impulsionados pela IA

<div class="abs-br m-6 text-sm opacity-60">
  Seu Nome · Sua Empresa · 2026
</div>

<!-- IMG: capa — visual dark de "superfície" (mapa de IPs/portas, teia de conexões). O background aponta para /images/PLACEHOLDER-capa.png -->

<!--
[Abertura]
- Quem sou eu e o que faço.
- Promessa: não é pentest 101. É sobre o que o seu dia a dia cria sem você perceber — e como a IA acelerou isso.
- Aviso: nada aqui autoriza atacar terceiros. Reconhecimento só no SEU escopo.
-->

---

# O que é superfície de ataque

Tudo que está exposto e pode ser alcançado por quem não é você.

- Domínios, subdomínios, portas e serviços
- APIs, rotas internas e endpoints esquecidos
- Dependências e imagens de container
- Pessoas com acesso (credenciais, sessões, e-mail)

<div class="mt-8 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Diagrama: empresa no centro, setas para DNS, API, bucket, CI/CD e funcionários
</div>

<!-- IMG: diagrama simples "empresa" no centro com setas de exposição para DNS, API, S3, CI/CD e pessoas -->

<!--
- Superfície = soma de tudo alcançável, não só o servidor.
- Inclui o bucket, o preview deploy, a rota de staging e o dev.
- Esta talk não é sobre reduzir superfície em geral — é sobre o que você NÃO vê.
-->

---

# Por que isso é problema de dev

Segurança deixou de ser só o time de AppSec.

- Quem publica a rota é o dev — e quem esquece de removê-la também
- `npm install`, `docker pull`, IaC: a decisão acontece no PR
- A IA escreve código e sugere pacotes que você confia sem ler
- LGPD/ANPD: o incidente é responsabilidade da empresa, não do "CDN"

<!-- IMG: captura de um PR revisando dependência/rota, ilustrando a decisão de segurança no fluxo de dev -->

<!--
- Você não precisa ser pentester. Precisa saber o que o seu código deixou no ar.
- LGPD: a ANPD pune a organização; o vetor costuma ser um detalhe no PR.
- Transição: a superfície começa nas pessoas.
-->

---
layout: section
---

# Você faz parte da superfície

<!--
- Nenhum firewall segura alguém que clica.
- Dois vetores humanos: aliciamento de insiders e phishing.
-->

---

# Compra e aliciamento de funcionários

Às vezes o ataque não é técnico: é uma proposta no chat.

- Lapsus$: comprava credenciais e acesso via canais abertos
- Tesla (2020): tentativa de suborno (~US$1M) a funcionário para plantar malware
- Acesso vendido como serviço em fóruns e canais de mensagem
- Insider com acesso legítimo não dispara alarme

<div class="mt-8 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Print censurado de canal anunciando venda de acesso / manchete da tentativa de suborno na Tesla
</div>

<!-- IMG: captura (censurada) de canal dark/Telegram vendendo acesso, ou manchete do caso Tesla 2020 -->

<!--
- O Lapsus$ não "hackeava" tudo: pagava, ligava para o helpdesk e coagia.
- Tesla 2020: convite para instalar ransomware na rede; virou caso público via FBI — o alvo é o funcionário, não a máquina.
- Detecção: comportamento legítimo é difícil de separar. MFA resistente a phishing + menor privilégio reduzem.
-->

---

# Phishing não é um ataque só

- **Spear phishing** — e-mail direcionado com contexto real do alvo
- **BEC / CEO fraud** — "sou eu, faz o pagamento, é urgente"
- **Vishing** — ligação fingindo ser suporte/banco/chefe
- **Smishing / quishing** — SMS e QR code levando ao fluxo falso
- Objetivo comum: credencial, MFA ou convencer alguém a agir

<div class="mt-8 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Colagem de 3 canais (e-mail, ligação, QR/SMS) convergindo para o mesmo objetivo: credencial
</div>

<!-- IMG: colagem de 3 canais (e-mail, ligação, QR/SMS) apontando para o mesmo objetivo: roubar credencial -->

<!--
- BEC não tem malware: engana a pessoa.
- Quishing cresceu porque o QR esconde a URL real.
- Transição: o que a IA fez com isso? Multiplicou por escala e por realismo.
-->

---
layout: section
---

# Novos riscos impulsionados por IA

<!--
- Parte central da talk.
- Não é "IA é o futuro": são mecanismos concretos de ataque usando IA.
- Três: phishing/vishing clonado, shadow AI e slopsquatting.
-->

---

# Phishing e vishing turbinados por IA

- Voice/video cloning a partir de poucos segundos de áudio/vídeo público
- Arup (2024): deepfake do CFO em videochamada → ~US$25M
- Spear phishing em escala: OSINT + LLM personaliza idioma, cargo e contexto
- Callback phishing: e-mail leve + ligação clonada para "confirmar"

<div class="mt-8 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Diagrama do ataque: áudio público → clone de voz → ligação/videochamada enganando o financeiro
</div>

<!-- IMG: diagrama do ataque — amostra de áudio pública (palestra/podcast) → clone de voz → ligação ou videochamada enganando o financeiro -->

<!--
- Clonagem de voz: ~3s de amostra já bastam em TTS moderno; a fonte é palestra, podcast ou saudação de voicemail.
- Arup 2024: o funcionário viu e ouviu "o CFO" numa videochamada (deepfake de rosto); a empresa perdeu cerca de US$25M. O sinal que quebrou foi o pedido de mudar o número de callback.
- Escala: o LLM lê LinkedIn, commits públicos e o site da empresa e escreve no tom certo, sem os erros que denunciavam o phishing antigo.
- Defesa: verificação fora de banda (canal/número conhecido), MFA resistente a phishing, desconfiar de urgência + mudança de dados bancários.
-->

---

# Shadow AI

- Dev colando código proprietário, segredo ou dado de cliente em IA pública
- Conta pessoal/free ≠ conta corporativa com controle de dados
- Entrada pode ser retida, revisada ou usada como contexto/treino
- Extensão de IDE envia contexto do repo inteiro, não só a linha

<div class="mt-8 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Print de prompt público com trecho de código/.env colado (censurado), com o aviso de retenção do plano free destacado
</div>

<!-- IMG: captura de prompt de IA público com trecho de código/.env colado (censurado), destacando o aviso de retenção do plano free -->

<!--
- Samsung 2023: engenheiros colaram código-fonte interno no ChatGPT; a empresa restringiu o uso.
- Mecanismo: o prompt sai do seu perímetro. Dependendo do plano/vendor, fica retido, pode ser revisado e pode virar contexto de outro usuário ou de treino.
- O pior caso é o inadvertido: pedir "explica esse erro" e colar o stack trace com token, string de conexão ou PII.
- Defesa: IA aprovada com no-training, controle de egress/DLP, secret scanning no pre-commit, nunca colar credencial.
-->

---

# Slopsquatting

LLMs alucinam nomes de pacotes — e alguém registra esse nome.

- Estudo "We Have a Package for You!" (2024): ~1 em 5 nomes gerados não existe
- A mesma alucinação se repete entre prompts e modelos → previsível
- Atacante registra o nome no npm/PyPI com `postinstall` malicioso
- Dev (ou outra IA) roda `npm install` no pacote fantasma

```bash
npm install some-ai-suggested-package   # não existe de verdade
```

<!-- IMG: captura de chat de IA sugerindo um pacote inexistente + o mesmo nome já publicado no npm por terceiro -->

<!--
- Slopsquatting = typosquatting + alucinação de LLM.
- Mecanismo (Spracklen et al., 2024): geraram milhares de recomendações de pacote; ~19% eram inexistentes e recorrentes. Dá para minerar essas listas.
- Payload típico: `postinstall` roda `curl | bash`, lê variáveis de ambiente, pega `.npmrc`/tokens e exfiltra.
- Defesa: revisar pacote novo antes de instalar, lockfile + auditoria, registry interno/proxy, versão fixada e nunca instalar pacote sugerido sem conferir se existe e quem mantém.
-->

---
layout: section
---

# Dependências

<!--
- Seu código é a menor parte do que você roda.
- Duas frentes: bibliotecas e infra/containers.
-->

---

# Bibliotecas

- CVE não corrigida — inclusive transitiva, que você nem declarou
- Typosquatting e dependency confusion (nome interno registrado no público)
- Pacote comprometido: maintainer invadido, `postinstall` malicioso
- Exemplos: event-stream, xz-utils (CVE-2024-3094), colors/faker

```bash
npm ls --all | wc -l      # quantas deps você realmente tem?
npm audit --production
```

<!-- IMG: grafo de dependências transitivas destacando uma CVE profunda que não estava declarada no package.json -->

<!--
- `npm install` executa código de terceiros (scripts). Instalação = execução.
- Dependency confusion: pacote privado "empresa-utils" sem escopo; o atacante publica o mesmo nome no registry público com versão maior.
- xz-utils: backdoor introduzido por um maintainer "de confiança" após meses construindo reputação. Confiar na pessoa ≠ confiar no código.
- Defesa: lockfile, SCA no CI, registry proxy, versão fixada e revisão de scripts de install.
-->

---

# Infra e containers

- Imagem-base desatualizada / EOL acumulando CVE
- Segredos em camadas: `ENV`, `ARG` e arquivos "deletados" continuam na história
- K8s/IaC mal configurado: dashboard exposto, RBAC permissivo, S3 público
- Segredo em `values.yaml` / `tfstate` commitado

```bash
docker history --no-trunc minha-imagem:latest
docker save minha-imagem | grep -iE 'password|token|key'
```

<!-- IMG: saída de `docker history` mostrando um segredo em claro num ARG/ENV, ou print do `dive` com camada suspeita -->

<!--
- Camada é imutável: mesmo apagando o .env no passo seguinte do Dockerfile, ele vive na camada anterior.
- `docker history` mostra o comando de build e, às vezes, o segredo em claro no ARG.
- K8s: `kubectl get all --all-namespaces` + Services LoadBalancer esquecidos são clássicos.
- Defesa: base mínima/distroless, multi-stage, secret manager em runtime, scan de IaC (tfsec/checkov) e nunca segredo no git.
-->

---
layout: section
---

# Reconhecimento

<!--
- O que dá para descobrir de forma passiva, sem tocar no alvo de forma agressiva.
- Quase tudo aqui é público ou passivo.
-->

---
layout: image-right
image: /images/PLACEHOLDER-crt-sh.png
---

# Subdomínios via Certificate Transparency

Todo certificado TLS emitido vira registro público.

- SANs expõem `dev-`, `staging-`, `homolog-`, `jenkins-`
- `crt.sh` é consultável via API — sem tocar no alvo

```bash
curl -s "https://crt.sh/?q=%25.alvo.com&output=json" \
  | jq -r '.[].name_value' | sort -u
```

<!-- IMG: screenshot do crt.sh mostrando a lista de subdomínios de um domínio de exemplo (lado direito da imagem-right) -->

<!--
- CT é obrigatório para as CAs; por isso o log é público e gratuito.
- Muitas vezes o subdomínio de staging aparece no CT antes de qualquer link.
- Defesa: wildcard cert ajuda na aparência, mas o registro é append-only — melhor não nomear ambiente interno de forma previsível e não deixar staging exposto.
-->

---

# Automatizando a coleta

Do subdomínio bruto ao que responde de verdade.

```bash
subfinder -d alvo.com -silent \
  | httpx -silent -mc 200,301,302 -o live.txt
gowitness scan file -f live.txt
```

- `subfinder` — enumera subdomínios de fontes públicas
- `httpx` — filtra quais estão vivos e quais respondem 200
- `gowitness` — captura screenshot para triagem rápida

<div class="mt-6 p-4 border-2 border-dashed border-white/40 rounded text-center opacity-70">
  [placeholder] Pipeline: domínio → subfinder → httpx → gowitness → lista com screenshots
</div>

<!-- IMG: diagrama do pipeline subfinder → httpx → gowitness (entrada: domínio; saída: lista de hosts vivos com screenshots) -->

<!--
- Esse pipeline roda em minutos; a triagem visual do gowitness acha painel esquecido numa olhada.
- Só faça isso em escopo autorizado. Coleta passiva de dados públicos é diferente de scan agressivo.
- Complementos: Amass para enumeração mais profunda e dnsx para resolução.
-->

---
layout: two-cols-header
layoutClass: gap-6
---

# Motores de busca: o que fica indexado sem querer

Serviços e dispositivos são varridos e catalogados na internet — banner, certificado e título de página.

::left::

## Shodan

```txt
ssl:"alvo.com.br" http.status:200
http.title:"Grafana" country:"BR"
"MongoDB Server Information" port:27017
```

::right::

## FOFA

```txt
domain="alvo.com.br" && port="8080"
title="Kibana" && country="BR"
cert="alvo.com.br"
```

<!-- IMG: prints/ícones de Shodan e FOFA lado a lado (opcional — manter os dois blocos de código como destaque) -->

<!--
- Shodan indexa banner de serviço; FOFA indexa HTML/ícone/cert — útil para fingerprint e para achar painel.
- O erro comum: banco ou dashboard sem auth só "escondido" atrás de porta alta. O scanner acha.
- Defesa: não confie em obscuridade; use auth + rede privada e monitore exposição (ASM).
- Layout `two-cols-header`: título no topo e colunas lado a lado.
-->

---
layout: image-right
image: /images/PLACEHOLDER-wayback-antes-depois.png
---

# Wayback Machine: o endpoint que "não existe mais"

Snapshots históricos de páginas, JS e respostas de API ficam arquivados.

- Rota removida do front-end ≠ rota desligada no back-end
- Endpoint antigo ainda autentica, ainda vaza dado
- Cruze o passado com o presente:

```bash
gau alvo.com | httpx -silent -mc 200
```

<!-- IMG: "antes/depois" — snapshot antigo no Wayback (esquerda) vs. resposta 200 do MESMO endpoint hoje (direita) -->

<!--
- O dev limpa a rota do SPA ou do menu; o backend continua servindo. O Wayback guarda a URL antiga.
- `gau`/`waybackurls` extraem tudo que já foi visto: JS antigo revela endpoints de API e parâmetros.
- A validação é o pulo do gato: `gau alvo.com | httpx -mc 200` mostra quais daqueles endpoints antigos ainda respondem AGORA.
- É o melhor exemplo de "você acha que removeu". Fecha o arco da talk.
-->

---
layout: section
---

# Fechamento

<!--
- Como agir: reduzir, detectar e responder.
-->

---

# Reduzir · Detectar · Responder

<div class="grid grid-cols-3 gap-4 mt-8">

<div class="p-4 border border-white/20 rounded">

### Reduzir

- Inventário de ativos e subdomínios
- Remover a rota no back-end, não só no front
- Lockfile, base mínima, zero segredo no git

</div>

<div class="p-4 border border-white/20 rounded">

### Detectar

- CT logs e ASM para novos subdomínios
- SCA + scan de imagem/IaC no CI
- Alerta de segredo exposto e de egress anômalo

</div>

<div class="p-4 border border-white/20 rounded">

### Responder

- Revogar credencial/sessão e rotacionar segredo
- Plano de resposta a incidente + LGPD/ANPD
- Postmortem sem culpa, corrigir a classe do erro

</div>

</div>

<!-- IMG: ícones de escudo/lupa/bomba para as três colunas (opcional, o grid textual já funciona sozinho) -->

<!--
- Se o tema não tiver `three-cols`, este grid de 3 colunas cumpre o papel.
- A ordem importa: reduzir corta o vetor, detectar diminui o tempo, responder limita o dano.
- LGPD: avaliar notificação à ANPD e aos titulares conforme o caso.
-->

---
layout: statement
---

# "O que você já removeu do código pode não ter saído do ar."

<div class="mt-10 text-base opacity-70">
  Perguntas?
</div>

<div class="abs-br m-6 text-sm opacity-60">
  Seu Nome · @seu_handle
</div>

<!-- IMG: fundo sutil de código/URLs antigas desbotando, para acompanhar a frase (opcional) -->

<!--
- Frase-âncora: o front esquece, o back não.
- Obrigado — abrir para perguntas.
- Deixar contato/handle visível para follow-up.
-->
