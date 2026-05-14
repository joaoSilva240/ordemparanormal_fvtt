<div align="center">
  <img src="media/op-logo.png" alt="Logo do sistema Ordem Paranormal" />
</div>

# Ordem Paranormal para Foundry VTT

## Visão geral

Este repositório mantém um **sistema não oficial de Ordem Paranormal para o Foundry Virtual Tabletop**. O projeto adapta regras, fichas, itens, rolagens, interfaces e compêndios para uso dentro do Foundry, com foco em mesas que utilizam o cenário e a estrutura de jogo de Ordem Paranormal.

## Estado atual do projeto

- Manifesto principal em `system.json`.
- Compatibilidade declarada com **Foundry VTT 12+**, com `minimum: 12` e `verified: 13.350`.
- Ponto de entrada do runtime em `module/ordemparanormal.mjs`.
- Estrutura já migrada para fluxos compatíveis com **Foundry v12/v13** e com uso de **Application V2** nas partes recentes da interface.
- Sistema próprio de rolagem d20 registrado no runtime, além de templates, migrações, localização e compêndios empacotados.
- Integração opcional com **Bar Brawl** em `module/hooks.mjs` quando o módulo estiver ativo; não é dependência obrigatória para o sistema funcionar.

## Tecnologias e linguagem utilizadas

O código refletido neste repositório usa principalmente:

- **JavaScript ES Modules** (`.mjs`) para runtime, documentos, folhas, hooks, utilitários e scripts de manutenção.
- **Handlebars** (`.hbs`) e alguns templates **HTML** (`.html`) para folhas, chat cards, diálogos e partes reutilizáveis da interface.
- **SCSS** como fonte de estilos em `scss/`, compilado para **CSS** em `css/ordemparanormal.css`.
- **JSON** para manifesto, estrutura de dados-base, localização e conteúdo-fonte de compêndios.

Ferramentas de desenvolvimento presentes no projeto:

- `sass` via script npm para acompanhar a compilação de estilos.
- `@foundryvtt/foundryvtt-cli` para empacotar e desempacotar compêndios.
- `gulpfile.js` como fluxo legado/alternativo para compilação de SCSS.

## Estrutura do repositório

### Arquivos centrais

- `system.json`: manifesto do sistema, versões compatíveis do Foundry, módulos ES carregados, estilos, tipos de documento e compêndios.
- `template.json`: modelo base de dados do sistema para `Actor` e `Item`.
- `module/ordemparanormal.mjs`: bootstrap do sistema; registra documentos, sheets, dados de rolagem, fontes, settings, templates e migrações.
- `README.md`: documentação principal do repositório.
- `CHANGELOG.md`: histórico recente de mudanças.

### Código de runtime

- `module/applications/`: aplicações e diálogos do sistema, incluindo partes da interface compatíveis com Application V2.
- `module/components/`: componentes de interface e mensagens auxiliares.
- `module/dice/`: implementação das rolagens, incluindo o fluxo próprio de d20.
- `module/documents/`: classes customizadas de documentos do Foundry, como atores, itens e mensagens de chat.
- `module/helpers/`: configurações e helpers usados pelo runtime.
- `module/settings/`: registro de configurações e atalhos do sistema.
- `module/sheets/`: fichas e folhas customizadas para atores, ameaças e itens.
- `module/hooks.mjs`: hooks do sistema, incluindo integração opcional com Bar Brawl e ajustes de interface.
- `module/migrations.mjs`: rotinas de migração de dados entre versões.
- `module/utils.mjs`: utilitários carregados pelo runtime, como preload de templates.

### Interface e apresentação

- `templates/actor/`, `templates/threat/`, `templates/item/`: templates das fichas principais.
- `templates/chat/`: cards e renderizações de chat.
- `templates/dice/`: templates das rolagens e fórmulas.
- `templates/apps/`: templates de aplicações auxiliares.
- `templates/dialog/`: diálogos do sistema.
- `templates/shared/`: trechos compartilhados reutilizados entre telas.
- `lang/`: arquivos de localização, atualmente com `pt-BR.json` e `en.json`.
- `scss/`: fontes SCSS dos estilos.
- `css/`: CSS compilado consumido pelo Foundry.
- `media/`: imagens, fontes e assets estáticos carregados pela interface.

### Conteúdo e utilitários

- `packs/`: compêndios já empacotados no formato utilizado pelo Foundry.
- `packs/_source/`: fontes em JSON dos compêndios versionadas no repositório.
- `utils/packs.mjs`: utilitário para empacotar (`pack`) e desempacotar (`unpack`) compêndios.
- `utils/semver-compare.mjs`: utilitário auxiliar relacionado a versionamento.

## Como o sistema funciona no Foundry

Em tempo de execução, o Foundry lê o manifesto `system.json`, carrega `module/ordemparanormal.mjs` como módulo ES e aplica os estilos declarados em `css/ordemparanormal.css`. A partir daí, o sistema:

1. registra classes customizadas de `Actor`, `Item` e `ChatMessage`;
2. define sheets próprias para agentes, ameaças e itens;
3. carrega templates Handlebars usados nas fichas, diálogos e chat;
4. registra configurações, atalhos e helpers;
5. habilita o fluxo próprio de rolagem d20;
6. executa migrações quando necessário para manter dados compatíveis entre versões;
7. expõe compêndios declarados em `system.json` para uso dentro do mundo.

Os dados-base do sistema são estruturados em `template.json`, enquanto a apresentação e a interação passam pelos templates em `templates/` e pelo código em `module/`.

## Desenvolvimento local

### Pré-requisitos

- Node.js e npm instalados.
- Uma instalação do Foundry VTT com acesso à pasta `Data/systems`.

### Instalação

1. Clone o repositório.
2. Instale as dependências com:

```bash
npm install
```

### Scripts atuais

Os scripts disponíveis em `package.json` hoje são:

```bash
npm run sass
npm run build:db
npm run build:json
```

#### `npm run sass`

Inicia o `sass --watch` para compilar `scss/ordemparanormal.scss` em `css/ordemparanormal.css` enquanto você altera os estilos.

#### `npm run build:db`

Empacota os JSONs versionados em `packs/_source/` para os bancos de compêndio em `packs/`.

#### `npm run build:json`

Faz o caminho inverso: extrai/desempacota os compêndios declarados em `system.json` para JSON em `packs/_source/`.

### Fluxo realista de manutenção

Um fluxo prático para trabalhar neste sistema é:

1. editar código em `module/`, templates em `templates/` ou dados-base em `template.json`;
2. editar estilos em `scss/` e manter `npm run sass` rodando durante o trabalho visual;
3. ao alterar compêndios, sincronizar `packs/` e `packs/_source/` com `npm run build:db` ou `npm run build:json`, conforme a direção da mudança;
4. validar o carregamento do sistema diretamente no Foundry em uma instalação compatível com v12/v13.

Se preferir um fluxo legado para estilos, o repositório ainda possui `gulpfile.js` como alternativa de compilação SCSS, mas o script npm com `sass` é o caminho mais direto já configurado.

## Compêndios e conteúdo

Os compêndios distribuídos pelo sistema são declarados em `system.json` e ficam em `packs/`. O conteúdo-fonte versionável permanece em `packs/_source/`, o que facilita revisão, diff e manutenção de itens, habilidades, tabelas, cenas e documentação.

Entre os conteúdos presentes atualmente estão, por exemplo:

- armamentos;
- equipamentos gerais;
- proteções;
- habilidades de classes, origens, trilhas e poderes paranormais;
- tabelas auxiliares;
- mapas;
- documentação do sistema.

## Observações sobre este fork

Este repositório representa o estado atual de um **fork aprimorado** do sistema não oficial de Ordem Paranormal para Foundry VTT. Ele é mantido com foco prático nas necessidades da mesa que o utiliza hoje, preservando a base existente e ajustando compatibilidade, interface, rolagens, localização e conteúdo conforme necessário.

**Importante:** este projeto é um **fork aprimorado para uso pessoal e restrito do grupo de RPG "Lol e RPG"**.
