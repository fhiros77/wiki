# RESILIENCIA DE SISTEMAS

Resiliência de sistemas não é sobre impedir falhas, é sobre sobreviver a elas

Durante muito tempo eu associava qualidade técnica com ausência de falhas. Se uma aplicação caísse, a conclusão era simples, a arquitetura estava errada.

Com o tempo trabalhando com sistemas distribuídos, percebi algo diferente, falhas não são exceção, elas fazem parte do funcionamento normal do ambiente. Rede oscila, dependências ficam lentas, serviços reiniciam, conexões expiram, memória cresce e consumidores param de consumir.

A pergunta deixou de ser, “Como evitar falhas?” e passou a ser “Como o sistema continua funcionando quando elas acontecerem?”

Quando retry vira parte do problema, uma das primeiras armadilhas que encontrei foi acreditar que retry resolvia indisponibilidade. Na prática, retry sem controle pode criar exatamente o efeito contrário. Imagine um serviço já sobrecarregado recebendo centenas de novas tentativas simultâneas. O incidente inicial vira um incidente maior. Hoje, antes de pensar em retry, normalmente penso em: timeout explícito, limite de tentativas, backoff exponencial, operações idempotentes, proteção contra efeito cascata.

O objetivo deixou de ser insistir mais e passou a ser falhar de maneira controlada, ou seja recuperação automática deixou de ser opcional.

Em integrações assíncronas, perder conexão não deveria exigir intervenção humana. Se um consumidor desconecta e só volta quando alguém percebe, o sistema não está realmente resiliente. O que passou a fazer parte do desenho: detecção automática de desconexão, reprocessamento seguro, re-subscribe controlado, prevenção de múltiplas reconexões concorrentes e observabilidade do ciclo de recuperação.

Resiliência não acontece quando tudo funciona, mas sim quando o sistema consegue voltar sozinho. Nem toda falha precisa derrubar tudo, devemos continuar entregando parte do valor, pois indisponibilidade parcial é melhor do que indisponibilidade total. Percebi que disponibilidade não significa entregar 100% o tempo todo, mas sim significa preservar o que é essencial, ou seja podemos responder com dados em cache, desacoplar processamento, reduzir funcionalidades temporariamente ou priorizar operações críticas.

Um sistema pode parecer saudável e ainda estar degradando, mantendo conexões órfãs, consumo crescente de memória, recursos que nunca são liberados, buffers acumulando. Esses problemas raramente aparecem como erro explícito, normalmente aparecem como comportamento estranho antes do incidente. 

Hoje, quando penso em resiliência, normalmente penso em quatro perguntas:
O que acontece quando uma dependência falha?
Como o sistema se recupera sozinho?
O erro se propaga ou fica isolado?
Como vou descobrir isso antes do usuário?

Conclusão:
Sistemas robustos não são os que nunca quebram, mas sim os que continuam úteis quando algo quebra.