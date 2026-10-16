# HashTabela

Ferramenta gratuita, local e offline para calcular o **SHA-256** de vários arquivos e gerar uma tabela pronta para colar em documentos.

## Como usar
1. Baixe `index.html` e abra com dois cliques (funciona via `file://`, sem internet).
2. Clique em **Escolher arquivos**, **Escolher pasta** ou arraste arquivos/pastas para a área indicada.
3. (Opcional) Preencha o cabeçalho: operador, referência, dispositivo de origem.
4. (Opcional) Marque **Incluir data de modificação**.
5. Use **Copiar tabela** (cola formatada no Word, Google Docs e LibreOffice), **Exportar CSV** ou **Exportar HTML**.

## Manifesto (.sha256)
**Exportar manifesto** gera um arquivo no formato padrão do `sha256sum` (`<hash>  <caminho relativo>`, uma linha por arquivo, LF, UTF-8). Os metadados (ferramenta e versão, data e hora da geração, operador, referência, dispositivo de origem) ficam em linhas de comentário `#` no topo, que o `sha256sum -c` do GNU coreutils ignora (testado na versão 9.7, inclusive com `--strict`).

Quando você seleciona uma pasta, os caminhos ficam relativos a ela. Para conferir:

```
cd pasta-de-origem
sha256sum -c manifesto-AAAA-MM-DD_HH-MM.sha256
```

Depois de exportar, a tela mostra o **SHA-256 do próprio manifesto**, que pode ir para o relatório e comprova que o manifesto não foi alterado.

## Verificar manifesto
Na seção **Verificar manifesto**, carregue o `.sha256` e depois os arquivos ou a pasta. Para cada item a ferramenta mostra **OK**, **Diverge** (o hash é diferente) ou **Falta** (está no manifesto, mas não foi fornecido), e lista como **Extra** o que foi fornecido mas não consta do manifesto. Também lê manifestos gerados pelo próprio `sha256sum`.

## Privacidade
- Nenhum arquivo sai do seu computador: não há upload, conta, telemetria, CDN nem requisições de rede.
- Uma Content-Security-Policy no próprio arquivo bloqueia qualquer conexão externa.

## Como o hash é calculado
- Até 256 MB por arquivo: Web Crypto API do navegador (`crypto.subtle.digest`).
- Acima disso: leitura em blocos de 8 MB com SHA-256 incremental em JavaScript, com barra de progresso e sem carregar o arquivo inteiro na memória (mais lento).
- Confira com `sha256sum arquivo` (Linux), `shasum -a 256 arquivo` (macOS) ou `Get-FileHash arquivo` (Windows).

## Observações
- A data de modificação vem do sistema de arquivos e pode ter sido alterada por cópia/download; não é a data de criação do conteúdo.
- O CSV usa `;` como separador e BOM UTF-8 (abre direto no Excel/LibreOffice em PT-BR).
- Linhas `#`: o `shasum -a 256 -c` do macOS (Perl) pode avisar sobre linhas mal formatadas, mas confere os hashes. No macOS, prefira `gsha256sum` (coreutils) ou o modo Verificar da própria página.
- Testado no Chrome. Seleção de pasta e arrastar pastas dependem de suporte do navegador (Chrome, Edge, Firefox e Safari recentes).
- Hash comprova integridade (o arquivo não mudou), não autenticidade.

## Licença
A definir.
