---
layout: page
title: Proposta
published: false
---
## Problema
O roubo de celulares é um problema frequente nos dias atuais. Quando o usuário têm medo de atender na rua o seu celular, este deixa de ser móvel e passa a ser portátil, utilizado somente em ilhas onde a pessoa se sente segura, como sua casa, seu trabalho, dentro de estabelecimentos comerciais etc. Portanto, a falta de segurança pública afeta diretamente o propósito central do serviço de telefonia móvel e toda a sua infraestrutura [1]. 

De acordo com a Secretaria da Segurança Pública de São Paulo, cerca de 25% dos crimes de roubo e furto no estado de São Paulo têm como alvo o aparelho de celular [2]. Os alvos preferidos são as pessoas que andam distraídos pela rua, falando ao celular ou com o aparelho no bolso de trás [3]. As vítimas são orientadas a informar o IMEI (Identidade Internacional de Equipamentos Móveis) do aparelho no boletim de ocorrência, para que as operadoras bloqueiem os aparelhos [4]. Porém, investigações sobre destinos dos celulares realizado pelo Departamento de Operações Policiais Estratégicas (Dope) apuraram que grande parte dos aparelhos seriam levados para Dacar, em Senegal, na África [5], onde não têm o controle de IMEI [6]. Portanto, muitos dispositivos não retornam às vítimas.

Predição de crimes não é uma tarefa fácil. Crimes não são aleatórios, mas também não ocorrem consistentemente no espaço ou no tempo. Informações das proporções roubo de cada tipo de lugar (via pública, estabelecimento comercial, ponto de ônibus, etc) a partir da posição e do período do dia onde se encontra abre a possibilidade de evitar locais mais perigosos, auxiliando na tomada de decisão de utilizar o celular, mesmo que a área tenha um alto índice de crime.

## Objetivos
O primeiro objetivo deste trabalho é o desenvolvimento de uma metodologia puramente orientada a dados para a predição de locais com o maior proporção de risco de roubo de celular, através de análise de dados e aprendizado de máquina. Esta parte envolve duas etapas:

- Estudo das pesquisas relacionadas a análise e predição de distribuição espacial de crimes utilizando dados abertos. Pesquisas iniciais [7-10] estudam predições de áreas com alto índice de crimes (hotspots), que beneficiam a prática do policiamento preventivo e alocação eficiente de recursos policiais (que são limitados). Os resultados são favoráveis quanto ao uso de dados e algoritmos de aprendizado de máquina para predições de crimes.

- Criação de um modelo. Para isso, deve-se processar, tratar e analisar os dados brutos dos boletins de ocorrência disponíveis no portal da transparência da Secretaria da Segurança Pública. Os dados analisados deverão ser preparados para treinar um modelo de classificação com o uso de algoritmos de Aprendizado de Máquina. Este modelo deverá informar os tipos de locais mais prováveis para o crime a partir de uma geolocalização e um período do dia. Deverá ser utilizado o Scikit-Learn, que é uma biblioteca de aprendizado de máquina de código aberto para a linguagem de programação Python.

O segundo objetivo é o desenvolvimento de um aplicativo, que implementa o modelo do primeiro objetivo, a fim de prevenir o roubo de celulares. Esta parte envolve duas etapas:

- Desenvolvimento de um sistema web. Uma API (interface de programação de aplicações) deverá receber as requisições com os dados da localização e responder com a predição do modelo do primeiro objetivo. O sistema deverá ser desenvolvido com Django, que é um arcabouço para desenvolvimento para web escrito em Python.

- Desenvolvimento de um aplicativo móvel, que deve capturar a localização atual do dispositivo e realizar a comunicação com a API da primeira etapa. O aplicativo deverá ser desenvolvido com Flutter, que é um SDK (kit de desenvolvimento de software) de código aberto criado pelo Google para o desenvolvimento de aplicativos para Android, iOS, Desktop ou Web

Por fim, deve-se criar um logotipo e publicar o aplicativo em uma loja online (como o Google Play Store) e implantar o sistema web utilizando um serviço de computação em nuvem (como o Google Cloud ou Amazon Web Services). Os trabalhos finais incluem a escrita da monografia e a criação do poster.

## Cronograma
Estudos relacionados (Março, Abril e Maio)
- Distribuição espacial dos crimes
- Técnicas de mineração de dados
- Análise e predição 

Criação do Modelo (Maio)
- Análise de Dados
- Treinamento do Modelo
- Testes e Resultados

Desenvolvimento Web e Aplicativo (Junho, Julho e Agosto)

Produção (Agosto)
- Implantação do sistema web
- Publicação do aplicativo

Trabalhos Finais (Setembro, Outubro e Novembro)
- Escrita da monografia
- Produção do poster

## Referências
[1] Roubo de Celulares no Brasil. Panorama Mobile Time / OpinionBox, Jul. de 2019. Disponível em: <https://panoramamobiletime.com.br/pesquisa-celulares-roubados-julho-de-2019>. Acesso em: 6 de Abr. de 2020. 

[2] Perfil de Roubo. SSP. Disponível em: <http://www.ssp.sp.gov.br/estatistica/perfilroubo.aspx>. Acesso em: 6 de abr. de 2020.

[3] Orientações de Segurança. Polícia Militar. Disponível em: <http://www.policiamilitar.sp.gov.br/servicos/orientacao-seguranca/3/celular>. Acesso em: 6 de abr. de 2020.

[4] Consulta a Celular Impedido. SSP, 2016. Disponível em <http://www.ssp.sp.gov.br/servicos/celulares.aspx>. Acesso em: 6 de Abr. de 2020.

[5] COSTA, Natalia. Dope detém nove e apreende quase 600 celulares na Grande São Paulo. SSP, São Paulo, 27 de Fev. de 2020. Disponível em: <http://www.ssp.sp.gov.br/LeNoticia.aspx?ID=46900>. Acesso em: 6 de Abr. de 2020.

[6] BIAZZI, Renato. Polícia prende 9 pessoas que tentaram embarcar para a África com 550 celulares furtados no carnaval de SP. SP2, 27 de Fev. de 2020. Disponível em: <https://g1.globo.com/sp/sao-paulo/noticia/2020/02/27/policia-prende-9-pessoas-que-tentaram-embarcar-para-a-africa-com-550-celulares-furtados-no-carnaval-de-sp.ghtml>. Acesso em: 6 de Abr. de 2020.

[7] BOGOMOLOV, Andrey; LEPRI, Bruno; STAIANO, Jacopo; OLIVER, Nuria; PIANESI, Fabio; PENTLAND, Alex. Once Upon a Crime: Towards Crime Prediction from Demographics and Mobile Data. arXiv:1409.2983, 2014

[8] YU, Chung-Hsien; WARD, Max W.; MORABITO, Melissa; DING, Wei. Crime Forecasting Using Data Mining Techniques. Data Mining Workshops(ICDMW), 2011 IEEE 11th International Conference on, Vancouver, BC, Canada, 11 de Dezembro de 2011, p. 779-786.

[9] WANG, Bao; ZHANG Duo; ZHANG, Duanhao; BERTOZZI, Andrea L.; BRANTINGHAM, P.Jeffery. Deep Learning for Real-Time Crime Forecasting. arXiv:1707.03340, 2017.

[10] ARAUJO JR, Adelson; CACHO, Nélio; BEZERRA, Leornardo; VIEIRA, Carlos; BORGES, Julio. Towards a Crime Hotspot Detection Framework for Patrol Planning. 2018 IEEE 20th International Conference on High Performance Computing and Communications; IEEE 16th International Conference on Smart City; IEEE 4th International Conference on Data Science and Systems (HPCC/SmartCity/DSS), Exeter, Reino Unido, 2018, p. 1256-1263.
