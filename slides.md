---
theme: apple-basic
layout: intro
class: text-center
background: /images/PLACEHOLDER-capa.png
title: "Arranhando a Superfície de Ataque: Escopos Desconhecidos e os Novos Riscos Impulsionados pela IA"
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

<style>
.slidev-layout.intro h1 {
  font-size: 3.25rem;
  line-height: 1.15;
}

.slidev-layout.intro h2 {
  font-size: 1.5rem;
}
</style>

<!-- IMG: capa — visual dark de "superfície" (mapa de IPs/portas, teia de conexões). O background aponta para /images/PLACEHOLDER-capa.png -->

<!--
[Abertura]
- Quem sou eu e o que faço.
- Promessa: não é pentest 101. É sobre o que o seu dia a dia cria sem você perceber — e como a IA acelerou isso.
- Aviso: nada aqui autoriza atacar terceiros. Reconhecimento só no SEU escopo.
-->

---

# O que é superfície de ataque

Tudo o que está exposto e pode ser alcançado por quem não é você.

- Domínios, subdomínios, portas e serviços
- APIs, rotas internas e endpoints esquecidos
- Dependências e imagens de container
- Pessoas com acesso (credenciais, sessões, e-mail)

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

# Phishing não é um ataque só

- **Spear phishing** — e-mail direcionado com contexto real do alvo
- **BEC / CEO fraud** — "sou eu, faz o pagamento, é urgente"
- **Vishing** — ligação se passando por suporte/banco/chefe
- **Smishing / quishing** — SMS e QR code levando ao fluxo falso
- Objetivo comum: obter credencial, burlar o MFA ou convencer alguém a agir

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

## layout: section

# Novos riscos impulsionados por IA

<!--
- Parte central da talk.
- Não é "IA é o futuro": são mecanismos concretos de ataque usando IA.
- Três: phishing/vishing clonado, shadow AI e slopsquatting.
-->

---

# Phishing e vishing turbinados por IA

Vazamentos deixam de ser só uma fonte de "dados cadastrais" e passam a ser matéria-prima para ataques altamente personalizados por IA.

| Antes                                              | Com dados expostos + IA                                                       |
| -------------------------------------------------- | ----------------------------------------------------------------------------- |
| "Olá, cliente. Seu banco identificou um problema." | Mensagem citando banco, cidade, agência ou produto que a pessoa realmente usa |
| Phishing genérico em massa                         | Phishing individualizado automaticamente                                      |
| Golpista precisava pesquisar a vítima              | IA correlaciona dados de múltiplas fontes                                     |
| Texto mal escrito                                  | Mensagens linguisticamente naturais e adaptadas ao perfil                     |
| Poucas tentativas por operador                     | Milhares de variações geradas automaticamente                                 |
| Engenharia social baseada em informação            | Engenharia social baseada em contexto                                         |

<!--
- Ponto central: o vazamento é o combustível. Dados cadastrais (CPF, endereço, agência, produto) permitem personalização em escala.
- Antes: o golpista pesquisava a vítima e escrevia um texto genérico e cheio de erros. Agora: a IA correlaciona múltiplos vazamentos e gera mensagens naturais no tom certo.
- Clonagem de voz: ~3s de amostra já bastam em TTS moderno; a fonte é palestra, podcast ou saudação de voicemail.
- Arup 2024: o funcionário viu e ouviu "o CFO" numa videochamada (deepfake de rosto); a empresa perdeu cerca de US$25M. O que denunciou o golpe foi o pedido de mudar o número de callback.
- Defesa: verificação fora de banda (canal/número conhecido), MFA resistente a phishing, desconfiar de urgência + mudança de dados bancários.
-->

---

# Os dados à venda

<div class="h-full flex flex-col items-center justify-center gap-4 px-8 py-6">
  <span class="text-sm opacity-60"></span>
  <div class="flex items-start justify-center gap-6">
    <img src="/images/catalogo-de-dados-1.png" alt="Catálogo de dados pessoais — parte 1: tipos de dados" class="rounded border border-white/20" style="max-height: 480px" />
    <img src="/images/catalogo-de-dados-2.png" alt="Catálogo de dados pessoais — parte 2: planos e preços" class="rounded border border-white/20" style="max-height: 480px" />
  </div>
</div>

---

<div class="mt-4 flex items-end justify-center gap-6">
  <figure class="flex flex-col items-center gap-2">
    <img src="/images/data-sample.png" alt="Print censurado de canal no Telegram anunciando venda de dados e acesso" class="rounded border border-white/20" style="max-height: 220px" />
    <figcaption class="text-xs opacity-60"></figcaption>
  </figure>
</div>
<!--
- Divisão em duas metades iguais (322×468) para exibir a ~100% sem cortar conteúdo.
- Parte 1: cabeçalho + lista de dados (CPF a VACINAS).
- Parte 2: restante da lista (FOTOS a PIX) + planos e preços.
-->

---

<div class="h-full flex flex-col items-center justify-center gap-3 px-8 py-4">
  <img src="/images/hospital-di-camp.png" alt="Relatório de vazamento do Hospital Di Camp, em Campo Grande" class="rounded border border-white/20" style="max-height: 460px" />
  <span class="text-xs opacity-60"></span>
</div>

<!--
- Escala: não é um vazamento pontual, é a população inteira (213.968.521).
- 13,6 Gb, formato .DB, PoC de 5 milhões de registros.
-->

---

<div class="h-full flex flex-col items-center justify-center gap-3 px-8 py-4">
  <img src="/images/serpro-214-milhoes.png" alt="" class="rounded border border-white/20" style="max-height: 460px" />
  <span class="text-xs opacity-60"></span>
</div>

---

# Phishing e vishing turbinados por IA
