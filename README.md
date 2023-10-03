# PROCESSANDO E TRANSFORMANDO DADOS COM POWER BI
Descrição do desafio módulo 3 – Processamento de Dados Simplificado com Power BI
1. Criação de uma instância na Azure para MySQL
2. Criar o Banco de dados com base disponível no github
3. Integração do Power BI com MySQL no Azure
4. Verificar problemas na base a fim de realizar a transformação dos dados

## Problemas na integração e no banco de dados
1. **Obtenção dos dados**
   <br>Power BI não conectava ao MySQL, gerando uma mensagem de erro:
   ![image](https://github.com/isabelabu/transformando-dados-powerbi/assets/113195403/3bf76f53-26a9-429d-80af-65ad9c57d4fc)
<br>**Solução:** Instalação da versão 8.0.28 do MySQL, através do link: https://downloads.mysql.com/archives/c-net/

2. **Erro nos inserts**
   <br>Ao tentar inserir os dados nas tabelas, todas mostravam um erro que dizia "cannot add or update a child row"
   <br>**Solução:** Inserir os dados antes de criar a foreign key fk_employee, já que a existência dela atrapalha na inserção dos dados relacionados a ela e, consequentemente, atrapalha a inserção de todos os dados
  
3. **Tabelas vazias**
   <br>Quando as tabelas eram puxadas para o Power BI, elas não mostravam os dados inseridos
   <br>**Solução:** Esse é o problema com a solução mais simples, porque basta clicar no ícone no canto superior direito de cada tabela para atualizar os dados.

## Transformação dos dados
