

```antlr
grammar TableQL;

program: statement* EOF;

statement
  : tableDecl
  | queryDecl
  | insertStmt
  ;

tableDecl
  : 'table' IDENTIFIER '{' columnDef+ '}'
  ;

columnDef
  : IDENTIFIER ':' typeSpec ('primary')?
  ;

typeSpec
  : 'Int'
  | 'String'
  | 'Bool'
  | 'Float'
  ;

queryDecl
  : 'query' IDENTIFIER '{' queryBody '}'
  ;

queryBody
  : 'from' IDENTIFIER whereClause? selectClause orderByClause? limitClause?
  ;

selectClause
  : 'select' IDENTIFIER (',' IDENTIFIER)*
  ;

whereClause
  : 'where' expr comparator expr
  ;

orderByClause
  : 'orderby' IDENTIFIER ('asc' | 'desc')?
  ;

limitClause
  : 'limit' NUMBER
  ;

insertStmt
  : 'insert' 'into' IDENTIFIER '{' fieldAssignment+ '}'
  ;

fieldAssignment
  : IDENTIFIER ':' expr
  ;

expr
  : IDENTIFIER
  | NUMBER
  | STRING
  | expr '+' expr
  | expr '-' expr
  | expr '*' expr
  | expr '/' expr
  ;

comparator
  : '==' | '!=' | '>=' | '<=' | '>' | '<'
  ;

IDENTIFIER: [a-zA-Z_][a-zA-Z0-9_]*;
NUMBER: [0-9]+;
STRING: '"' (~["\\] | '\\' .)* '"';
WS: [ \t\r\n]+ -> skip;
