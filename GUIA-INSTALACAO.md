# Guia de instalação do Crivo Notarial Desktop no cartório

Para o escrevente e o Titular da serventia. Versão de referência: **Crivo
Notarial Desktop 0.1.17** (canal `desktop`). Tudo o que está aqui descreve o que
o aplicativo faz hoje — telas, mensagens e limites vêm do próprio produto
(ADR-0070/0071/0072 e `docs/desktop/gateway-local.md`).

## O que é (em uma frase)

O Crivo Notarial Desktop é o Crivo rodando **dentro do computador do cartório**:
os dados de trabalho (pessoas, imóveis, minutas, certidões em PDF) ficam no
próprio computador; a nuvem da Crivo serve só para login, licença e alguns
serviços sem dados pessoais. Cada computador tem a sua própria base — dois
computadores **não** compartilham dados entre si (isso é por desenho; o backup
e a restauração, abaixo, são o caminho para mover dados de uma máquina a outra).

## 1. Antes de instalar — checklist

- [ ] **Windows 10 ou 11 (64 bits)**. Não precisa de usuário administrador: a
      instalação é "por usuário" e os dados ficam na pasta do perfil Windows
      (`%LOCALAPPDATA%\CrivoDesktop`). Por isso, **instale e use sempre com o
      mesmo usuário Windows** — cada usuário Windows tem a sua própria base.
- [ ] **4 GB de memória** e **2 GB livres em disco** (o banco e os PDFs
      crescem com o uso).
- [ ] **Google Chrome instalado** no local padrão (`C:\Program Files\Google\Chrome`
      ou `C:\Program Files (x86)\...`). As certidões são emitidas com o Chrome
      de verdade do computador; sem ele a emissão falha com a mensagem
      "chrome.exe não encontrado — instale o Google Chrome no computador".
- [ ] **Internet** no computador. O login, a licença, a extração de documentos
      e a resolução de captcha passam pela nuvem; os portais de certidão são
      acessados diretamente pelo Chrome do cartório.
- [ ] **Conta Crivo já criada** para quem vai usar o computador (o convite de
      onboarding da Crivo cria a conta, a serventia e o período de teste). O
      Desktop não cria contas — ele faz login numa conta que já existe.
- [ ] **Titular com aplicativo autenticador no celular** (Google Authenticator,
      Microsoft Authenticator ou similar): a conta do Titular exige verificação
      em duas etapas (MFA). Se ainda não estiver ativada, o app pede para ativar
      no primeiro login.
- [ ] **Antivírus**: o app inicia dois programas auxiliares no próprio
      computador (`postgres.exe` e `postgrest.exe`). Eles **não abrem porta na
      rede** — só conversam dentro da máquina (porta local 54321). Se o
      antivírus do cartório bloquear programas desconhecidos, avise o
      responsável de TI antes (ver "Se algo falhar").

## 2. Baixar o instalador

**Pelo Crivo na internet** — entre em www.crivonotarial.com.br, vá em
**Configurações → aba "Crivo Desktop"** e clique em **"Baixar instalador"**
(o botão ao lado, "Guia de instalação", abre este guia).

**Ou pelo caminho direto** — abra este endereço no navegador e o download do
instalador da versão mais recente começa sozinho (o arquivo
`CrivoDesktop-windows-x64-setup.exe`, cerca de 74 MB, vai para a pasta
Downloads):

**https://github.com/diogohmendesr-create/crivo-desktop-releases/releases/latest/download/CrivoDesktop-windows-x64-setup.exe**

O instalador **não fica em nenhuma pasta** do computador nem na lista de
arquivos do GitHub — ele só existe como download pelo endereço acima (ou pela
página abaixo). Se você cair na página inicial do repositório (uma lista de
pastas e arquivos), procure **"Releases"** na coluna da direita.

Se preferir ver a página da versão (ou se o endereço acima não baixar nada):

1. Abra **https://github.com/diogohmendesr-create/crivo-desktop-releases/releases/latest**
   É uma página do GitHub (onde a Crivo publica as versões): no alto, o título
   com o nome do aplicativo e a versão mais recente, e um parágrafo curto. **O download
   fica na caixa "Assets"**, logo abaixo desse texto — se a caixa estiver
   recolhida, clique em "Assets" para abrir.
2. Na caixa "Assets" aparecem quatro itens. Clique só em
   **`CrivoDesktop-windows-x64-setup.exe`**.
   - `latest.json` é de uso interno do atualizador — ignore.
   - "Source code (zip)" e "Source code (tar.gz)" são o código-fonte, que o
     GitHub adiciona sozinho a toda versão — ignore.
   - Se algum dia aparecer um arquivo `.msi`, não use (é para instalação
     corporativa por TI e criaria uma segunda instalação paralela).

## 3. Instalar — e o aviso do SmartScreen

1. Dê dois cliques no `CrivoDesktop-windows-x64-setup.exe`.
2. O Windows vai mostrar uma tela azul **"O Windows protegeu o seu computador"**.
   Isso acontece porque o instalador **ainda não tem certificado de assinatura
   de código** (a Crivo emitirá quando o CNPJ estiver constituído). Não é
   detecção de vírus — é o Windows dizendo que não conhece o programa.
   - Clique em **"Mais informações"** e depois em **"Executar assim mesmo"**.
   - Se o botão "Executar assim mesmo" não aparecer, o SmartScreen do
     computador está em modo "bloquear"; o responsável de TI precisa liberar.
     **Não** use truques de "desbloquear arquivo" nem extrair com programas de
     compactação — o procedimento oficial é o botão acima.
3. O instalador roda sem pedir senha de administrador e cria "Crivo Notarial
   Desktop" no menu Iniciar.
4. **Instale somente o arquivo baixado pelos caminhos da seção 2** — nunca um
   instalador recebido por e-mail, WhatsApp ou pendrive, mesmo que pareça
   vir da Crivo. Enquanto não há certificado de assinatura, o que garante a
   origem é o endereço de download (e o código SHA-256 publicado na página
   da versão, que o TI pode conferir).

## 4. Primeira abertura

Ao abrir, aparece uma tela de preparação com as etapas:

- "Verificando ambiente…" → "Iniciando o serviço local…"
- "Verificando atualizações…" → tela de login.

Num computador comum isso leva **menos de 30 segundos** na primeira vez
(medido: 9 segundos, incluindo a montagem do banco do zero) e poucos segundos
nas aberturas seguintes. Se o computador for lento ou o antivírus estiver
inspecionando os programas novos, depois de 10 segundos aparece
**"Preparando o banco de dados local — a primeira execução pode levar alguns
minutos…"** — é normal; o limite é de 3 minutos antes de o app desistir com a
mensagem "não respondeu a tempo" (tabela abaixo).

Mensagens possíveis nessa tela e o que significam:

| Mensagem | O que fazer |
|---|---|
| "A porta 54321 está em uso por outro programa. Feche outros aplicativos Crivo (ou o Supabase CLI) e tente novamente." | Há outro Crivo Desktop aberto (talvez em outro usuário Windows) ou um programa técnico usando a mesma porta. Feche-o e clique em "Tentar novamente". |
| "Encerrando instância anterior…" | Ficou um serviço local de uma abertura anterior (o app foi fechado à força ou o computador desligou). Normal: o app o encerra e segue sozinho em poucos segundos. |
| "Aguardando a instância anterior finalizar um trabalho em curso…" | O app anterior ainda está terminando uma emissão. Espere; ele substitui sozinho. |
| "O serviço local não respondeu a tempo. Tente novamente ou contate o suporte." | Geralmente antivírus segurando o `postgres.exe` na primeira execução. Tente de novo. **Se repetir, e principalmente se o nome da conta do Windows tiver acento (José, Antônio):** baixe a versão mais recente e instale por cima, sem desinstalar e sem apagar `%LOCALAPPDATA%\CrivoDesktop`. Depois veja "Se algo falhar". |
| "Instalação incompleta — reinstale o Crivo Notarial Desktop." | Arquivos do instalador faltando. Baixe e instale novamente. |

**Fechar a janela não fecha o app.** O "X" da janela só a esconde: o Crivo
Desktop continua rodando na **bandeja do Windows** (ícone ao lado do relógio),
cuidando da fila, do backup automático e da verificação do TJ-SP. Para abrir a
janela de novo, clique no ícone da bandeja (ou no atalho do menu Iniciar). Para
encerrar de verdade: botão direito no ícone da bandeja → **"Sair"**.

## 5. Login

1. Informe o **e-mail e a senha da conta Crivo** e clique em **Entrar**.
   ("Verifique email e senha." = credenciais erradas.)
2. **Titular:** se a verificação em duas etapas ainda não estiver ativa, o app
   mostra **"Ativar MFA"** — leia o QR code com o aplicativo autenticador e
   digite o código. Nas próximas entradas, o código de 6 dígitos é pedido
   ("Verifique no seu aplicativo autenticador."), exceto em dispositivo já
   marcado como confiável.
3. O login precisa de internet. Depois de logado, o trabalho é feito
   localmente.

**Modo somente-leitura.** Se a assinatura/teste da serventia não estiver ativa,
ou se o computador ficar **mais de 7 dias sem conseguir falar com a nuvem**, o
app entra em somente-leitura e avisa: *"O Crivo Desktop está em modo
somente-leitura — gravar está desabilitado, mas exportar e baixar continuam
disponíveis."* Consultar, exportar e baixar **nunca** são bloqueados. Para
voltar ao normal: reconecte à internet e entre de novo (ou regularize a
assinatura).

## 6. Backup — obrigatório, e a senha é só sua (Titular)

O backup protege o cartório contra disco quebrado, computador roubado ou
formatação. **Ele é a única cópia dos dados fora deste computador: a nuvem da
Crivo não recebe os arquivos de backup.**

Onde: na barra lateral, seção **GOVERNANÇA → Configurações** (o mesmo item
existe no menu da conta, nas suas iniciais no rodapé da barra) → última aba,
**"Crivo Desktop"**. A tela que abre se chama **"Manutenção — Crivo Desktop"**. Só o Titular vê a configuração ("A
configuração de backup é uma ação exclusiva do Titular da serventia.").

1. **Pasta de backup neste computador**: uma pasta que já exista (o app **não
   cria** a pasta).
   Exemplos: `D:\Backups\Crivo` ou `\\servidor\backups`. O ideal é uma pasta
   **fora deste computador** (servidor da rede, HD externo, pasta sincronizada
   com uma nuvem de arquivos do cartório).
2. **Senha de backup** + **Confirmar senha**. Leia o aviso da tela — ele é
   literal:

   > **Guarde esta senha. Sem ela, NENHUM backup poderá ser restaurado — não há
   > recuperação.**

   A senha é **do Titular, guardada fora do app** (cofre de senhas do cartório,
   envelope lacrado — o que a serventia usar para segredos). **A Crivo não tem,
   não guarda e não consegue recuperar essa senha.** Perdeu a senha = os
   backups já feitos viram arquivos inúteis. Trocar a senha vale só para os
   backups seguintes.
3. **Backup automático diário**: vem **Desligado** — clique para deixar
   **Ligado**. Ele roda uma vez por dia, sem interromper o trabalho (o
   computador precisa estar ligado com o app rodando em algum momento do dia).
4. Clique em **"Salvar configuração"** (aparece "Configuração de backup
   salva") e depois em **"Fazer backup agora"**. Em poucos segundos a tela
   mostra "Backup concluído: …" e o arquivo aparece na lista **"Backups na
   pasta configurada"** e na pasta, com o nome
   `crivo-backup-<data>-<hora>-<id>.crivobak` (a hora do nome é a universal,
   3 horas à frente de Brasília; a coluna "Criado em" da lista mostra a hora
   local). O arquivo é cifrado; só abre com a senha. A coluna "Versão" é a
   versão interna do serviço local (ex.: 0.2.10 — acompanha a do Crivo Companion), não a do aplicativo.

O card **"Status desta instalação"** na mesma tela mostra "Banco local" e "API
local" (devem estar "Ativo"/"Ativa"), as migrações aplicadas, o dispatcher de
emissões, a sincronização com a nuvem e o último backup bem-sucedido.

### Restaurar um backup (máquina nova ou disco trocado)

Na mesma tela, no fim, botão **"Restaurar um backup…"**. Abre um assistente
de 3 passos — **Escolher → Validar → Executar**:

1. **Escolher:** a lista mostra os backups da pasta configurada (arquivo, data,
   tamanho); marque o desejado ou informe o caminho completo de um arquivo
   vindo de outro computador. Digite a **senha de backup** → **Continuar**.
2. **Validar:** o app abre o arquivo **sem mexer em nada** ainda. Senha errada
   para aqui: *"Senha de backup incorreta. Use a senha definida quando o
   backup foi configurado — sem ela, o arquivo não pode ser aberto."* → Voltar
   e tentar de novo. Com a senha certa, mostra quando o backup foi criado e a
   versão, e avisa: *"Todos os dados atuais deste computador serão
   substituídos pelos do backup."* → **Continuar**.
3. **Executar:** digite **`RESTAURAR`** e clique em **"Restaurar agora"**. Em
   poucos segundos ("Validando o backup…" → "Restaurando banco…" →
   "Reiniciando…") aparece **"Restauração concluída"** → **"Recarregar
   agora"**. O app volta logado, com os dados do backup.

Os dados que estavam no computador **não são apagados**: ficam guardados em
`%LOCALAPPDATA%\CrivoDesktop\pgdata.pre-restore-<data-hora>` e
`storage.pre-restore-<data-hora>` por segurança (podem ser removidos depois,
quando tiver certeza de que a restauração está certa). Num computador
recém-instalado é preciso **fazer login com internet uma vez** antes de
restaurar.

## 7. Primeiro teste — uma certidão avulsa

Objetivo: provar que fila, Chrome e nuvem estão funcionando antes de usar em
ato real.

1. Barra lateral **Certidões** → tela **Painel** → botão **"Emitir avulsa"**
   (canto superior direito). Abre uma janela com o formulário: **Pessoa
   Física**, o **CPF** e a lista **"Tipos de certidão"** (marque só o que for
   testar). Conforme o tipo, aparecem campos extras: Receita Federal pede a
   **data de nascimento**; TJ-SP pede **nome completo**, **RG** (opcional) e
   **gênero**. O botão **"Emitir certidões selecionadas"** só habilita com
   tudo preenchido.
2. Use o **seu próprio CPF** (de quem está fazendo o teste) — os portais
   conferem o CPF na Receita; CPF inventado falha. Não use o CPF de outra
   pessoa sem autorização dela por escrito, guardada pelo cartório: a
   certidão emitida é real, fica no computador e entra nos backups. Ao
   terminar o teste, faça um backup novo (as certidões de teste ficam no
   Acervo como qualquer outra).
3. Depois de "Emitir", a janela fecha, aparece "1 certidão(ões)
   enfileirada(s)" e a certidão entra no painel agrupada pelo CPF. O status
   passa por **"Aguardando Companion"** (na fila — o nome é herdado de outro
   produto da Crivo; aqui significa só "na fila") → **"Processando"** →
   **"Emitida"**, com o botão **"Baixar PDF"**. O PDF é salvo na pasta
   **Downloads** do Windows (aparece o balão de download no canto superior
   direito do app) — o mesmo vale para "Baixar" no Acervo e nos alertas.
4. **Primeiro teste: CNDT (PF).** Não usa o Chrome (é direto com o TST) —
   cerca de **15 segundos**.
5. **Segundo teste: Receita Federal (PF).** O app usa o **Google Chrome em
   segundo plano** para preencher o portal — cerca de **1 minuto e meio**.
   A partir da versão 0.1.17, nenhuma janela fica na tela (no máximo uma piscada
   rápida no começo; nas versões anteriores a janela do Chrome aparece): pode
   continuar trabalhando normalmente e acompanhe pelo status na tela do Crivo.
   Em alguns computadores — quando o antivírus ou uma política da rede bloqueia
   o recurso de ocultar — a janela do Chrome **aparece, como sempre apareceu**:
   nesse caso não feche nem mexa nela; a emissão funciona do mesmo jeito. Se a
   pessoa já tiver uma certidão da Receita ainda válida, o portal devolve
   essa mesma certidão (com a data de emissão original) — é o comportamento
   da Receita, não um erro.
6. **TJ-SP (Justiça Estadual) é diferente:** o app faz o pedido no e-SAJ em
   segundo plano (cerca de 1 minuto) e a certidão fica como **"Aguardando e-SAJ"**;
   o tribunal gera o documento depois de um tempo e o app volta lá sozinho
   para buscar o PDF — a cada 6 horas enquanto estiver rodando (e ao abrir,
   se já passaram 6 horas desde a última verificação). Em geral o pedido fica
   pronto em minutos, mas o prazo oficial é de dias — não é erro. Computador
   desligado não verifica: o app precisa estar rodando (a janela pode estar
   fechada — basta o ícone na bandeja).

Se uma emissão falhar, o painel mostra o motivo em português. Falhas
temporárias (portal fora do ar, instabilidade) são tentadas de novo sozinhas;
falhas definitivas (dado inválido, portal recusou) pedem correção do dado e
nova emissão.

## 8. Minuta de escritura com IA (opcional — Titular)

O módulo de escritura (a aba **Escritura** nas minutas com tipo de ato
"Escritura") e a IA que ajuda a redigir e conferir o texto são **configurações
deste computador**, não da conta na nuvem. Ligar o módulo no site da Crivo
**não** liga no Desktop, nem o contrário — a sincronização com a nuvem copia
só o nome e o CNPJ da serventia. Cada computador do cartório é configurado
separadamente, nos dois passos abaixo.

1. **Ligar o módulo (Titular).** Barra lateral **GOVERNANÇA → Configurações**,
   aba **"Geral"**, card **"Minuta de escritura (opcional)"** → botão
   **"Ativar módulo Escritura"** (o botão passa a "Ativado — desativar").
   Vale para todos os usuários deste computador. Sem isso a aba Escritura não
   aparece e os botões de IA respondem que o módulo está desativado.
2. **Conectar a conta OpenAI (Titular, um vínculo por computador).** Mesma
   tela de Configurações, última aba **"Crivo Desktop"** (a tela "Manutenção —
   Crivo Desktop"), card **"Conta OpenAI (IA local)"** → **"Conectar conta
   OpenAI"**. Aparece um código; clique em **"Abrir página de ativação"**,
   entre com a assinatura ChatGPT **do cartório** e digite o código. O estado
   passa de "Aguardando ativação" para **"Conta conectada"** sozinho.
   - O vínculo fica **neste computador, na conta do Windows em uso**,
     protegido pelo Windows. **Não entra no backup e não vai para a nuvem** —
     computador novo, disco restaurado ou outra conta do Windows = conectar de
     novo (é só repetir este passo).
   - **"Desconectar"** apaga o vínculo deste computador; a assinatura ChatGPT
     do cartório não é alterada.

**O que a IA faz no Desktop.** Na aba Escritura, cada seção tem **"Ajustar com
IA"** (reescreve o trecho conforme a instrução do escrevente) e **"Conferir com
IA"** (aponta exigências e alertas citando as normas do Crivo). O texto da
seção vai para a OpenAI **pela conta do cartório**, nunca para a Crivo. Sem
conta conectada, esses dois botões avisam que não há provedor de IA e mostram
o botão **"Conectar conta OpenAI"**, que leva direto à aba "Crivo Desktop" do
passo 2; o resto do módulo (gerar a minuta, lacunas `[[FALTA]]`, modelos,
exportação) funciona normalmente sem IA.

## 8.1 Conector para o Claude Desktop (opcional — Titular)

Se o cartório usa o **Claude Desktop** (aplicativo da Anthropic) para redigir,
dá para ligar os dois: o Claude passa a consultar as qualificações e as
certidões do Crivo Desktop e a emitir só as que faltam. Tudo fica nesta
máquina; nada vai para a nuvem da Crivo por causa do conector — o que o Claude
lê vai para a Anthropic pela conta Claude do cartório (ver os Termos de Uso,
seção 4-B, em www.crivonotarial.com.br/termos). Passos conferidos no Claude
Desktop 2.2553.1 (Microsoft Store) em 18/09/2026.

1. No Crivo Desktop, **Configurações → Manutenção → "Conector para o Claude
   Desktop"** → **"Baixar conector (.mcpb)"**. Guarde o arquivo na pasta
   Downloads.
2. Ainda nessa tela, clique em **"Gerar chave de pareamento"** e copie a chave
   (ela aparece uma única vez — se perder, gere outra).
3. No Claude Desktop: **Configurações** (Ctrl+,) → **Extensões** →
   **"Configurações avançadas"** → **"Instalar extensão"** → escolha o arquivo
   baixado (ou arraste o arquivo para a tela de Extensões, onde está escrito
   "Arraste arquivos .MCPB ou .DXT aqui para instalar"). O duplo clique no
   arquivo **não** funciona na versão da Microsoft Store.
4. Confira o nome **"Crivo Notarial Desktop"** e a lista de ferramentas, clique
   em **"Instalar"** e confirme a caixa "Deseja instalar Crivo Notarial
   Desktop?".
5. Cole a chave no campo **"Chave de pareamento do Crivo Desktop"** e clique em
   **"Salvar"**. A extensão é instalada **desligada**: no topo da tela da
   extensão, ligue o botão ao lado de "Desabilitado" — deve mudar para
   **"Ativado"**. (Para voltar a essa tela depois: Extensões → "Crivo Notarial
   Desktop" → **"Configurar"**.)
6. Na mesma tela, em **"Permissões de ferramentas"**, o Claude Desktop separa
   as ferramentas em dois grupos. Em **"Ferramentas somente leitura"** (8:
   estado do Crivo Desktop, buscar pessoa, qualificação completa, listar
   minutas, detalhe de uma minuta, listar certidões, ler uma certidão,
   acompanhar emissões) escolha **"Sempre permitir"** para não confirmar cada
   consulta. Em **"Ferramentas de gravação/exclusão"** há só **"Emitir
   certidões (consome créditos)"**: deixe em **"Requer aprovação"**, que é o
   padrão — assim cada emissão pede o seu OK e nenhum crédito é gasto sem você
   ver. Nunca marque "Sempre permitir" nessa.
7. Abra uma conversa nova e peça: *"verifique a conexão com o Crivo"*. Se
   você não mexeu nas permissões, o Claude pede autorização na hora ("Sempre
   permitir" vale para as consultas). A resposta deve mostrar `pareamento:
   paired` e a sessão `ok`.

**A sessão precisa estar ativa.** A chave só funciona neste computador, com o
Crivo Desktop **aberto e logado com a conta do Titular** que a gerou. Por
segurança, o Crivo Desktop encerra a sessão depois de **8 horas sem ninguém
mexer nele** (as chamadas do Claude não contam como uso): na manhã seguinte,
ou depois de um fim de semana, o Claude responde "Abra o Crivo Desktop e entre
com a conta que criou o pareamento" — basta entrar de novo no Crivo Desktop e
repetir o pedido. Quedas curtas de internet não derrubam a sessão.

**Atualizar ou desfazer.** Quando o Crivo Desktop atualizar (seção 9), baixe o
conector de novo pela tela de Manutenção e instale por cima pelo mesmo caminho
do passo 3: o Claude Desktop reconhece como "Atualizar" e mantém a chave (só
cole uma chave nova se você tiver gerado outra no Crivo Desktop); confira as
permissões do passo 6 depois, porque o Claude pode voltar a pedir autorização.
Para desfazer a integração, clique em **"Revogar"** na tela do Crivo Desktop (o
Claude passa a receber "não pareado"); para remover a extensão, no Claude
Desktop use **"…" → "Desinstalar"** ao lado dela.

## 9. Atualizações

O Desktop **nunca instala uma atualização sem perguntar**. Quando houver
versão nova, um aviso amarelo aparece no topo do app com o botão **"Reiniciar
e atualizar"**. Ao clicar, o app confirma que vai reiniciar (trabalhos em
andamento — emissões, backup — terminam antes; salve o que estiver
editando), fecha e reabre sozinho, e aí pergunta se pode instalar. Se não
houver backup nas últimas 24 horas, a pergunta é: *"Não há backup recente.
Recomendamos fazer um backup pela tela Manutenção antes de atualizar. Instalar
mesmo assim?"* — a recomendação é responder **Não**, fazer o backup pela tela
Manutenção e só então atualizar (o aviso continua lá; basta clicar de novo).
Durante a atualização o app fecha e reabre; se der erro, ele mostra "Tentar
novamente" sem perder os dados. Sair e abrir o app à mão continua
funcionando: a pergunta aparece na abertura — mas **sair é pelo ícone da
bandeja (botão direito → Sair)**; o X da janela só a esconde, e reabrir pela
bandeja volta à mesma sessão, sem passar pela verificação de versão.

## 10. Se algo falhar

| Sintoma | Causa provável | O que fazer |
|---|---|---|
| Tela azul do Windows no instalador | SmartScreen sem certificado | "Mais informações" → "Executar assim mesmo" (seção 3). |
| Primeira abertura fica em "Preparando o banco de dados local…" e termina em "não respondeu a tempo" | Antivírus bloqueando `postgres.exe` ou a pasta `%LOCALAPPDATA%\CrivoDesktop` — **ou nome de conta do Windows com acento** (José, Antônio), corrigido a partir da versão 0.1.9 | TI adiciona exceção para a pasta do programa e para `%LOCALAPPDATA%\CrivoDesktop`; abra o app de novo. Se o nome da conta tem acento, **baixe a versão mais recente e instale por cima** — a atualização automática não alcança quem nunca conseguiu abrir o app. |
| "A porta 54321 está em uso por outro programa" | Outro Crivo Desktop aberto (outro usuário Windows) ou programa técnico na porta | Feche o outro programa; "Tentar novamente". |
| O app foi fechado à força / o computador desligou no meio | — | Basta abrir de novo: o app substitui sozinho o serviço que ficou para trás. |
| "O serviço local encerrou inesperadamente" logo depois de uma **restauração de backup interrompida** (app fechado ou computador desligado no meio) | O app se recusa a criar um banco novo por cima dos seus dados: eles estão na pasta `%LOCALAPPDATA%\CrivoDesktop\pgdata.pre-restore-<data>` | Chame o suporte. A recuperação é renomear essa pasta de volta para `pgdata` (e `storage.pre-restore-<data>` para `storage`, se existir) e abrir o app — nada foi perdido. |
| "Este Crivo Desktop está vinculado a outra serventia." ao entrar | Este computador já foi usado por uma conta de OUTRO cartório: o Desktop guarda os dados de um cartório só e recusa contas de outros | Entre com uma conta do cartório vinculado. Se o computador mudou de cartório de verdade, chame o suporte — a desvinculação apaga `%LOCALAPPDATA%\CrivoDesktop\org-binding.json` (e o banco local, que é do cartório anterior). |
| "chrome.exe não encontrado — instale o Google Chrome no computador" | Chrome ausente (a partir da versão 0.1.11 o Desktop encontra o Chrome instalado por máquina em `Program Files` E o instalado só para o usuário, em `%LOCALAPPDATA%`) | Instale o Google Chrome; qualquer uma das duas formas de instalação serve. |
| A janela do Google Chrome **aparece na tela** durante a emissão | O app oculta essa janela, mas só quando o computador deixa: antivírus ou política da rede bloqueando o PowerShell impedem, e o app então abre a janela como sempre abriu — a emissão não é afetada | Não feche nem mexa na janela. Se quiser a janela oculta, a TI libera o `powershell.exe` para o Crivo; o app confere de novo sozinho em até 7 dias (o suporte pode antecipar). |
| Cliquei em **"Baixar PDF"** (ou "Baixar" no Acervo) e nada acontece | Versão anterior à **0.1.17** — o app não conseguia abrir o download | Atualize pelo aviso de versão nova ("Reiniciar e atualizar"). Até lá, no painel de certidões o botão **"Baixar lote (.zip)"** funciona. |
| Certidão falhou com mensagem de portal | Portal instável ou dado inválido | Leia a mensagem no painel; temporária = espere a nova tentativa; definitiva = corrija o dado e emita de novo. |
| "modo somente-leitura" inesperado | Sem internet há dias, ou assinatura inativa | Reconecte, entre de novo; confira a assinatura com a Crivo. |
| Perdeu a senha de backup | — | Os backups antigos **não** têm recuperação. Defina uma senha nova na tela Manutenção e faça um backup novo imediatamente. |

**Reset completo (último recurso — apaga os dados deste computador):** só
depois de um backup bem-sucedido, feche o app e apague a pasta
`%LOCALAPPDATA%\CrivoDesktop`. Na próxima abertura o app monta um banco vazio;
restaure o backup pela tela Manutenção.

**O que mandar para o suporte da Crivo:** versão do app, o que estava fazendo,
a mensagem exata da tela e o arquivo de log em
`%LOCALAPPDATA%\com.crivonotarial.desktop\logs\`, pelo e-mail de suporte da
Crivo. O log não contém CPF, nome de pessoa qualificada nem conteúdo de
documento. Pode conter caminhos de pasta do Windows — e um caminho carrega o
nome da sua conta de usuário. Nunca mande o backup nem a senha de backup.

