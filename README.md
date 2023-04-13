# Criação de MFE baseado apenas no Framework Angular

**Foi utilizado o seguinte guia como referência**
https://medium.com/@gurunadhpukkalla/implementing-microfrontends-in-angular-15-35c4d2307e36

Para esse exemplo iremos utilizar a versão mais recente do Angular(v15) da data de criação desse guia(10/04/2023).

Precisaremos seguir alguns passos para que seja possível a criação dessa aplicação.

## Instalação

Para a criação do projeto, precisaremos da instalação de duas ferramentas primordiais, o NodeJS(NPM) e o Angular.io

- Instalar o NodeJS
    - https://nodejs.org/en
    - É recomendável sempre utilizar a versão LTS, no dia da criação desse guia, é a versão 18.15.0

- Instalar o Angular
    - Após a instalação do NodeJS o NPM também será instalado.
    - Deverá ser executado o seguinte comando no prompt de comando executado de forma admin
    - npm install -g @angular/cli

- Após essas etapas feitas, poderemos seguir com a criação do projeto.

## Criação do Projeto

Para efetuarmos a criação do projeto, iremos primeiramente criar um Workspace que comportará toda a aplicação(host) e seus micro-frontends(remotes)

- Para criarmos o Workspace, iremos utilizar o seguinte comando
    - ng new nome-do-workspace --create-application="false"
    - Após a execução do comando, deveremos ver a seguinte imagem:
    
    ![image](https://user-images.githubusercontent.com/60113791/230966008-20f3b4c1-f470-4fb8-a672-fb90448db627.png)
    
- Após a criação do Workspace, precisaremos criar as aplicações, tanto a aplicação host quanto as aplicações remotas.
    - O primeiro passo é entrarmos no Workspace para que seja possível executar os comandos das aplicações
        - Para isso, temos algumas formas, mas se estivermos usando o cmd ou equivalente, podemos rodar o comando "cd nome-do-workspace" para entrarmos nele
    - Todos os passos abaixo, serão repetidos para o host e as outras aplicações
        - Para efetuar essas criações, para cada aplicação iremos rodar o seguinte comando
            - ng g application nome-da-aplicacao
                - A primeira coisa que iremos ver será o envio de informações anônimas para o google, fica a seu critério/do projeto o envio dessas informações
                - Logo abaixo, é questionado se desejamos acrescentar o Angular Routing, informaremos que sim escrevendo y

                ![image](https://user-images.githubusercontent.com/60113791/230967340-28b4df82-0915-4bc6-ac51-3f8dce1c80aa.png)

                - Após isso, iremos informar qual formato de CSS iremos usar, isso varia de projeto para projeto. Escolha o que mais fizer sentido para a sua situação

                ![image](https://user-images.githubusercontent.com/60113791/230967974-eb968cf1-d685-4c72-9315-05278286b19c.png)

                - Após escolhermos qual formato de CSS será utilizado, será iniciada a criação da aplicação e na conclusão deveremos ver algo parecido com isso

                ![image](https://user-images.githubusercontent.com/60113791/230971542-94452a21-54f0-4c8a-8510-b304ee659af2.png)

                - Após a aplicação ser iniciada, iremos adicionar o Module Federation Package.
                    - Utilizaremos o seguinte comando:
                        - ng add @angular-architects/module-federation --project nome-da-aplicacao --port porta-da-aplicacao
                    - Na primeira execução, veremos uma imagem similar a essa

                    ![image](https://user-images.githubusercontent.com/60113791/230972287-7a02d4e8-43d5-4062-898d-dd82b75fdbd6.png)

                    - Na primeira execução precisaremos instalar para o Workspace o Module Federation Package para que seja usado nos projetos, nas execuções seguintes ele apenas irá alterar configurações sem a necessidade da instalação.
                    - Após informamos que desejamos instalar, respondendo _**Y**_ no questionamento da imagem acima, iremos ver o que foi alterado/criado no nosso Workspace conforme a imagem abaixo

                    ![image](https://user-images.githubusercontent.com/60113791/230972853-8771f182-116a-49b3-bfae-a77120d6fabf.png)
                    
                    - Após instalarmos o Module Federation Package iremos criar os mfes e repetir os mesmos passos de criar a application e adicionar o Module Federation Package. 
                    - **_Lembrando que temos que especificar portas diferentes para cada aplicação_**
                    - Após instalarmos todos os Module Federation Packages e criarmos o host e ao menos um mfe, precisaremos criar a navegação entre eles, para isso iremos criar um módulo e um componente dentro do host para servir apenas como carregador inicial, vamos chamá-lo de AdminModule.
                        - Para isso, dentro da pasta host/src/app iremos criar uma pasta chamada modules, dentro da pasta module, iremos executar o seguinte comando
                            - **ng g m admin && ng g c admin**
                            - Após isso, iremos ver algo assim no nosso cmd

                            ![image](https://user-images.githubusercontent.com/60113791/231783805-4fc889e9-fd24-4f2d-8a9c-2a6e77fbc35e.png)
                            
                            - Após a criação, vamos tornar o módulo acessível, precisaremos modificar dois arquivos, um dentro do módulo Admin e outro na pasta app do host
                            - No módulo Admin iremos modificar o admin.module.ts para deixarmos dessa forma

                            ![image](https://user-images.githubusercontent.com/60113791/231785010-e020a8c1-3a54-408d-8a54-62fe1e930278.png)

                            - Após isso, iremos modificar o nosso app-routing.module.ts dentro da pasta app do nosso host, para criarmos a navegação até o módulo. Para isso iremos deixar o arquivo da seguinte forma

                            ![image](https://user-images.githubusercontent.com/60113791/231785628-ccb219de-b2fb-43b1-b3bb-d18a9a5b3f0b.png)

                            - Com isso, temos um inicializador do nosso shell, vamos testar se tudo funcionou corretamente, para isso vamos voltar para a pasta raiz do workspace e executar o comando **ng serve host**
                            
                            ![image](https://user-images.githubusercontent.com/60113791/231786286-2431d4c6-0b0c-4b8b-a2f4-866f2c15bbd9.png)
                            
                            - Caso você veja essa tela com o "admin works!" lá no final, o host está criado corretamente.
                            - Mas isso ainda não é um mfe, isso foi apenas a criação de uma aplicação angular com lazy routing. Agora vamos criar a navegação entre os mfes
                            - Para isso precisaremos ir para a pasta de algum mfe criado, nesse exemplo eu criei um mfe chamado **mfe1**, seguindo o guia feito pouco acima. Criando a application e adicionando o Module Federation Package.
                            - A primeira coisa que iremos fazer é o mesmo passo que fizemos para o host, criar um módulo para que ele se torne acessível pelo host, para isso iremos repetir os passos acima dentro da pasta do mfe1.
                            - Ir dentro do host/src/app, criar uma pasta modules, após isso executar o comando **ng g m home && ng g c home**, modificar o arquivo de rota dentro da pasta home e o arquivo de rosta dentro da pasta app, eles ficarão assim
                            - Home

                            ![image](https://user-images.githubusercontent.com/60113791/231788087-6381ffb9-5e9f-43da-af52-5f5204b963f2.png)

                            - App
                            
                            ![image](https://user-images.githubusercontent.com/60113791/231788307-d90d74fa-26ac-42d8-a866-e78c1616b9a2.png)
                            
                            - Com isso também teremos um angular app com lazy routing no mfe1, vamos testar se está funcionando, para isso iremos executar o comando **ng serve mfe1**
    
                            ![image](https://user-images.githubusercontent.com/60113791/231788796-4dbcd8bb-0b13-4d7e-92fb-c950f7b0b016.png)
                            
                            - Caso você veja algo desse tipo, escrito "home works!" lá embaixo, significa que funcionou corretamente, mas perceba que eu utilizei uma porta diferente aqui, ao invés da porta padrão do angular (4200) foi utilizada a porta 4201, pois será necessário que os apps rodem em paralelo para que funcione corretamente a leitura do app host, pois ele serve apenas como uma casca.
                            - Com isso, iremos passar para a configuração do Webpack que é o que torna esses caras comunicáveis entre si, para isso, iremos iniciar com o host, precisaremos buscar o arquivo chamado **webpack.config.js**

                            ![image](https://user-images.githubusercontent.com/60113791/231790940-7c13dbb3-70d3-4275-b69b-3dfce6858a1c.png)

                            - Ele foi criado lá no começo quando adicionamos o Module Federation Package dentro de cada projeto, inicialmente ele vai estar assim:
                            
                            ![image](https://user-images.githubusercontent.com/60113791/231791382-af0b8752-c1ae-471c-a0aa-5cb7d2c4b646.png)

                            - Inicialmente a única parte que precisaremos alterar é a parte de **plugins**, pois o resto já está funcionando corretamente. Porém essa alteração será diferente dependendo se estamos alterando o host ou algum mfe, para demonstrar iremos modificar o host

                            ![image](https://user-images.githubusercontent.com/60113791/231792647-833bef04-75ce-4fb7-8413-d116733d3615.png)
                            
                            - Alterei dentro do host para que ele possa ler o mfe1, para isso coloquei a porta 4201, a que vai ser utilizada quando o mfe1 estiver rodando.
                            - Agora precisamos alterar o mfe1 para que ele exponha o arquivo remoteEntry.js, para isso iremos no arquivo **webpack.config.js** dentro da pasta do projeto mfe1, para isso iremos exibir o módulo Home que criamos anteriormente.

                            ![image](https://user-images.githubusercontent.com/60113791/231793752-474f832b-11be-4f0b-9057-553f53396cec.png)
                            
                            - Com isso feito, precisaremos voltar para o projeto **host** e criar a navegação para o **mfe1**
                            - Para isso iremos modificar o arquivo de rotas do app-routing.module.ts da nossa aplicação host, ele irá ficar da seguinte forma

                            ![image](https://user-images.githubusercontent.com/60113791/231794646-80aab298-6133-4935-8fdc-fca171307311.png)
                            
                            - Perceba que o mfe1/Mfe1Module está grifado de vermelho, já iremos corrigir, mas antes gostaria de explicar como essa url vai conseguir ler o módulo do mfe1
                            - Dentro do webpack.config.js da aplicação host, eu informei que a chave para ler o mfe1 seria **mfe1** e dentro do webpack.config.js do mfe1 eu informei que para ler o módulo HomeModule desse mfe seria ./Mfe1Module, por isso essa url ficou dessa forma
                            - Para corrigirmos o erro precisamos criar um arquivo dentro da pasta src/app do projeto host. esse arquivo será chamado **decl.d.ts** e terá o seguinte conteúdo

                            ![image](https://user-images.githubusercontent.com/60113791/231802580-4388221c-569e-41b8-91b6-537e521052ce.png)

                            - Com isso o erro no arquivo app-routing.module.ts no projeto host será resolvido.
                            - Agora iremos testar se a navegação entre mfes está funcionando corretamente, para isso iremos retornar para a pasta do workspace e rodaremos o seguinte comando
                            - npm run run:all
                            - Ao acessarmos a url informada no nosso host, deveremos ver a seguinte tela

                            ![image](https://user-images.githubusercontent.com/60113791/231804014-28f64959-cfcf-427b-bb40-bd8893594aa7.png)
                            
                            - Caso esse seja o caso, a aplicação mfe foi criada com sucesso, a partir de agora, para os próximos mfes, será necessário seguir os passos de criação de aplicação e navegação para os novos mfes.
