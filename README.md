# Lua Source Code Reading Notes
I have been interested in compiler & interpreter since long long time ago. 
I have read several books and many articals about compilter principle, eg the dragon book, sicp,
compiler in action and written many program, like nfa machine, lexer, parser. 
And I finished mini [assembly interpreter](https://github.com/robin1001/tvm) and 
[calculator compiler & interpreter](https://github.com/robin1001/calculator).
It's really an interesting work. Now I plan to read lua source code since it's
simple, fast, embeding but powerful language, and many people think highly of lua.
May me have a nice trip in reading lua.


## File & Function Name
ÿ��C�ļ��ĵ�����������ʹ��lua��ͷ��û��lua��ͷ�ĺ�������static������lua��Ĵ�дǰ׺���Ա�ʶ��������������ļ���
* luaL_loadfile luaL_loadfilex LӦ����library����˼������lauxlib
* luaD_protectedparser luaD_pcall D��do����˼������ldo
* luaU_undump U ��undump����˼������lundump
* luaY_parser Y �Ǵ���yacc����˼��lua��parser�������ù�yacc���ɵģ������ĳ���д������Ҳ��������������lparser
* luaZ io
* luaX lex

## Lexer
### LexState
```c
/* state of the lexer plus state of the parser when shared by all
   functions */
typedef struct LexState {
  int current;  /* current character (charint) */
  int linenumber;  /* input line counter */
  int lastline;  /* line of last token `consumed' */
  Token t;  /* current token */
  Token lookahead;  /* look ahead token */
  struct FuncState *fs;  /* current function (parser) */
  struct lua_State *L;
  ZIO *z;  /* input stream */
  Mbuffer *buff;  /* buffer for tokens */
  struct Dyndata *dyd;  /* dynamic structures used by the parser */
  TString *source;  /* current source name */
  TString *envn;  /* environment variable name */
  char decpoint;  /* locale decimal point */
} LexState;
```
### Keypoints
1. luaX_next: ��ȡ��һ��token����lookahead�Ƿ�Ϊ�մ�lookahead�������ж�ȡ
2. luaX_lookahead: �����ж�ȡtoken��lookahead��
3. next: function, read next char to current;
4. save: ���浱ǰ�ַ�current��Mbuffer�У���tokenΪ���ֻ��߱����ַ���ʱ
5. save_and_next: save+next
6. llex: return current token type,typeΪint

### Review & Comment
1. zio��ȡ����������̫��
2. t & lookahead, luaX_next & luaX_lookahead��һ����������


