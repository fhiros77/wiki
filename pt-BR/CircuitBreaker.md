# CIRCUIT BREAKER

O que é Circuit Breaker?

Em sistemas distribuídos, falhas não são exceção, são inevitáveis. Quando uma API externa demora, um banco fica indisponível ou um serviço começa a responder com erro, o maior risco não é apenas a falha inicial. O problema real acontece quando essa falha se propaga e começa a consumir threads, conexões e recursos até derrubar partes saudáveis do sistema. É exatamente para isso que existe o padrão Circuit Breaker.

O Circuit Breaker (Disjuntor) é um padrão de resiliência que interrompe chamadas para um serviço que está falhando continuamente.

A ideia é simples, em vez de continuar tentando acessar um recurso indisponível e piorar o problema, o sistema detecta falhas e “abre o circuito”, interrompendo novas chamadas temporariamente. É o mesmo conceito do disjuntor elétrico da sua casa.

Estados do Circuit Breaker

Closed (Fechado): Tudo funciona normalmente. As requisições seguem para o serviço.

Se a taxa de erro ultrapassar o limite configurado, muda para Open.

Open (Aberto): O sistema para de enviar requisições e retorna erro imediatamente ou executa um fallback.

Aplicação → Circuito Aberto → Resposta imediata

Objetivo: reduzir pressão no serviço falhando, evitar timeout em cascata e liberar recursos internos.

Half-Open (Semiaberto): Depois de um período de espera, algumas requisições são liberadas.
Se funcionarem, fecha novamente.
Se falharem, abre de novo.