# 🚗 Urban Routes — Test Design & Test Cases (Carsharing)

Projeto de Engenharia de Qualidade de Software (QA) desenvolvido durante o **2º Sprint** da formação em Quality Assurance pela **TripleTen Brasil**. O foco deste repositório é demonstrar a aplicação estruturada de técnicas de **Design de Teste** para validar o fluxo de cadastro da **Carteira de Motorista** no recurso de compartilhamento de carros (*Carsharing*) da aplicação web **Urban Routes**.

---

## 🎯 Objetivo do Projeto

Garantir a cobertura completa dos requisitos funcionais e regras de negócio do formulário **"Adicionar carteira de motorista"**, prevenindo falhas de validação de entrada de dados antes e durante a execução dos testes manuais.

---

## 🛠️ Técnicas e Ferramentas Aplicadas

* **Análise de Requisitos & Decomposição Atômica:** Quebra de regras de negócio em blocos testáveis (comprimento de texto, conjuntos de caracteres válidos/inválidos e restrições de calendário).
* **Particionamento em Classes de Equivalência (EP):** Agrupamento de dados válidos e inválidos para maximizar a cobertura sem redundância.
* **Análise de Valores-Limite (BVA):** Mapeamento rigoroso de fronteiras ($LI - 1$, $LI$, $LI + 1$, $LS - 1$, $LS$, $LS + 1$), incluindo validações de meses de 30/31 dias e anos bissextos.
* **Otimização de Testes:** Eliminação de casos duplicados antes da execução.
* **Documentação & Gestão:** Google Sheets / Excel, Draw.io (Mapas Mentais e Fluxogramas).

---

## 🔍 Escopo Mapeado e Regras de Negócio

O design de testes abrangeu três campos críticos do formulário de habilitação:

### 1. Campos `Nome` e `Sobrenome`
* **Comprimento:** Intervalo fechado de **2 a 14 caracteres** ($LI = 2$, $LS = 14$).
* **Caracteres Válidos:** Apenas letras latinas e um hífen (`-`).
* **Caracteres Inválidos:** Espaços (início, meio ou fim), números, caracteres especiais e alfabetos não latinos.
* **Tratamento de Erro:** Moldura vermelha no campo e exibição da mensagem `"Insira um nome correto"` / `"Insira um sobrenome correto"`.

### 2. Campo `Data de nascimento`
* **Formato:** `dd.mm.aaaa` (apenas números; separadores inseridos automaticamente ao remover o foco).
* **Intervalo de Ano:** **1880 a 2006** inclusive ($LI = 1880$, $LS = 2006$).
* **Regras de Calendário:**
  * Meses com 31 dias ($01$ a $31$) e meses com 30 dias ($01$ a $30$).
  * Fevereiro em ano comum ($28$ dias) vs. Fevereiro em ano bissexto ($29$ dias — ex: `29.02.2004`).

---

## 📊 Amostra da Matriz de Classes de Equivalência e Limites

| Campo | Classe de Equivalência | Tipo | Limites | Dados de Teste | Classificação |
| :--- | :--- | :---: | :---: | :--- | :---: |
| **Nome** | De 2 a 14 caracteres | Intervalo | `2, 14` | `Paula` (meio) / `Pa` ($LI$) / `Paularrbizarro` ($LS$) | **Válida** |
| **Nome** | Menos de 2 caracteres | Intervalo | `1` | `P` ($LI - 1$) | **Inválida** |
| **Nome** | Maior que 14 caracteres | Intervalo | `15` | `Paularrbizarroo` ($LS + 1$) | **Inválida** |
| **Nome** | Um hífen | Conjunto | — | `Ana-Maria` | **Válida** |
| **Nome** | Espaço (início, meio, fim) | Conjunto | — | `" Paula"`, `"Pa ula"`, `"Paula "` | **Inválida** |
| **Nome** | Números e caracteres especiais | Conjunto | — | `12345`, `@#$%&` | **Inválida** |
| **Data de Nasc.** | Ano de 1880 a 2006 | Intervalo | `1880, 2006` | `01.01.1880` ($LI$) / `31.12.2006` ($LS$) | **Válida** |
| **Data de Nasc.** | Fevereiro (Ano Bissexto) | Intervalo | `01, 29` | `29.02.2004` ($LS$) / `30.02.2004` ($LS + 1$) | **Válida / Inválida** |

---

## 🧪 Exemplo de Casos de Teste Executados

**Pré-condição:** Aplicação Urban Routes aberta, rota definida de `"East 2nd Street, 601"` para `"1300 1st St"`, modo `"Personal"` e transporte `"Carsharing"` selecionados, com o modal `"Adicionar carteira de motorista"` visível.

| ID | Título do Caso de Teste | Etapas de Execução | Resultado Esperado |
| :--- | :--- | :--- | :--- |
| **t-7** | Validação negativa de Nome com espaço no início | 1. Inserir `" Paula"` no campo Nome.<br>2. Remover o foco do campo. | O campo é destacado em vermelho e exibe o erro: `"Insira um nome correto"`. |
| **t-15** | Validação negativa de Nome com campo vazio | 1. Deixar o campo Nome vazio, sem digitar nada.<br>2. Remover o foco do campo. | O campo é destacado em vermelho e exibe o erro: `"Insira um nome correto"`. |
| **t-16** | Validação positiva de Nome contendo hífen | 1. Inserir `"Ana-Maria"` no campo Nome.<br>2. Remover o foco do campo. | O sistema aceita o valor sem exibir mensagens de erro. |

---

## 📈 Resultados e Aprendizados

* **Cobertura Estruturada:** 100% dos requisitos do formulário mapeados em classes positivas e negativas antes do início da execução.
* **Rastreabilidade:** Padronização de IDs, pré-condições, passos atômicos e resultados esperados para facilitar a execução por qualquer membro da equipe de QA.
* **Próximos Passos:** Evolução deste conjunto de testes para validação de interface (UI/Figma) e automação End-to-End com **Python + PyTest + Selenium**.

---

## 👩‍💻 Autora

**Paula Bizarro** — Quality Assurance Analyst  
* **GitHub:** [github.com/paulabizarro](https://github.com/paulabizarro)
* **LinkedIn:** [linkedin.com/in/paula-bizarro](https://www.linkedin.com/in/paula-bizarro)
* **Contato:** paulabizarro@icloud.com
