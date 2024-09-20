# gsi-rel

1. Acesse a máquina `oulu` via RDP
2. Entre no GSA no Windows (URL: [jurandy.net/gsa](http://jurandy.net/gsa))
3. No Windows, autenticar no site do GitHub: ([github.com/login](https://github.com/login/))
4. No Windows, acesse a URL para autenticação de dispositivos: [github.com/login/device](https://github.com/login/device)
5. Na Oulu, executar o comando: `gh auth login`
   
   1. Pressione a tecla <kdb>Enter</kbd> várias vezes até aparecer um código de 8 caracteres

7. Copiar ou digitar o código do passo 5, executado na Oulu, para a URL do passo 4, no Windows

## Criação de repositório no GitHub

Execute os seguintes comandos:

1. `gh auth status`
2. `gh repo create gsi-rel`

## Criação de repositório na Oulu

Execute os seguintes comandos:

1. `type -f take`
2. `type -f takedir`
3. `take /srv/tsi5/repo/NOME`[^1]
4. `git init .`
5. `touch README.md`
6. `git add .`
7. `git status`
8. `git commit -m "MENSAGEM"`[^2]
9. `git branch`[^3]
10. `git branch -m main`

## Sincronização entre repositório da Oulu e o do GitHub

1. `git remote -v`
12. `git remote add origin URL`[^4]
13. `git push -u origin main`

## Encerramento da sessão do GitHub CLI

1. `gh auth`
2. `gh auth status`
3. `gh auth logout`
4. `gh auth status`

[^1]: Substitua `NOME` por seu primeiro nome, com letras minúsculas e sem acento.
[^2]: Digite uma mensagem para registrar o que você fez.
[^3]: Verifique se a ramificação (*branch*) está com o nome `main`. 
[^4]: A URL da Home Page de seu repositório do GitHub. Geralmente terminal com `.git`.


