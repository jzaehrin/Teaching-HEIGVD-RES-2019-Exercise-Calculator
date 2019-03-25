# Protocol de communication client-server pour une calculatrice

Le serveur sera disponible sur le port 1100 en TCP/IP à l'adresse du serveur `jzaehrin.xyz` ou par son IP.

La communication se donnera par des champs séparer de tab.

## Sequence

1. Le serveur commence par notifié sa présence, son et son attente d'une demande. `HELLO\t<operations>:\t[<operation>\t]+\n`
    1. Le client prends connaissance des opérations disponnibles par le serveur.
2. Le client peut maintenant envoyé une liste d'operand/operation de type polonaise inverse,
    c'est à dire que les nombres seront empilés et dépilés deux par deux lors de l'arrivé d'une operation spécifié par un mot clé `OP`. `REQUEST\t<operand>[[\t<operand>]{0,1}\tOP<operation>]+\n`
    Ce schema permet d'être extensible à plusieurs opérations concecutives. Le resultat sera empilé.
3. Le serveur renvoi la première valeur de la pile. `ACK\t<result>\n`
    1. Si une erreur est rencontré, le serveur repondra `NACK\t<message d'erreur>\n`
4. Le cycle recommence à partir du point `2`
    1. Si le client souhaite stoppé la connection, il coupe le canal. Le serveur attendra un timeout.

## Operation possible

- sqrt : 1st operand donne l'exposant
- multi,* : 1st * 2nd
- add,+ : 1st + 2nd
- pow,^ : 2nd^1st
- div,/ : 1st / 2nd
- log   : 1st operand donne la base
- ln    : logarithme naturelle de 1st

## Exemple

- (2 + 4) * 6 : `REQUEST\t2\t4\tOP+\t6\tOP*`
- (2 + 4)^8   : `REQUEST\t2\t4\tOP+\t8\tOP^`
- ln(10)      : `REQUEST\t10\tOPln`
