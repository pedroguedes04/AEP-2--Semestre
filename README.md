# PSE em Ação

Sistema de terminal, escrito em C, para **planejar e acompanhar ações de saúde nas escolas** do Programa Saúde na Escola (PSE).

Projeto da AEP do 2º semestre de 2026, disciplina de Linguagem C.

> O sistema usa apenas **dados fictícios e coletivos**. Ele não guarda nenhum dado individual de estudante.

---

## Funcionalidades

| Opção | Função | O que faz |
|:-----:|--------|-----------|
| 1 | Cadastrar ação | Registra uma nova ação com código, escola, tema, data prevista, público-alvo, responsável e quantidade prevista de participantes. Toda ação começa como **Planejada**. |
| 2 | Listar ações | Mostra todas as ações cadastradas e o total. |
| 3 | Atualizar situação | Muda a situação de uma ação para **Planejada**, **Realizada** ou **Cancelada**. Se for Realizada, pede a quantidade efetiva de participantes. |
| 4 | Gerar resumo | Mostra quantas ações há em cada situação, o total de participantes e o percentual de participação em relação ao previsto. |
| 5 | Pesquisar ação | Busca por **código**, **escola** ou **tema**. A busca por texto ignora maiúsculas e minúsculas, inclusive em letras acentuadas. |
| 6 | Sair | Encerra o programa. |

Antes do menu, o programa pede login.

| Usuário | Senha |
|---------|-------|
| `admin` | `1234` |

---

## Como compilar e executar

O programa usa só a biblioteca padrão do C, então não há nada para instalar além de um compilador.

### macOS / Linux

```bash
gcc -Wall -o pse_em_acao pse_em_acao_certo.c
./pse_em_acao
```

### Windows (MinGW / Code::Blocks / Dev-C++)

```bash
gcc -Wall -o pse_em_acao.exe pse_em_acao_certo.c
pse_em_acao.exe
```

No Windows, o programa ajusta o terminal sozinho para mostrar os acentos corretamente.

---

## Validações

O sistema confere os dados digitados e mostra uma mensagem de erro clara quando algo está errado. Depois do erro, ele volta ao menu sem travar.

- **Código:** precisa ser um número inteiro maior que zero e não pode repetir o código de outra ação.
- **Campos de texto:** escola, tema, público-alvo e responsável são obrigatórios. Textos com mais de 49 caracteres são cortados sem quebrar letras acentuadas.
- **Data prevista:** precisa estar no formato `dd/mm/aaaa` e existir no calendário, considerando meses de 30 e 31 dias e anos bissextos. Por exemplo, `31/04/2026` e `29/02/2027` são recusadas.
- **Quantidades:** precisam ser números inteiros iguais ou maiores que zero.
- **Entradas numéricas:** valores como `12abc`, campo vazio, número negativo ou número grande demais são recusados.
- **Limite:** cabem até **20 ações**.
- **Fim da entrada:** se a entrada acabar (Ctrl+D ou Ctrl+Z), o programa fecha normalmente em vez de entrar em loop.

---

## Exemplo de uso

```
=== PSE EM AÇÃO ===

Usuário: admin
Senha: 1234

Login realizado com sucesso!

===== MENU PRINCIPAL =====
1 - Cadastrar ação
2 - Listar ações
3 - Atualizar situação
4 - Gerar resumo
5 - Pesquisar ação
6 - Sair
Escolha uma opção: 1

=== CADASTRAR AÇÃO ===
Código (número maior que zero): 10
Escola: Escola Municipal Centro
Tema: Saúde bucal
Data prevista (dd/mm/aaaa): 15/03/2026
Público-alvo: Alunos do 5º ano
Responsável: Ana
Quantidade prevista de participantes: 30
Ação cadastrada com sucesso! Situação inicial: Planejada.
```

Se essa ação for marcada como **Realizada** com 25 participantes, o resumo mostra:

```
=== RESUMO GERAL ===
Total de ações cadastradas: 1
Ações planejadas: 0
Ações realizadas: 1
Ações canceladas: 0
Total de participantes (ações realizadas): 25
Participantes previstos (ações realizadas): 30
Percentual de participação em relação ao previsto: 83.3%
```

---

## Como o código está organizado

Os dados ficam em **vetores e matrizes paralelos**: a posição `i` de cada vetor corresponde à ação `i`. A variável `total` guarda quantas ações já foram cadastradas.

| Parte | Funções |
|-------|---------|
| Leitura de dados | `lerTexto`, `lerInteiro`, `ajustarCorteUtf8` |
| Auxiliares | `minuscula`, `minusculaAcentuada`, `ehDigito`, `dataValida`, `textosIguais`, `buscarCodigo`, `mostrarSituacao`, `mostrarAcao` |
| Login e menu | `telaLogin`, `mostrarMenu` |
| Funcionalidades | `cadastrarAcao`, `listarAcoes`, `atualizarSituacao`, `gerarResumo`, `pesquisarAcao` |
| Programa principal | `main` |

Constantes principais:

| Constante | Valor | Significado |
|-----------|:-----:|-------------|
| `MAX_ACOES` | 20 | Quantidade máxima de ações |
| `TAM_TEXTO` | 50 | Tamanho máximo dos textos (49 caracteres + `\0`) |
| `TAM_DATA` | 11 | `dd/mm/aaaa` + `\0` |
| `PLANEJADA` / `REALIZADA` / `CANCELADA` | 1 / 2 / 3 | Situações possíveis de uma ação |

---

## Limitações

- Os dados ficam **apenas na memória**. Quando o programa fecha, tudo é perdido, porque não há gravação em arquivo.
- O login é fixo e serve só para demonstração.
- A pesquisa por escola ou tema exige o **nome completo**. Uma busca parcial, como `Centro`, não encontra `Escola Municipal Centro`.
- No Windows, letras acentuadas **digitadas pelo usuário** podem aparecer com caracteres estranhos, dependendo da configuração do terminal. As mensagens do próprio programa aparecem corretamente.
