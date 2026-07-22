## Lean 4

### Resumo

O artigo é uma introdução prática ao Lean 4, uma linguagem funcional e assistente de provas usado para verificação formal. O público-alvo são engenheiros de criptografia e desenvolvedores que desejam aprender os fundamentos da verificação formal aplicando-os a um exemplo concreto: a One-Time Pad (OTP). 

#### Os principais conceitos apresentados são:

- O que é verificação formal: em vez de confiar apenas em uma prova escrita por humanos, a demonstração é codificada em Lean e verificada automaticamente. Se o compilador aceita a prova, ela é considerada logicamente correta (assumindo a correção do próprio Lean). 

- Introdução ao Lean: o tutorial explica que Lean é ao mesmo tempo uma linguagem de programação funcional pura e um provador de teoremas. O leitor aprende a usar comandos simples (#eval), a sintaxe da linguagem e conceitos como funções de primeira classe. 

- Modelagem da One-Time Pad: o objetivo é traduzir a definição matemática da OTP para Lean. Para isso, o autor representa bits usando aritmética modular (ZMod 2) e define um tipo BitString sobre o qual implementa a operação XOR. 

Provas sobre o XOR: antes de provar que a OTP está correta, o tutorial demonstra formalmente propriedades fundamentais da operação XOR:
- comutatividade;
- associatividade;
- elemento identidade;
- auto-inversão (x xor x = 0). 

Definição de um Shannon Cipher: usando uma structure de Lean, o artigo modela um esquema de cifra contendo:
- uma função de criptografia;
- uma função de descriptografia;
- uma propriedade de correção que garante que descriptografar uma mensagem criptografada recupera a mensagem original. 

Prova da correção da One-Time Pad: por fim, a OTP é implementada como uma instância de ShannonCipher, usando XOR tanto para cifrar quanto para decifrar. A propriedade de correção é demonstrada automaticamente combinando os lemas provados anteriormente sobre o XOR. 

#### Conclusão

O autor encerra relacionando a verificação formal com projetos modernos de blockchain e criptografia (como Ethereum e Zcash), destacando que essa técnica aumenta a confiança em sistemas críticos. Ao mesmo tempo, ressalta que a verificação formal não elimina todos os riscos, pois ainda é necessário modelar corretamente o sistema e confiar no compilador e nas ferramentas utilizadas. O artigo prepara o terreno para a segunda parte da série, na qual será provada formalmente a segurança perfeita (perfect secrecy) da One-Time Pad. 

## A digestion of the Jacobian conjecture counterexample

O post "A digestion of the Jacobian conjecture counterexample" é a tentativa de Terry Tao de explicar a estrutura conceitual do recém-descoberto contraexemplo para a Conjectura de Jacobi. Em vez de seguir a prova original linha por linha, ele procura responder à pergunta: "por que esse contraexemplo funciona?". 

### Contexto

A Conjectura de Jacobi, formulada em 1939, afirmava que toda aplicação polinomial

F:C^n → C^n

com determinante jacobiano constante e não nulo deveria possuir uma inversa polinomial.

Como um jacobiano não nulo implica invertibilidade local (pelo Teorema da Função Inversa), a conjectura pode ser resumida como:

- invertibilidade local ⇒ invertibilidade global.

Recentemente foi encontrado um contraexemplo explícito em dimensão 3, encerrando a conjectura para n ≥ 3. O caso bidimensional permanece aberto. 

### O objetivo do post

Tao não tenta reproduzir todos os cálculos da construção. Em vez disso, ele procura reorganizar o argumento para revelar a "mecânica" do exemplo.

A ideia central é que o mapa polinomial, apesar de parecer extremamente complicado, pode ser reinterpretado por meio de uma mudança de coordenadas e de uma descrição geométrica muito mais simples.

### A principal observação

Depois de uma transformação adequada, o comportamento do mapa passa a ser governado por um polinômio cúbico auxiliar.

Esse cúbico determina quantos pontos do domínio são enviados para um mesmo ponto da imagem.

- Se o cúbico possui três raízes distintas, existem três pré-imagens diferentes.
- Logo, o mapa não pode ser injetivo.
- Portanto, não pode ser invertível.
- Assim, toda a dificuldade da conjectura acaba concentrada no estudo desse polinômio de grau 3. 

O papel do discriminante Tao destaca que o discriminante desse cúbico é o objeto que organiza toda a geometria do problema.

Ele separa os casos em que:
- existem três pré-imagens distintas;
- duas raízes coincidem;
- ocorre uma degeneração da fibra.
- Em vez de enxergar milhares de identidades algébricas, o leitor passa a visualizar uma superfície que controla exatamente quando a estrutura muda. 

O comportamento "no infinito"
Um dos pontos mais interessantes do texto é a explicação de por que o jacobiano constante não basta.

Localmente, tudo funciona perfeitamente:

- o mapa é diferenciável;
- o jacobiano nunca se anula;
- em torno de qualquer ponto existe uma inversa local. O problema aparece globalmente.

Existem sequências de pontos que "fogem para o infinito", enquanto suas imagens permanecem limitadas. Esse fenômeno impede que a aplicação seja própria (proper), e é justamente essa falha global que permite que pontos muito distantes acabem sendo enviados para a mesma imagem. 

### A mensagem principal

Para Tao, a descoberta é interessante porque mostra que o contraexemplo não é uma construção artificial cheia de cancelamentos misteriosos. Depois da mudança de perspectiva correta, sua estrutura se torna relativamente simples:

- um mapa polinomial explícito;
- uma parametrização geométrica adequada;
- um polinômio cúbico que controla as fibras;
- um discriminante que explica quando aparecem múltiplas pré-imagens.

O post é, portanto, uma "digestão" da prova original: ele não substitui o artigo técnico, mas fornece a intuição geométrica que torna o contraexemplo compreensível. Em vez de pensar apenas em manipulações algébricas, Tao mostra que a verdadeira razão do fracasso da conjectura está no comportamento global da aplicação e na geometria de suas fibras.

## Postgres consegue escalar muito mais do que as pessoas acham

Postgres consegue escalar muito mais do que a maioria das startups imagina, desde que você conheça seus limites e faça alguns ajustes importantes. Antes de adicionar Kafka, ClickHouse, Redis, ElasticSearch ou outros sistemas especializados, vale a pena explorar tudo o que o Postgres oferece.

Os principais pontos são:

- Comece com Postgres. Ele resolve muito mais problemas do que apenas armazenar dados relacionais. A equipe da Hatchet usa Postgres como base para filas de tarefas, eventos, workflows e diversos outros componentes, evitando aumentar a complexidade operacional.

- Conexões são um gargalo comum. Abrir muitas conexões simultâneas piora o desempenho em vez de melhorar. O ideal é usar um pool de conexões bem configurado (como PgBouncer) e limitar o número de conexões ativas.

- Batching é um dos maiores ganhos de performance. Inserir registros em lotes (INSERT ... VALUES (...), (...), ...) aumenta drasticamente o throughput em comparação com inserir uma linha por vez.

- Particione tabelas grandes. Quando uma tabela cresce para centenas de milhões ou bilhões de linhas, particionamento por data facilita retenção de dados, melhora consultas e reduz problemas de manutenção.

- Autovacuum não resolve tudo. Em tabelas particionadas, é importante executar ANALYZE na tabela pai periodicamente, pois o autovacuum atualiza apenas as partições. Caso contrário, o otimizador pode escolher planos de execução ruins.

- JSONB é excelente, mas tem custo. Armazenar muitos documentos grandes em jsonb pode fazer as tabelas crescerem rapidamente. A Hatchet recomenda manter apenas os dados "quentes" no banco e mover payloads antigos para armazenamento externo (como S3), deixando apenas referências no Postgres.

- Evite adicionar infraestrutura cedo demais. Muitas empresas adotam Kafka, ClickHouse ou bancos de séries temporais antes de realmente precisarem. Em muitos casos, um Postgres bem modelado suporta milhões ou bilhões de registros mantendo uma arquitetura muito mais simples.

### Lições práticas

Se você está construindo um SaaS ou sistema backend, as recomendações práticas do artigo são:

- Use Postgres como primeira opção.
- Configure corretamente o pool de conexões.
- Faça inserts em lote sempre que possível.
- Monitore VACUUM e ANALYZE.
- Particione tabelas que crescem continuamente.
- Não abuse de jsonb para dados grandes e raramente acessados.
- Só introduza bancos especializados quando o Postgres realmente se tornar um gargalo comprovado.

Resumindo: "Use Postgres por muito mais tempo do que você imagina. Ele costuma deixar de ser o gargalo bem depois do que a maioria das startups acredita; o verdadeiro problema geralmente está na forma como ele é utilizado, e não no banco em si."
