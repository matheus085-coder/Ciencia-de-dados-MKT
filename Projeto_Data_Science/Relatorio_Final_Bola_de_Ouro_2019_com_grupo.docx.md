**CIÊNCIA DE DADOS APLICADA AO**  
**MARKETING ESPORTIVO**

**Impacto social e Análise complementar da avaliação da Bola de Ouro 2019**

Metodologia CRISP-DM e proposta de inovação social  
Campanha: ALÉM DOS GOLS

Autor: Ariel Ernesto Duarte Dominguez

Francisco Luís Moisés Júnior  
Artur Ponce de Leon Arruda  
Matheus Bortoluzzi Cardoso Lima

UniCEUB — Ciência de Dados / Marketing Digital  
Professora: Kadidja Valéria Reginaldo de Oliveira  
Brasília — 2026

# **Resumo**

Este projeto investiga como uma metodologia estatística complementar pode ampliar a análise do desempenho individual de jogadores de futebol, considerando indicadores ofensivos e defensivos normalizados por posição. O estudo utiliza dados do FBref referentes às cinco principais ligas europeias na temporada 2018–2019 e toma a classificação da Bola de Ouro de 2019, organizada pela France Football, como referência comparativa. Após limpeza, integração e agregação das bases, foram identificados 2.556 atletas únicos, dos quais 1.607 atingiram o critério mínimo de 900 minutos. Foram construídos indicadores compostos de desempenho ofensivo e defensivo por meio de Z-scores posicionais. Entre os 30 finalistas oficiais, 22 jogadores de linha tiveram correspondência com os dados e foram incluídos no ranking alternativo. Nesse recorte, Trent Alexander-Arnold ocupa a primeira posição pelo índice equitativo, enquanto Lionel Messi aparece em segundo e Virgil van Dijk em décimo segundo. Os resultados demonstram que a ponderação escolhida altera a ordenação dos atletas, mas não comprovam, isoladamente, viés sistemático ou superioridade do modelo. Como aplicação estratégica, propõe-se a campanha “Além dos Gols”, voltada à transparência metodológica, ao debate qualificado e à equidade no reconhecimento esportivo.

Palavras-chave: Ciência de Dados; CRISP-DM; marketing esportivo; Bola de Ouro; Z-score; equidade; análise de desempenho.

# **1\. Introdução**

Premiações individuais ocupam posição relevante no ecossistema do futebol profissional. Além do reconhecimento esportivo, elas podem influenciar a visibilidade pública dos atletas, suas narrativas de carreira, oportunidades comerciais e estratégias de comunicação de clubes, patrocinadores e representantes. Nesse contexto, a avaliação individual envolve um desafio: jogadores de posições distintas executam funções diferentes e contribuem para o resultado coletivo por meio de ações que nem sempre são diretamente comparáveis.

Gols e assistências são indicadores importantes, mas não descrevem sozinhos o desempenho de defensores, meio-campistas e goleiros. Ações como desarmes, interceptações, bloqueios, recuperação de posse, organização tática e defesas também podem ser relevantes. O problema investigado neste trabalho não é afirmar que a Bola de Ouro utiliza exclusivamente estatísticas ofensivas, nem questionar a legitimidade de sua votação. A investigação busca verificar como uma metodologia estatística complementar, que normalize indicadores por posição e combine dimensões ofensivas e defensivas, reorganiza a classificação de um conjunto de finalistas.

A edição de 2019 foi escolhida como estudo de caso. Lionel Messi venceu com 686 pontos, seguido por Virgil van Dijk, com 679 pontos, uma diferença de sete pontos. A partir dessa referência, o projeto utiliza o processo CRISP-DM para organizar a compreensão do negócio, dos dados, sua preparação, a modelagem, a avaliação e a proposta de aplicação.

## **1.1 Problema de pesquisa**

Em que medida uma metodologia complementar, baseada em indicadores ofensivos e defensivos normalizados por posição, altera a avaliação comparativa de jogadores finalistas da Bola de Ouro de 2019?

## **1.2 Objetivo geral**

Construir e avaliar um índice estatístico exploratório que combine desempenho ofensivo e defensivo, normalizado por posição, para comparar sua ordenação com a classificação oficial da Bola de Ouro de 2019\.

## **1.3 Objetivos específicos**

* Integrar dados de desempenho padrão e defensivo da temporada 2018–2019 das cinco grandes ligas europeias.  
* Tratar registros de atletas que atuaram por mais de um clube e selecionar jogadores com pelo menos 900 minutos.  
* Calcular Z-scores por posição para gols, assistências, desarmes ganhos e interceptações.  
* Construir um índice composto com pesos iguais para as dimensões ofensiva e defensiva.  
* Comparar o ranking alternativo com a classificação oficial no conjunto de finalistas com dados correspondentes.  
* Discutir limitações e propor uma aplicação estratégica com potencial contribuição para a equidade no reconhecimento esportivo.

# **2\. Metodologia: CRISP-DM**

O projeto foi estruturado pelas seis fases do CRISP-DM: entendimento do negócio, entendimento dos dados, preparação dos dados, modelagem, avaliação e implantação. A seguir, são descritas as decisões registradas no notebook e os resultados disponíveis nos arquivos.

## **2.1 Fase 1 — Entendimento do negócio**

A Bola de Ouro é uma premiação organizada pela France Football. O reconhecimento individual possui relevância esportiva e também repercussões de comunicação e marketing: atletas premiados ou bem posicionados tendem a receber maior exposição editorial e podem utilizar esse reconhecimento em estratégias de posicionamento de marca pessoal, patrocínio e relacionamento com o público.

O desafio analítico está na comparabilidade entre posições. Um atacante pode se destacar por gols e assistências, enquanto um defensor contribui por meio de desarmes, interceptações e prevenção de oportunidades adversárias. Comparar apenas valores absolutos de métricas diferentes não seria adequado. Por isso, o projeto adota a normalização estatística por posição como uma forma exploratória de comparar o desempenho relativo de cada atleta em relação a jogadores de função semelhante.

A hipótese de trabalho é que uma combinação equilibrada de indicadores ofensivos e defensivos pode produzir uma classificação diferente da votação oficial. Essa hipótese não equivale a afirmar que a votação oficial é comprovadamente enviesada: o modelo não observa diretamente os critérios individuais de cada votante, e a pontuação final pode refletir fatores técnicos, contextuais e qualitativos não presentes nas bases.

## **2.2 Fase 2 — Entendimento dos dados**

Foram utilizadas duas bases extraídas do FBref, referentes à temporada 2018–2019 nas cinco grandes ligas europeias: Premier League, La Liga, Serie A, Bundesliga e Ligue 1\. A base standard contém informações gerais, minutagem, gols, assistências e cartões. A base defensive contém variáveis de ações defensivas.

| Base / conjunto | Registros | Variáveis relevantes | Uso no projeto |
| :---- | :---- | :---- | :---- |
| standard\_2018\_2019.csv | 2.656 | Player\_ID, Player, Pos, Squad, Comp, Min, Gls, Ast, CrdY, CrdR | Desempenho geral, ofensivo, minutagem e disciplina |
| defensive\_2018\_2019.csv | 2.656 | Player\_ID, Squad, Comp, TklW, Int | Indicadores defensivos disponíveis |
| Ranking oficial de 2019 | 30 finalistas | Posição e Pontos\_BdO | Referência de comparação |
| ranking\_auditado\_bola\_de\_ouro\_2019.csv | 22 correspondências | Ranks, jogador, posição, estatísticas, Z-scores e índice | Resultado consolidado do modelo |

A leitura das bases no notebook identificou 2.656 registros e 2.556 identificadores únicos de atletas. A diferença de 100 registros está associada a jogadores com mais de um registro por terem atuado por clubes diferentes durante a temporada. O diagnóstico de valores ausentes apontou que TklW e Int estavam disponíveis para os registros utilizados, enquanto diversas outras colunas defensivas, incluindo Blocks e Clr, estavam integralmente vazias no arquivo histórico analisado. Portanto, essas variáveis não foram incorporadas ao índice.

O notebook registra também dois valores ausentes na variável Nation e um na variável Pos em cada base. A análise depende principalmente de posição, identificação do atleta e métricas numéricas. A disponibilidade limitada de variáveis defensivas constitui uma restrição importante para a interpretação do desempenho de zagueiros e outros jogadores com funções defensivas.

## **2.3 Fase 3 — Preparação dos dados**

O notebook removeu linhas de cabeçalho repetidas, renomeou o identificador de jogador para Player\_ID e selecionou apenas as colunas consideradas válidas. Em seguida, realizou a integração das bases standard e defensive por Player\_ID, Squad e Comp, utilizando junção interna (inner join). Essa chave permite associar as estatísticas do mesmo atleta, clube e competição.

As variáveis numéricas foram convertidas explicitamente para formato numérico. Valores não convertíveis foram tratados como ausentes e, no código executado, preenchidos com zero nas colunas numéricas selecionadas. Essa decisão deve ser considerada com cautela: zero pode significar ausência real de ocorrência, mas também pode mascarar falhas de registro. Uma versão futura do pipeline deve distinguir ausência de informação de valor efetivamente igual a zero.

Os registros foram então agregados por Player\_ID, somando minutagem, partidas, gols, assistências, cartões, desarmes ganhos e interceptações. Para atributos textuais, foi mantido o primeiro valor ou reunidos os clubes e competições distintos. Após a consolidação, foram identificados 2.556 atletas únicos. Aplicou-se o filtro de pelo menos 900 minutos, resultando em 1.607 atletas regulares: 654 meio-campistas (MF), 556 defensores (DF), 273 atacantes (FW) e 124 goleiros (GK).

| Posição primária | Atletas com Min ≥ 900 |
| :---- | :---- |
| MF — Meio-campistas | 654 |
| DF — Defensores | 556 |
| FW — Atacantes | 273 |
| GK — Goleiros | 124 |
| Total | 1.607 |

A posição primária foi obtida a partir dos dois primeiros caracteres do campo de posição. Essa simplificação permite agrupar atletas em quatro categorias amplas, mas pode reunir funções táticas distintas — por exemplo, laterais e zagueiros na categoria DF, ou volantes e meias ofensivos na categoria MF. Essa limitação deve ser considerada ao interpretar o Z-score posicional.

## **2.4 Fase 4 — Modelagem**

A modelagem utilizou o Z-score para padronizar cada métrica dentro de cada grupo posicional. Para uma métrica x do atleta i pertencente à posição p, o cálculo é:

**Zᵢ,ₚ \= (xᵢ,ₚ − μₚ) / σₚ**

Em que μₚ representa a média da métrica entre os atletas da posição p e σₚ representa o desvio-padrão dessa métrica no mesmo grupo. Valores positivos indicam desempenho acima da média posicional na variável considerada; valores negativos indicam desempenho abaixo da média.

Foram construídos dois indicadores compostos. O indicador ofensivo corresponde à média dos Z-scores de gols e assistências. O indicador defensivo corresponde à média dos Z-scores de desarmes ganhos e interceptações:

**Z\_Ofensivo \= (Z\_Gls \+ Z\_Ast) / 2**  
**Z\_Defensivo \= (Z\_TklW \+ Z\_Int) / 2**

O índice equitativo foi definido com ponderação de 50% para cada dimensão:

**Índice Equitativo \= 0,50 × Z\_Ofensivo \+ 0,50 × Z\_Defensivo**

O notebook também calculou Z\_Disciplina, invertendo o Z-score de cartões por 90 minutos, de modo que menor quantidade de cartões correspondesse a valor mais alto. Essa variável foi utilizada em análises descritivas, mas não integra o índice equitativo final.

Além do índice, o notebook ajustou uma regressão linear múltipla, com Pontos\_BdO como variável dependente e Z\_Ofensivo e Z\_Defensivo como variáveis explicativas. Também ajustou um Random Forest Regressor com 200 árvores e profundidade máxima 3\. Os resultados dos modelos foram calculados na própria amostra, sem registro de validação cruzada ou conjunto independente de teste; por isso, suas métricas de ajuste não devem ser interpretadas como desempenho preditivo fora da amostra.

## **2.5 Fase 5 — Avaliação dos resultados**

A lista oficial contém 30 finalistas. O cruzamento com atletas regulares das cinco grandes ligas, excluindo goleiros para a comparação entre jogadores de linha, resultou em 22 correspondências: 15 atacantes, quatro defensores e três meio-campistas. Oito finalistas não foram incluídos na comparação por não apresentarem correspondência no recorte de dados utilizado ou por não atenderem às condições de inclusão.

| Atleta | Posição | Rank oficial | Rank equitativo | Índice equitativo |
| :---- | :---- | :---- | :---- | :---- |
| Trent Alexander-Arnold | DF | 19 | 1 | 1,9824 |
| Lionel Messi | FW | 1 | 2 | 1,7915 |
| Raheem Sterling | FW | 12 | 3 | 1,5072 |
| Eden Hazard | FW | 13 | 4 | 1,5001 |
| Sadio Mané | FW | 4 | 5 | 1,3984 |
| Antoine Griezmann | FW | 18 | 6 | 1,2151 |
| Bernardo Silva | MF | 9 | 7 | 1,1540 |
| Kylian Mbappé | FW | 6 | 8 | 1,0727 |
| Pierre-Emerick Aubameyang | FW | 20 | 9 | 1,0616 |
| Sergio Agüero | FW | 16 | 10 | 0,9890 |
| Marquinhos | DF | 28 | 11 | 0,9315 |
| Virgil van Dijk | DF | 2 | 12 | 0,9171 |

O principal resultado do ranking alternativo é a primeira posição de Trent Alexander-Arnold, que havia ocupado a 19ª posição oficial. Lionel Messi aparece em segundo lugar. Virgil van Dijk, segundo colocado oficial, aparece em 12º no ranking alternativo. Esses resultados mostram que a ordenação depende dos indicadores e dos pesos escolhidos. Não demonstram que Trent deveria necessariamente ter vencido a premiação, nem que o modelo 50/50 seja universalmente mais justo.

O caso Messi–Van Dijk evidencia a importância de interpretar os indicadores dentro de suas limitações. Messi apresenta Z\_Ofensivo de 4,3043 e Z\_Defensivo de \-0,7213, resultando em índice de 1,7915. Van Dijk apresenta Z\_Ofensivo de 1,5277 e Z\_Defensivo de 0,3065, resultando em 0,9171. O índice de Van Dijk pode não capturar integralmente contribuições defensivas que não estão representadas pelas variáveis disponíveis, como bloqueios, cortes, duelos aéreos e prevenção de finalizações.

| Modelo estatístico | Resultado informado no notebook | Interpretação cautelosa |
| :---- | :---- | :---- |
| Regressão linear múltipla | Coef. Z\_Ofensivo \= \+68,89; coef. Z\_Defensivo \= −16,64; R² \= 0,1403 | Baixo poder explicativo na amostra; coeficientes exploratórios, sem inferência causal |
| Random Forest | Importância: ofensivo 53,7%; defensivo 46,3%; R² \= 0,7125 | Ajuste na própria amostra; sem validação independente, pode haver sobreajuste |

A regressão linear apresenta R² de 0,1403, isto é, o modelo explica aproximadamente 14,03% da variação observada na pontuação oficial dentro da amostra analisada. O coeficiente positivo do indicador ofensivo e o coeficiente negativo do defensivo descrevem a associação estimada pelo modelo, condicionada às duas variáveis incluídas. Não foram apresentados intervalos de confiança, valores-p ou validação fora da amostra. Assim, não se deve afirmar que os coeficientes são estatisticamente significativos.

O Random Forest apresenta R² de 0,7125 e importância de variáveis relativamente próxima (53,7% para o componente ofensivo e 46,3% para o defensivo). Como o ajuste e a avaliação foram feitos nos mesmos 22 registros, esse resultado pode refletir sobreajuste e não comprova capacidade de generalização. Além disso, importância de variável em Random Forest não equivale a efeito causal nem a peso diretamente interpretável na votação.

Há ainda uma diferença conceitual entre a distribuição das posições na amostra e o processo de votação. Dos 22 jogadores de linha com correspondência, 18 são atacantes ou meio-campistas e quatro são defensores. Esse perfil descreve a composição do conjunto analisado, mas, isoladamente, não comprova exclusão ou desvalorização sistemática de determinadas posições. A lista de finalistas e os critérios oficiais podem envolver elementos que não estão nas bases estatísticas.

## **2.6 Limitações e qualidade da evidência**

* Apenas 22 dos 30 finalistas oficiais foram incluídos no cruzamento, o que reduz o tamanho e a representatividade da amostra.  
* A base defensiva histórica não disponibiliza diversas métricas relevantes, como bloqueios, cortes e duelos aéreos ganhos.  
* O índice utiliza gols, assistências, desarmes ganhos e interceptações; não contempla todas as dimensões do desempenho, nem indicadores específicos suficientes para goleiros.  
* O agrupamento em quatro posições amplas pode misturar funções táticas diferentes.  
* O cálculo usa contagens totais em vez de padronizar todas as métricas por 90 minutos ou por oportunidades de ação.  
* A regressão e o Random Forest foram avaliados na própria amostra, sem validação externa ou temporal documentada.  
* A ponderação 50/50 é uma decisão normativa do projeto, não um peso estimado ou validado por especialistas.  
* A pontuação da premiação pode depender de critérios qualitativos e contextuais não presentes nos dados.

## **2.7 Fase 6 — Implantação e aplicação estratégica**

O índice deve ser tratado como ferramenta exploratória de apoio à análise, e não como substituto automático da votação ou como critério oficial de premiação. Uma aplicação futura poderia ocorrer em três níveis: (1) análise interna por especialistas e analistas de desempenho; (2) apresentação transparente dos indicadores e de suas limitações aos responsáveis por processos de avaliação; e (3) comunicação pública que ajude torcedores e mídia a compreenderem diferentes dimensões do desempenho.

Para uma implantação responsável, recomenda-se ampliar as bases com indicadores adequados a cada posição, obter validação de especialistas, testar diferentes pesos, avaliar sensibilidade do ranking e documentar o processo de ponta a ponta. Também seria necessário verificar se as métricas estão disponíveis de forma consistente entre ligas, temporadas e posições.

# **3\. Impacto social e inovação na estratégia de Marketing Digital**

## **3.1 Necessidade social**

A necessidade social relacionada ao projeto é a equidade no reconhecimento do desempenho esportivo. Atletas de posições diferentes contribuem para o resultado coletivo por meio de responsabilidades distintas. Quando a comunicação ou os modelos de avaliação dão visibilidade desproporcional a um conjunto restrito de indicadores, existe o risco de que outras contribuições sejam menos compreendidas pelo público ou insuficientemente representadas em análises comparativas.

Os dados deste estudo não comprovam que essa desigualdade de reconhecimento ocorre de forma sistemática na premiação. Eles mostram que a aplicação de critérios estatísticos diferentes produz uma classificação diferente para o grupo de 22 finalistas analisado. A necessidade social é, portanto, apresentada como um problema de representatividade e transparência a ser investigado e enfrentado por meio de uma proposta metodológica complementar.

## **3.2 Públicos afetados, benefícios, riscos e mitigação**

| Público | Possível benefício | Risco ou barreira | Mitigação |
| :---- | :---- | :---- | :---- |
| Organizadores e responsáveis pela avaliação (público institucional prioritário) | Acesso a uma ferramenta complementar e documentada para análise comparativa | Índice simplificado ser confundido com critério definitivo | Apresentar o modelo como apoio exploratório, com validação e documentação |
| Especialistas e analistas de desempenho | Base para debate técnico e aperfeiçoamento de métricas | Indicadores incompletos ou inadequados a certas posições | Revisão técnica, métricas específicas por posição e análise de sensibilidade |
| Atletas e representantes | Maior visibilidade de contribuições além de gols e assistências | Uso comercial de rankings sem contexto ou comparação inadequada | Explicar escopo, temporada, posição e limitações dos indicadores |
| Torcedores e público geral | Maior compreensão sobre diferentes funções no futebol | Barreiras de linguagem, acessibilidade ou interpretação estatística | Conteúdo simples, legendado, visual, acessível e com glossário |
| Mídia e criadores de conteúdo | Novas pautas e narrativas baseadas em dados | Reprodução de conclusões exageradas ou descontextualizadas | Materiais de apoio, metodologia aberta e notas de cautela |

A estratégia de marketing digital deve distinguir dois públicos: o público institucional prioritário, formado pelos organizadores e responsáveis pela avaliação, e o público de comunicação, que inclui especialistas, atletas, mídia e torcedores. A campanha poderá dialogar com todos, com atenção especial à compreensão dos torcedores, sem confundir engajamento de audiência com adoção institucional da metodologia.

## **3.3 Evidências disponíveis e limites**

A evidência já disponível é a comparação entre a classificação oficial e o índice alternativo calculado para 22 finalistas com correspondência de dados da temporada 2018–2019. A fonte estatística é o conjunto de dados do FBref utilizado no projeto; o período observado é a temporada 2018–2019; e o grupo analisado é formado pelos finalistas de linha incluídos no cruzamento. O ranking alternativo e os cálculos estão registrados no notebook e no arquivo ranking\_auditado\_bola\_de\_ouro\_2019.csv.

Não houve aplicação da campanha, entrevistas com responsáveis pela premiação, validação formal por especialistas ou questionário com torcedores. Assim, não há evidência de mudança de percepção, compreensão ou comportamento. Esses impactos devem ser apresentados como resultados esperados. Conforme a orientação da professora, visualizações medem exposição, mas compreensão exige medida própria; para avaliar mudança, é necessário comparar uma situação inicial com uma medição posterior equivalente.

## **3.4 Ação de inovação social: campanha Além dos Gols**

**ALÉM DOS GOLS**  
**Uma avaliação mais completa. Um reconhecimento mais equilibrado.**

Para enfrentar o desafio de representar de maneira mais abrangente as contribuições de atletas de diferentes posições, propõe-se a campanha digital “Além dos Gols”, voltada a organizadores da premiação, especialistas, atletas, mídia e torcedores. A iniciativa apresentará a metodologia complementar por meio de infográficos, comparações entre o ranking oficial e o índice exploratório, conteúdos sobre as particularidades das posições e vídeos curtos que expliquem os indicadores. A campanha pode contribuir para a autoridade de uma iniciativa de análise esportiva baseada em dados e gerar benefício social ao estimular transparência, compreensão e debate qualificado sobre o reconhecimento dos atletas. Para reduzir o risco de interpretações equivocadas, todos os materiais devem apresentar critérios, período, público analisado e limitações.

| Formato / conteúdo | Objetivo | Canal sugerido |
| :---- | :---- | :---- |
| Infográfico: o que cada posição entrega | Explicar diferenças entre funções e indicadores | Instagram / LinkedIn |
| Carrossel: como funciona o Z-score posicional | Apresentar a metodologia em linguagem acessível | Instagram |
| Comparativo: ranking oficial x índice complementar | Mostrar a mudança de ordenação sem declarar um vencedor legítimo | Instagram / site / LinkedIn |
| Vídeo curto: além de gols e assistências | Dar exemplos de ações defensivas e de construção de jogo | Reels / TikTok / YouTube Shorts |
| Nota técnica e documentação do modelo | Permitir avaliação crítica por especialistas e instituições | Site / LinkedIn / apresentação institucional |

Os recursos necessários incluem as bases estatísticas, notebook e pipeline documentado, ferramentas de visualização, produção audiovisual, revisão técnica e acessibilidade dos conteúdos (legendas, contraste e linguagem clara). A comunicação não deve apresentar o índice como ranking oficial ou como prova de injustiça; deve posicioná-lo como proposta exploratória para discussão e validação.

## **3.5 Indicadores de acompanhamento**

| Indicador | O que mede / cálculo | Público e momento | Resultado esperado |
| :---- | :---- | :---- | :---- |
| Alcance e exposição | Contas alcançadas, impressões e visualizações por conteúdo | Audiência digital; durante e ao final da campanha | Ampliar a exposição à proposta; não equivale a compreensão |
| Engajamento qualificado | Compartilhamentos, salvamentos, comentários pertinentes e cliques em materiais técnicos, reportados separadamente | Torcedores, mídia e especialistas; por publicação e no fechamento | Identificar interesse real e dúvidas recorrentes |
| Compreensão dos critérios | Percentual de participantes que respondem corretamente a perguntas sobre Z-score, indicadores e limites do índice | Torcedores; questionário antes e depois da exposição aos conteúdos | Aumento esperado na proporção de respostas corretas |
| Percepção sobre diversidade de contribuições | Comparação das respostas a itens sobre a relevância de ações ofensivas e defensivas | Torcedores; linha de base e medição posterior | Maior reconhecimento declarado de diferentes contribuições |
| Avaliação técnica do modelo | Rubrica de especialistas sobre validade, clareza, adequação posicional e limitações | Analistas e especialistas; antes de qualquer proposta de adoção | Identificar ajustes necessários e condições mínimas de uso |

A primeira medição deve ocorrer antes da exposição à campanha, utilizando questionário com perguntas objetivas e uma escala de percepção. Após a campanha, o mesmo instrumento — ou uma versão equivalente — deve ser aplicado a um grupo comparável, registrando tamanho da amostra, perfil dos participantes e taxa de resposta. Para especialistas, recomenda-se uma avaliação estruturada do modelo, com critérios previamente definidos. Como não há coleta realizada até o momento, todos os resultados são metas ou efeitos esperados, não resultados observados.

# **4\. Considerações finais**

O projeto investigou a possibilidade de utilizar Ciência de Dados para construir uma análise complementar do desempenho individual de jogadores de futebol, considerando indicadores ofensivos e defensivos normalizados por posição. A aplicação da metodologia CRISP-DM organizou o processo desde a compreensão do problema até a proposta de aplicação estratégica.

A integração das bases do FBref resultou em 2.556 atletas únicos, dos quais 1.607 atingiram o filtro mínimo de 900 minutos. A partir dos Z-scores de gols, assistências, desarmes ganhos e interceptações, foi construído um índice com pesos iguais para as dimensões ofensiva e defensiva. No conjunto de 22 finalistas com correspondência de dados, Trent Alexander-Arnold passou da 19ª posição oficial para a primeira posição no ranking alternativo; Messi ficou em segundo e Van Dijk em 12º.

Esses resultados sustentam que uma escolha diferente de indicadores e pesos altera a ordenação comparativa. Não sustentam, isoladamente, que a votação oficial tenha sido injusta, que exista viés sistemático comprovado ou que o índice 50/50 seja a metodologia ideal. A regressão linear apresentou baixo R² na amostra, e os modelos não foram validados em dados independentes. A ausência de métricas defensivas relevantes, a exclusão de goleiros da comparação dos finalistas e o agrupamento amplo de posições também limitam as conclusões.

Como aplicação, a campanha “Além dos Gols” propõe apresentar a metodologia de forma transparente e estimular a discussão sobre a representatividade das diferentes contribuições esportivas. Sua contribuição social esperada é favorecer uma avaliação mais abrangente e compreensível, enquanto sua contribuição de negócio está na produção de conteúdo esportivo baseado em dados e no fortalecimento da autoridade analítica. O impacto sobre a compreensão do público e a eventual adoção institucional ainda precisam ser verificados por meio de indicadores, pesquisa com torcedores e avaliação de especialistas.

Para evoluir o projeto, recomenda-se ampliar o conjunto de variáveis, incluir métricas específicas para goleiros e funções táticas, testar pesos alternativos, realizar validação cruzada e análise de sensibilidade, e submeter a metodologia à avaliação de especialistas. Assim, o índice poderá evoluir de uma exploração estatística para uma ferramenta de apoio mais robusta, sem substituir o julgamento técnico e contextual necessário à avaliação esportiva.

# **Referências e materiais utilizados**

* FBref. Bases estatísticas da temporada 2018–2019 — Big 5 European Leagues Combined. Arquivos fornecidos no projeto: standard\_2018\_2019.csv e defensive\_2018\_2019.csv. O notebook registra a origem e a necessidade de citação do provedor.  
* France Football. Classificação oficial da Bola de Ouro 2019\. Lista de 30 finalistas e pontuações reproduzida no notebook do projeto para fins de análise acadêmica.  
* Projeto\_CRISPDM\_BolaDeOuro2019.ipynb. Notebook de análise desenvolvido para a disciplina Ciência de Dados / Marketing Digital, UniCEUB, 2026\.  
* ranking\_auditado\_bola\_de\_ouro\_2019.csv. Base consolidada com os 22 finalistas correspondentes e os resultados do índice equitativo.  
* CEUB. Tutorial do estudante: Impacto social no relatório final. Aula finalizadora de 18/11/2026, professora Kadidja Valéria. Material fornecido para a etapa de impacto social e inovação.

# **Nota de revisão técnica antes da entrega**

Este relatório foi elaborado a partir dos arquivos disponibilizados e dos resultados registrados no notebook. Antes da entrega acadêmica, recomenda-se revisar a referência bibliográfica completa do FBref e da classificação oficial, confirmar o padrão de citação exigido pelo curso, e executar novamente o notebook em ambiente limpo para reproduzir os resultados. Também é recomendável corrigir no notebook expressões conclusivas como “comprovar viés”, “refutar neutralidade” e “confirmado”, substituindo-as por formulações exploratórias compatíveis com o tamanho da amostra e com a ausência de validação inferencial.