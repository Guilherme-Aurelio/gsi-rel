# Explicação de seu DN do LDAP no IFRN

Pontuação: [30 pontos][^1]

!!! warning

    Esta tarefa valerá 40 pontos se escrita à mão em folha pautada e entregue como um único PDF, conforme as instruções da seção sobre digitalização.

Explique o DN[^1] de seu usuário na árvore DIT do LDAP. 

Especifique:

- DN
- RDN
- DC
- OU

## Digitalização de documentos para um arquivo PDF

<iframe width="560" height="315" src="https://www.youtube.com/embed/JGlD6FMIaAw?si=jUj796GwskXyjMbE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

[^1]: Em uma máquina Window do IFRN, execute, no CMD, o comando `whoami /fqdn`.

---
### A Distinguished Name (DN) é o caminho completo de um objeto dentro do diretório LDAP. Ele representa a localização hierárquica exata de um objeto no diretório.

### DN do IFRN:

```bash
CN=20221148060019,OU=Alunos,OU=DG-PAR,OU=RE,OU=IFRN,DC=ifrn,DC=local
```

## 1. DN (Distinguished Name)

### DN é a descrição completa da hierarquia do objeto dentro do diretório LDAP. No exemplo, o DN completo é:

```bash
CN=20221148060019,OU=Alunos,OU=DG-PAR,OU=RE,OU=IFRN,DC=ifrn,DC=local
```

### Esse DN identifica o usuário com o valor CN=20221148060019 dentro da hierarquia do LDAP.


## 2. RDN (Relative Distinguished Name)

### RDN é a parte específica do objeto no contexto imediato. Neste caso, o RDN é:

```bash
CN=20221148060019
```

### Aqui, CN (Common Name) representa o nome comum do objeto, e o valor 20221148060019 é o identificador único deste objeto (usuário).


## 3. DC (Domain Component)

### DC identifica os componentes de domínio da organização. No exemplo, temos dois DCs:

```bash
DC=ifrn,DC=local
```

### Isso indica que o objeto está no domínio ifrn.local, que provavelmente é o domínio de rede da instituição IFRN (Instituto Federal do Rio Grande do Norte).


## 4. OU (Organizational Unit)

### OU representa as Unidades Organizacionais dentro da hierarquia LDAP. No exemplo, temos várias OUs:

```bash
OU=Alunos
OU=DG-PAR
OU=RE
OU=IFRN
```

### Essas OUs indicam que o objeto está dentro de várias subunidades organizacionais, hierarquicamente dispostas da seguinte forma:

```bash
OU=Alunos: Unidade organizacional de estudantes.

OU=DG-PAR: Provavelmente uma subunidade relacionada ao departamento DG-PAR.

OU=RE: Outra unidade organizacional, talvez relacionada à reitoria.

OU=IFRN: Unidade principal que representa a instituição IFRN.
```