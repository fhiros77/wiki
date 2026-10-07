# Principais erros e armadilhas no uso de filas e sistemas de mensageria

O uso de filas, sejam estruturas de dados em memória ou sistemas de mensageria assíncrona é um dos pilares para construir sistemas escaláveis e desacoplados. Porém, na prática, muitos projetos enfrentam problemas recorrentes que transformam ganhos de performance em gargalos operacionais.

Um dos erros mais comuns é assumir que toda mensagem eventualmente será processada com sucesso, mas quando um consumidor falha repetidamente ao processar uma mensagem, sem uma política de descarte ou isolamento, ela retorna continuamente para a fila principal. O resultado será consumo desnecessário de CPU e memória, aumento da latência das mensagens válidas, filas congestionadas e dificuldade de identificar a causa raiz. O ideal seria implementar uma Dead-Letter Queue (DLQ) com limites claros de retry e mecanismos de observabilidade para análise posterior.

Outro erro recorrente é usar o padrão errado de distribuição. Em uma fila tradicional (queue), cada mensagem é consumida por apenas um consumidor. Já em um modelo (Pub/Sub), vários consumidores recebem a mesma mensagem de forma independente. Nesse caso, sempre usar Queue para processamento exclusivo e Topic para propagação de eventos.

Adicionar mais consumidores não significa necessariamente ganhar performance. Sem controle adequado, múltiplas instâncias podem disputar os mesmos recursos, gerar race conditions e produzir atualizações inconsistentes. Em brokers distribuídos, o problema aparece também no balanceamento entre partições e grupos consumidores. Nesse caso e necessario controlar paralelismo por chave de negócio, ajustar prefetch, visibility timeout e número de consumidores, evitar compartilhamento de estado entre workers.

Mensageria não substitui banco de dados, pois filas foram desenhadas para transporte e desacoplamento, não retenção indefinida. Deve-se então armazenar arquivos ou grandes objetos externamente, enviar apenas IDs ou referências, monitorar backlog, retenção e throughput.

Muitos sistemas assumem que mensagens serão entregues exatamente na ordem de publicação, mas na prática reprocessamentos, concorrência, múltiplas partições e falhas de rede podem alterar a ordem observada. Exemplo crítico: Receber primeiro “saldo = 100” e depois “saldo = 50”. Deve-se usar filas FIFO quando necessário, aplicar versionamento, incluir número sequencial ou timestamp lógico.

Uma regra prática em mensageria é considerar que qualquer mensagem poderá ser entregue mais de uma vez. Nesse caso, falhas de confirmação, reinicializações e retries podem provocar duplicidade. Erro comum nesses casos é processar duas vezes a mesma mensagem. Uma boa pratica é implementar idempotência, usando chave única, controle de versão, tabela de mensagens processadas.

Muitos sistemas só descobrem problemas quando usuários reclamam e sem métricas fica difícil responder. Deve-se Monitorar tamanho da fila, taxa de consumo, taxa de erro, idade média das mensagens, tempo até processamento.

Quando produtores enviam mais rápido do que consumidores conseguem processar, cria-se um efeito cascata. Os sintomas logo aparecem com crescimento contínuo da fila, aumento de memória e timeout em serviços downstream. Deve-se aplicar rate limiting, auto scaling, backpressure e circuit breakers.

No fim, filas resolvem desacoplamento e escalabilidade, mas introduzem desafios clássicos de consistência, observabilidade, ordenação e tolerância a falhas. Em produção, normalmente o problema não é “colocar uma fila”, e sim operar corretamente o ecossistema ao redor dela.