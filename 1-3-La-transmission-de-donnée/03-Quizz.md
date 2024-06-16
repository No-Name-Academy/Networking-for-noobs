# La transmission de donnée

<<<<<<< HEAD
## 03. Quizz

Afin de coder les caractères, nous utilisons la table ASCII suivante:

xx	|	0000	|	0001	|	0010	|	0011	|	0100	|	0101	|	0110	|	0111	|	1000	|	1001	|	1010	|	1011	|	1100	|	1101	|	1110	|	1111
----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----
0000	|	NUL	|	SOH	|	STX	|	ETX	|	EOT	|	ENQ	|	ACK	|	BEL	|	BS	|	HT	|	LF	|	VT	|	FF	|	CR	|	SO	|	SI
0001	|	DLE	|	DC1	|	DC2	|	DC3	|	DC4	|	NAK	|	SYN	|	ETB	|	CAN	|	EM	|	SUB	|	ESC	|	FS	|	GS	|	RS	|	US
0010	|	SP	|	!	|	"	|	#	|	$	|	%	|	&	|	'	|	(	|	)	|	*	|	+	|	,	|	-	|	.	|	/
0011	|	0	|	1	|	2	|	3	|	4	|	5	|	6	|	7	|	8	|	9	|	:	|	;	|	<	|	=	|	>	|	?
0100	|	@	|	A	|	B	|	C	|	D	|	E	|	F	|	G	|	H	|	I	|	J	|	K	|	L	|	M	|	N	|	O
0101	|	P	|	Q	|	R	|	S	|	T	|	U	|	V	|	W	|	X	|	Y	|	Z	|	[	|	\	|	]	|	^	|	_
0110	|	`	|	a	|	b	|	c	|	d	|	e	|	f	|	g	|	h	|	i	|	j	|	k	|	l	|	m	|	n	|	o
0111	|	p	|	q	|	r	|	s	|	t	|	u	|	v	|	w	|	x	|	y	|	z	|	{	|		|	}	|	~	|	DEL

Cette table se lit comme suit:

pour un caractère donné, les 4 premiers bits sont ceux de la ligne, et les 4 derniers ceux de la colonne.

Par exemple, le caractère z donne 0111 1010

### Exercice
#### Contexte:

Dans le contexte d'une transmission série sur un switch, via un cable console et un UART RS232, nous entrons les commandes suivantes (tenir compte des retours à la ligne):

`en`

`conf t`

`int fa0/1`

#### Question 1:

En tenant compte du protocole RS232 (le plus simplifié) et du codage ASCII, convertir ces lignes de commandes ci dessus en binaire.
Séparez chaque caractère pour plus de lisibilité, ainsi que les octets de chaque caractère comme dans l'exemple du caractère z ci-dessus, et n'oubliez pas les espaces et retours à la ligne, qui sont des caractères.


#### Question 2:

En quelques mots, à quoi sert chaque ligne de cette commande sur un switch Cisco
=======
## 03. Quizz

Afin de coder les caractères, nous utilisons la table ASCII suivante:

ASCII	|	0000	|	0001	|	0010	|	0011	|	0100	|	0101	|	0110	|	0111	|	1000	|	1001	|	1010	|	1011	|	1100	|	1101	|	1110	|	1111
----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----	|	----
0000	|	NUL	|	SOH	|	STX	|	ETX	|	EOT	|	ENQ	|	ACK	|	BEL	|	BS	|	HT	|	LF	|	VT	|	FF	|	CR	|	SO	|	SI
0001	|	DLE	|	DC1	|	DC2	|	DC3	|	DC4	|	NAK	|	SYN	|	ETB	|	CAN	|	EM	|	SUB	|	ESC	|	FS	|	GS	|	RS	|	US
0010	|	SP	|	!	|	"	|	#	|	$	|	%	|	&	|	'	|	(	|	)	|	*	|	+	|	,	|	-	|	.	|	/
0011	|	0	|	1	|	2	|	3	|	4	|	5	|	6	|	7	|	8	|	9	|	:	|	;	|	<	|	=	|	>	|	?
0100	|	@	|	A	|	B	|	C	|	D	|	E	|	F	|	G	|	H	|	I	|	J	|	K	|	L	|	M	|	N	|	O
0101	|	P	|	Q	|	R	|	S	|	T	|	U	|	V	|	W	|	X	|	Y	|	Z	|	[	|	\	|	]	|	^	|	_
0110	|	`	|	a	|	b	|	c	|	d	|	e	|	f	|	g	|	h	|	i	|	j	|	k	|	l	|	m	|	n	|	o
0111	|	p	|	q	|	r	|	s	|	t	|	u	|	v	|	w	|	x	|	y	|	z	|	{	|		|	}	|	~	|	DEL

Cette table se lit comme suit: pour un caractère donné, les 4 premiers bits sont ceux de la ligne, et les 4 derniers ceux de la colonne.
Par exemple, le caractère z donne 0111 1010

Dans le contexte d'une transmission série sur un switch, via un cable console et un UART RS232, nous entrons les commandes suivantes (tenir compte des retours à la ligne):

`enable`

`conf t`

`int fa0/1`

En tenant compte du protocole RS232 (le plus simplifié) et du codage ASCII, convertir ces lignes de commandes en binaire.
Séparé les octets de caractère comme dans l'exemple du caractère z ci-dessus, et n'oubliez pas les espaces, et retours à la ligne.
>>>>>>> 7bb166d5b3b958e36d48666d3745f8a4c9472937