# Crivo Notarial Desktop — releases

Canal de distribuição do **Crivo Notarial Desktop** (SKU com banco de dados local
no cartório). Este repositório serve dois propósitos:

1. **Download do instalador** — pegue sempre o `CrivoDesktop-windows-x64-setup.exe`
   do release mais recente.
2. **Endpoint do atualizador automático** — o aplicativo consulta o `latest.json`
   deste repositório e verifica a assinatura antes de instalar qualquer coisa.

## Instalação

📘 **Guia passo a passo para o cartório (escrevente/Titular):** [GUIA-INSTALACAO.md](./GUIA-INSTALACAO.md) — download, aviso do SmartScreen, primeira abertura, login, backup, primeiro teste de certidão e restauração.

Baixe o `CrivoDesktop-windows-x64-setup.exe` e execute. A instalação é **por
usuário**, sem privilégio de administrador — os dados do cartório ficam em
`%LOCALAPPDATA%\CrivoDesktop` e nunca saem da máquina.

Enquanto o certificado de assinatura de código não é emitido, o SmartScreen do
Windows exibe um aviso: clique em **Mais informações** → **Executar assim mesmo**.

## Requisitos

- Windows 10/11 x64
- 4 GB de RAM e ~2 GB de disco livre
- Conexão com a internet para entrar na conta e emitir certidões

## Aviso

Os pacotes publicados aqui são assinados com a chave do canal desktop. **Não
instale binários do Crivo obtidos por qualquer outra origem.**

O Crivo Companion (automação de certidões para o produto web) é outro aplicativo,
distribuído em `crivo-companion-releases`.
