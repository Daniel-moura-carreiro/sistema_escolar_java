# Sistema Escolar — Spring Boot

## Aluno
**Nome completo:** Daniel Moura Carreiro

---

## Descrição do sistema

Sistema de cadastro escolar desenvolvido com Spring Boot que permite gerenciar professores, alunos e matrículas.  
Utiliza banco de dados em memória H2, JPA/Hibernate para mapeamento objeto-relacional e expõe uma API REST com endpoints para criar, listar, atualizar e excluir cada entidade.

---

## Os 8 erros encontrados e corrigidos

### Erro 1 — `pom.xml`, linha 8 — Versão inexistente do Spring Boot
**Erro:** `<version>4.0.5</version>`  
**Por que causa falha:** A versão 4.0.5 do spring-boot-starter-parent não existe no Maven Central. O projeto não conseguia ser resolvido e nem compilava.  
**Correção:** Alterado para `<version>3.4.5</version>`

---

### Erro 2 — `pom.xml`, linha 60 — Artefato web inexistente
**Erro:** `<artifactId>spring-boot-starter-webmvc</artifactId>`  
**Por que causa falha:** O artefato spring-boot-starter-webmvc não existe no Maven. Sem ele, o projeto ficava sem a camada web e nenhum controller funcionava.  
**Correção:** Alterado para `<artifactId>spring-boot-starter-web</artifactId>`

---

### Erro 3 — `Aluno.java`, linha 14 — Nome de tabela errado
**Erro:** `@Table(name = "professores")`  
**Por que causa falha:** A entidade Aluno apontava para a tabela professores. Os dados dos alunos eram gravados na mesma tabela dos professores, causando conflito de colunas e erros no banco H2.  
**Correção:** Alterado para `@Table(name = "alunos")`

---

### Erro 4 — `DadosListagemAluno.java`, linhas 14–15 — Campos nome e email invertidos
**Erro:** `aluno.getEmail()` era passado como nome e `aluno.getNome()` como email  
**Por que causa falha:** O JSON retornado por GET /alunos/listar trazia o e-mail no campo nome e o nome no campo email, retornando dados incorretos ao cliente.  
**Correção:** Trocado para a ordem correta: `aluno.getNome()` e depois `aluno.getEmail()`

---

### Erro 5 — `AlunoController.java`, linha 47 — `@PostMapping` duplicado
**Erro:** O método `atualizar()` estava anotado com `@PostMapping` igual ao método `cadastrar()`  
**Por que causa falha:** Dois métodos com @PostMapping na mesma URL /alunos causam conflito de rota. O Spring não conseguia distinguir qual método chamar e lançava erro na inicialização.  
**Correção:** Alterado para `@PutMapping`

---

### Erro 6 — `Professor.java`, linha 47 — Campo errado recebendo o e-mail
**Erro:** `this.nome = dados.email()`  
**Por que causa falha:** Ao atualizar um professor, o valor do e-mail era gravado no campo nome, sobrescrevendo o nome e nunca atualizando o e-mail corretamente.  
**Correção:** Alterado para `this.email = dados.email()`

---

### Erro 7 — `MatriculaController.java`, linha 50 — Nome do `@PathVariable` errado
**Erro:** `@PathVariable Integer ids` (parâmetro chamado `ids`)  
**Por que causa falha:** A URL do endpoint é DELETE /matriculas/{id}, mas o parâmetro Java se chamava `ids`. O Spring não conseguia fazer o bind da variável de rota e lançava exceção ao tentar deletar.  
**Correção:** Renomeado para `@PathVariable Integer id`

---

### Erro 8 — `AlunoRepository.java`, linha 6 — Tipo do ID errado
**Erro:** `JpaRepository<Aluno, String>`  
**Por que causa falha:** O ID da entidade Aluno é Integer, mas o repositório declarava String. Isso causava erros de tipo em todas as operações com ID e impedia a compilação correta. O MatriculaController também precisou ser corrigido removendo o `.toString()` desnecessário.  
**Correção:** Alterado para `JpaRepository<Aluno, Integer>`

---

## Como executar o projeto

```bash
# 1. Clone o repositório
git clone https://github.com/Daniel-moura-carreiro/sistema_escolar_java.git
cd sistema_escolar_java

# 2. Execute a aplicação (Windows)
$env:JAVA_HOME = "C:\Program Files\Eclipse Adoptium\jdk-21.0.11.10-hotspot"
.\mvnw.cmd spring-boot:run

# 3. Acesse o console H2
# http://localhost:8080/h2-console
# JDBC URL: jdbc:h2:mem:clinicadb
```

---

## Exemplos de requisições

### Cadastrar Professor