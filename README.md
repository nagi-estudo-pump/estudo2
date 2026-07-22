# Lean 4

Resumo
O artigo é uma introdução prática ao Lean 4, uma linguagem funcional e assistente de provas usado para verificação formal. O público-alvo são engenheiros de criptografia e desenvolvedores que desejam aprender os fundamentos da verificação formal aplicando-os a um exemplo concreto: a One-Time Pad (OTP). 
H
HashCloak

Os principais conceitos apresentados são:

O que é verificação formal: em vez de confiar apenas em uma prova escrita por humanos, a demonstração é codificada em Lean e verificada automaticamente. Se o compilador aceita a prova, ela é considerada logicamente correta (assumindo a correção do próprio Lean). 
H
HashCloak
Introdução ao Lean: o tutorial explica que Lean é ao mesmo tempo uma linguagem de programação funcional pura e um provador de teoremas. O leitor aprende a usar comandos simples (#eval), a sintaxe da linguagem e conceitos como funções de primeira classe. 
H
HashCloak
Modelagem da One-Time Pad: o objetivo é traduzir a definição matemática da OTP para Lean. Para isso, o autor representa bits usando aritmética modular (ZMod 2) e define um tipo BitString sobre o qual implementa a operação XOR. 
H
HashCloak
Provas sobre o XOR: antes de provar que a OTP está correta, o tutorial demonstra formalmente propriedades fundamentais da operação XOR:
comutatividade;
associatividade;
elemento identidade;
auto-inversão (x xor x = 0). 
H
HashCloak
Definição de um Shannon Cipher: usando uma structure de Lean, o artigo modela um esquema de cifra contendo:
uma função de criptografia;
uma função de descriptografia;
uma propriedade de correção que garante que descriptografar uma mensagem criptografada recupera a mensagem original. 
H
HashCloak
Prova da correção da One-Time Pad: por fim, a OTP é implementada como uma instância de ShannonCipher, usando XOR tanto para cifrar quanto para decifrar. A propriedade de correção é demonstrada automaticamente combinando os lemas provados anteriormente sobre o XOR. 
H
HashCloak
Conclusão
O autor encerra relacionando a verificação formal com projetos modernos de blockchain e criptografia (como Ethereum e Zcash), destacando que essa técnica aumenta a confiança em sistemas críticos. Ao mesmo tempo, ressalta que a verificação formal não elimina todos os riscos, pois ainda é necessário modelar corretamente o sistema e confiar no compilador e nas ferramentas utilizadas. O artigo prepara o terreno para a segunda parte da série, na qual será provada formalmente a segurança perfeita (perfect secrecy) da One-Time Pad. 
