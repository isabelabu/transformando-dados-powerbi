# PROCESSANDO E TRANSFORMANDO DADOS COM POWER BI
Descrição do desafio módulo 3 – Processamento de Dados Simplificado com Power BI
1. Criação de uma instância na Azure para MySQL
2. Criar o Banco de dados com base disponível no github
3. Integração do Power BI com MySQL no Azure
4. Verificar problemas na base a fim de realizar a transformação dos dados

## Problemas na integração e no banco de dados
### Obtenção dos dados
   Power BI não conectava ao MySQL, gerando uma mensagem de erro:
   
   ![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/3bf76f53-26a9-429d-80af-65ad9c57d4fc)

**Solução:** Instalação da versão 8.0.28 do MySQL, através do link: https://downloads.mysql.com/archives/c-net/

### Erro nos inserts
   Ao tentar inserir os dados nas tabelas, todas mostravam um erro que dizia "cannot add or update a child row"
   
   **Solução:** Inserir os dados antes de criar a foreign key fk_employee, já que a existência dela atrapalha na inserção dos dados relacionados a ela e, consequentemente, atrapalha a inserção de todos os dados
  
### Tabelas vazias
   Quando as tabelas eram puxadas para o Power BI, elas não mostravam os dados inseridos
   
   **Solução:** Esse é o problema com a solução mais simples, porque basta clicar no ícone no canto superior direito de cada tabela para atualizar os dados.

## Transformação dos dados
### Diretrizes para transformação dos dados
1. Verifique os cabeçalhos e tipos de dados
2. Modifique os valores monetários para o tipo double preciso
3. Verifique a existência dos nulos e analise a remoção
4. Os employees com nulos em Super_ssn podem ser os gerentes. Verifique se há algum colaborador sem gerente
5. Verifique se há algum departamento sem gerente
6. Se houver departamento sem gerente, suponha que você possui os dados e preencha as lacunas
7. Verifique o número de horas dos projetos
8. Separar colunas complexas
9. Mesclar consultas employee e departament para criar uma tabela employee com o nome dos departamentos associados aos colaboradores. A mescla terá como base a tabela employee. Fique atento, essa informação influencia no tipo de junção
10. Neste processo elimine as colunas desnecessárias.
11. Realize a junção dos colaboradores e respectivos nomes dos gerentes . Isso pode ser feito com consulta SQL ou pela mescla de tabelas com Power BI. Caso utilize SQL, especifique no README a query utilizada no processo.
12. Mescle as colunas de Nome e Sobrenome para ter apenas uma coluna definindo os nomes dos colaboradores
13. Mescle os nomes de departamentos e localização. Isso fará que cada combinação departamento-local seja único. Isso irá auxiliar na criação do modelo estrela em um módulo futuro.
14. Explique por que, neste caso supracitado, podemos apenas utilizar o mesclar e não o atribuir.
15. Agrupe os dados a fim de saber quantos colaboradores existem por gerente
16. Elimine as colunas desnecessárias, que não serão usadas no relatório, de cada tabela

### Tipos de dados e nomes das colunas
**Colunas renomeadas para padronização:**
- Coluna Dno (tabela employee), coluna Dnum (tabela project) renomeadas para Dnumber
- Coluna Pno (tabela works_on) renomeada para Pnumber
- Coluna Super_ssn (tabela employee) renomeada para Mgr_ssn

**Tipos de dados:**
- Coluna Salary (tabela employee) modificada de Número Decimal para Número decimal fixo
- Como as colunas de SSN não serão usadas para cálculos, elas serão mantidas na forma de texto

### Mesclagem e Divisão de Colunas
**Tabela employee:**
- Colunas Fname, Minit e Lname mescladas com separador espaço para a nova coluna Ename
- Coluna Address dividida por delimitador (-) em cada ocorrência
- Colunas que surgiram da divisão de Address renomeadas para Address_number, Street, City e State

### Mesclagem e Agrupamento de tabelas
**Tabela employee com tabela department:**
- Colaboradores e seus departamentos

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/15b55092-af27-48ac-961e-a134613f8013)

- Contagem de colaboradores por departamento

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/ed196d19-e1e9-4332-a63f-7dd6b79e25da)

**Tabela employee:**
- Colaboradores e seus gerentes

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/e3b24c91-2185-4704-a1b7-b54f0657f5fb)

- Contagem de colaboradores por gerente

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/2afd58d2-62f6-48fa-938b-cee06ee665c8)

**Tabela department com tabela dept_locations:**
- Mesclagem do nome do departamento com sua localização (criado em uma nova consulta)
- Não podemos usar o Atribuir nesse caso porque as tabelas não possuem a mesma estrutura

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/4b4830f0-9b7c-45ce-bbba-99bd2029daac)

**Tabela dependent:**
- Contagem de dependentes por colaborador

![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/5dffc91f-05d7-41c1-9f60-4b6898175db2)

### Nulos e Exclusão de colunas desnecessárias
- Exclusão das colunas relacionadas às chaves estrangeiras em todas as tabelas para evitar repetições desnecessárias

**Tabela Department:**
- Exclusão das colunas Mgr_start_date e Dept_create_date, já que elas não são necessárias para a análise

**Tabela Employee:**
- Para uma análise geral, não é necessário ter a data de nascimento, o número e a rua do endereço do colaborador, apenas a cidade e o estado. Por isso, as colunas Bdate, Address_number e Street foram excluídas

**Tabela Dependent:**
- Toda a tabela foi excluída, já que a contagem de dependentes por colaborador é suficiente para esse contexto de análise

### Análise dos dados
**Pontos observados:**
- Todos os departamentos possuem gerentes
- O colaborador que tem um gerente nulo é o gerente do departamento
- Observando a contagem de horas, é possível perceber que um dos colaboradores não está trabalhando (0h)

### Relatório
![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/017c35c4-ba82-4a0e-ba93-c246f72b276f)
