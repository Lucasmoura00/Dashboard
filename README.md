# Dashboard de Producao

Projeto frontend em HTML, CSS e JavaScript para login e visualizacao de dashboards de producao em tempo real, com dados vindos do Firebase Realtime Database e complemento de informacoes via Google Sheets.

## Visao geral

O projeto possui:

- tela de login com validacao de usuario e senha no Firebase;
- dashboard da unidade padrao;
- dashboard da unidade especial;
- consolidado por hora, total do dia, tendencia, capacidade e desvio;
- tabela de OPs com cruzamento entre producao do dia e planilha do Google Sheets.

## Estrutura do projeto

```text
dashboard-main/
|-- index.html
|-- css/
|   |-- style.css
|   |-- style2.css
|   `-- style3.css
|-- js/
|   |-- script.js
|   `-- script2.js
`-- pages/
    |-- index.html
    |-- index2.html
    `-- index3.html
```

## Fluxo das paginas

### `index.html`

Arquivo de entrada da aplicacao. Ele redireciona automaticamente para `pages/index.html`.

### `pages/index.html`

Tela de login. Faz leitura no Firebase em:

```text
usuarios/{usuario}/senha
```

Se a senha estiver correta, o usuario e redirecionado para `pages/index2.html`.

### `pages/index2.html`

Dashboard da unidade padrao. Carrega o script `js/script.js`.

### `pages/index3.html`

Dashboard da unidade especial. Carrega o script `js/script2.js`.

## Fontes de dados

### Firebase Realtime Database

O projeto usa Firebase via CDN, sem build e sem backend local.

Bases utilizadas:

- Login: `https://gwcontas-3251a-default-rtdb.firebaseio.com/`
- Dashboard padrao: `https://appqrcode-8d0d2-default-rtdb.firebaseio.com/`
- Dashboard especial: `https://qrcodegw-b14df-default-rtdb.firebaseio.com/`

Os dashboards consultam historicos em caminhos no formato:

```text
usuarios/{celula}/historico/{dd-mm-aaaa}
```

### Google Sheets API

As OPs exibidas na tabela complementar sao enriquecidas com dados de uma planilha Google, usando:

- `SHEET_ID`
- `SHEET_API_KEY`
- intervalo `A:C`

## Logica dos dashboards

Os scripts calculam:

- producao por faixa horaria;
- hora extra;
- total produzido;
- tendencia do dia;
- capacidade percentual;
- desvio em relacao a meta.

Tambem existe atualizacao automatica a cada `90` segundos e recarga manual ao trocar a data no campo do dashboard.

## Diferencas entre os scripts

### `js/script.js`

- unidade padrao;
- considera 10 celulas (`Celula01` ate `Celula10`);
- meta por hora: `50`;
- meta diaria por celula: `440`;
- meta total do dia: `4400`.

### `js/script2.js`

- unidade especial;
- monta tabela principal com 5 celulas (`Gw01` ate `Gw05`);
- meta por hora: `51`;
- meta diaria por celula: `450`;
- meta total do dia: `1800`.

## Como executar

Como o projeto e estatico, voce pode abrir o `index.html` direto no navegador ou servir a pasta com um servidor local simples.

Exemplo com VS Code:

1. Abra a pasta do projeto.
2. Inicie uma extensao de servidor estatico, como Live Server.
3. Abra o arquivo `index.html`.

## Requisitos

- navegador moderno;
- acesso a internet para carregar:
  - Firebase via CDN;
  - Google Fonts;
  - Google Sheets API.

## Observacoes

- O projeto nao usa `package.json` nem processo de build.
- As credenciais e endpoints estao declarados diretamente nos arquivos HTML e JS.
- O login atual compara a senha digitada com a senha salva no banco, no lado do cliente.

## Melhorias futuras

- mover configuracoes sensiveis para um ambiente mais seguro;
- revisar autenticacao para evitar validacao de senha no frontend;
- padronizar encoding dos arquivos para evitar textos com caracteres corrompidos;
- separar configuracoes de metas e endpoints em um arquivo central;
- adicionar um README tecnico para manutencao dos dados no Firebase.
