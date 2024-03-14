# Cognitive-Search-in-Azure-ML
Azure Cognitive Search ou Azure AI Search (a ferramenta/recurso principal que será utilizado nesse repositório), é um serviço que oferece recuperação de informações segura em escala sobre conteúdo de propriedade do usuário em aplicativos de busca tradicionais e conversacionais que tem funcionalidades como motor de busca, síntaxe de consultas avançadas, formatos de documentos em JSON, integração direto com outros serviços da plataforma e entre outros
Vamos agora para a parte prática:

## Construindo o ambiente para desenvolvimento

Para o desenvolvimento desse laboratório, necessitamos criar três recursos em específicos além do AI Search: o Azure AI Services e o Storage Account

### AI Search

1. Para criá-lo, iremos clicar em criar um novo recurso, ir nas categorias do AI + Machine Learning e pesquisar por lá "AI Search". Encontrando o recurso da imagem abaixo é só clicar nela para iniciar a criação
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/5512ab44-66a1-42a9-9836-7f2a01fa6a38)

![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/6738c9db-6e7a-45ad-b841-e8009f143053)

2. Para criar um serviço de pesquisa (search service), precisaremos escolher seu grupo de recurso, o nome e o local do servidor e escolher o tier de preço para Básico ou Basic (clicando em Change Princing Tier e selecionando a opção como está na 2ª imagem)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/3004caea-98c2-4451-a188-51a3e20ecabf)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/5102f7b0-1c8a-491c-97c0-6fa3be7c610a)
Aviso: por algum motivo, a opção padrão de local "West US 2" não funcionou para mim no momento que se entrelaça esse recurso com o AI Services, então tive que usar o East US

### Azure AI Services

1. Indo para o 2º recurso, faremos o mesmo processo de entrar na categoria do AI + Machine Learning e clicar em criar ou create no Azure AI Services
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/3574d3ca-2864-4536-83fd-0b7f6d13d865)

2. Criando o recurso, deve se fazer o mesmo processo que nem o anterior basicamente, acrescentando checar a caixa de seleção na 2ª imagem e o riando 
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/3c0142e2-7d2e-4a35-b653-9e87787921a4)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/2169d366-73f5-473e-9afa-a49addf2c265)
Nota: É primordial que a localização do Azure AI Services seja a mesma da Search, caso contrário mais para frente na hora que você for entrelaçar as duas não irá aparecer na seleção


### Storage Account
1. O último recurso agora é muito necessário, pois será onde que será armazenado os dados que o AI Search utilizará para a pesquisa. Para acessá-lo, vá para a tela inicial do portal e na barra de pesquisas somente escreva "Storage" e automaticamente ela aparecerá
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/c2f954dc-64e8-4e17-84df-bef66b740cf7)

2. Você verá que não existirá nenhum conta de armazenamento. Para criar, somente clicar no botão create storage account
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/b06a089e-752c-4688-a560-de2d35291f81)

3. Fazendo a questão padrão de desenvolver recursos, os diferenciais neste é somente a perfomance ou redundância/redundancy. Para a primeira, você deve escolher a padrão/stardant e para a segunda a LRS (Locally-Reduntant Storage). Essas duas opções podem salgar muito o custo do armazenamento, por isso foram selecionadas dessa forma assim como o "Basic" do AI Search
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/43068371-85b5-45c1-8c21-40bd38ad3be8)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/44706668-2fc3-4b7f-88e3-aeb5dfbb0247)
4. Criando ele, clique em go to resource ou ir para o recurso
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/e030c15e-f0cd-4b60-8455-34ce8de060c5)
5. Na aba settings, vá para configurations/configrações. Você perceberá que o "Allow Blob anonymus access" estará desativado. Ative-o, pois é algo necessário nessa lab para passar o dados ao storage account 
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/fe36530b-af5c-41fa-aeae-1f6ab8cdf212)
6. Na aba settings, vá para configurations/configrações. Você perceberá que o "Allow Blob anonymus access" estará desativado. Ative-o, pois é algo necessário nessa lab para passar o dados ao storage account 
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/958e46fe-5cea-48cf-a095-8f7ddfe7e56b)

7. Entrando no Container criado, clique em upload e passe os arquivos que estão dentro desse link aqui: https://aka.ms/mslearn-coffee-reviews 
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/7e06f30d-3cc1-4ef8-8984-622cc9a04468)
Nota: para não ficar espalhado no diretório que será extraído o arquivo no armazenamento de seu dispositivo, crie uma nova pasta, passe o arquvio compactado na mesma, descompacte-o nela e puxe os 9 arquivos para o local especificado

## Desenvolvendo o AI Search
1. Concluindo os requisitos anteriores, vá para o recurso seu criado do AI Search e clique em Import Data e selecione o "Azure Blob Storage"
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/b89dee7a-6a0a-4772-a1bc-26ecaa403426)
2. Selecionando-o, você verá a imagem abaixo. Crie essa importação de data com está lá, e ao selecionar o Connection String, clique em Choose an existing Storage
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/810108d1-ea85-4e55-b209-a27e9ef4cfbe)
3. Entrando lá, passe para o seu container criado até onde está armazenado os 9 arquivos criados anteriormente e selecione
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/e58f331c-83cb-4858-afb0-48a62bf6da20)
4. Adicione os outros dados como está abaixo e vá para o Next
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/1c6e8e07-fdbd-40df-b6c8-a880bdba5f6a)
5. Indo ao subtópico Attach AI Services, você deve selecionar o recurso do AI Services que você criou de acordo com as notas e avisos que dei anteriormente
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/564effd1-6437-4b74-bb2b-ad93f1417759)
6. Copie os dados como estiver abaixo
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/e5e0506f-f10f-471e-9b7b-0960414d9097)

![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/18437d35-7e2e-4751-8c6e-48a3f5a60ab4)

7. No próximo subtópico, irá aparecer um erro no Storage account connection string. Para resolver, faça esse passo a passo (clique no link azul abaixo desse texto)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/4d215a09-c712-4014-ad1d-538b1225bb16)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/b309135b-2bbe-482c-b516-de03d0a4ede6)

8. Caso fique em dúvida no passo 7, essa imagem demonstra bem como deve ficar mais ou menos essa seção (Se o Search Mode estiver diferente, não tem problema) e passe para o próximo
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/83119f89-7eaf-4c90-9821-85413ef182af)

9. Selecione como estiver abaixo e finalize clicando no Next
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/ae07259f-0b9e-43cb-bf8b-c24b012f7ba2)
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/617f6bbf-ccf0-4570-b293-dbcf0962215b)

10. Por último, faça como está abaixo e clique em submit

![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/50a525cf-e8d2-4442-9639-3d30ddb66d19)

11. Caso deseja ver o resultado, volta para tela inicial do seu AI Search e procure pelo Search Explorer e teste a pesquisa pela lupa com esse texto: <code> search=locations:'Chicago' </code> e cliqe em Submit/enviar
![image](https://github.com/GustavoPereira-Dev/Cognitive-Search-in-Azure-ML/assets/108029506/9dca0d24-b52e-440a-85f4-891533ceb7ca)

Retornando algo um pouco semelhante com a imagem acima  o serviço está concluído! Agora só implementar ele em um sistema.

## Conclusão

Como é possível ver, esse laboratório é bem complicado de desenvolver, pois existem N Fatores a se prestar atenção, especialmente por utilizar não um recursos, mas três que no final irão se conectar em só um. Então recomendo você fazer com muita calma isso, pois você pode acabar cometendo erros graves como o que aconteceu comigo. Além disso, caso as imagens não fiquem muito clara, utilizem esse site aqui também como um apoio https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html, ele me ajudou muito a solucionar os problemas que haviam ocorrido no desenvolvimento disto.

Finalizando, também é interessante perceber que esta funcionalidade do recurso das pesquisas podem ser utilizadas em muitas outras ferramentas, como em portais de conteúdo e documentação, intranets corporativas, aplicativos de comércio eletrônico e varejo, chatbots de apoio a cliente, filtros inteligentes, enfim as possibilidades são amplas e esses são só alguns pequenos exemplos. Dessa forma eu termino este README. Até o próximo README!
