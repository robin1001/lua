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
每个C文件的导出函数都会使用lua开头，没有lua开头的函数都是static函数。lua后的大写前缀可以标识这个函数所属的文件：
* luaL_loadfile luaL_loadfilex L应该是library的意思，属于lauxlib
* luaD_protectedparser luaD_pcall D是do的意思，属于ldo
* luaU_undump U 是undump的意思，属于lundump
* luaY_parser Y 是代表yacc的意思，lua的parser最早是用过yacc生成的，后来改成手写，名字也保留下来，属于lparser
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
1. luaX_next: 读取下一个token，视lookahead是否为空从lookahead或者流中读取
2. luaX_lookahead: 从流中读取token到lookahead中
3. next: function, read next char to current;
4. save: 保存当前字符current到Mbuffer中，当token为数字或者变量字符串时
5. save_and_next: save+next
6. llex: return current token type,type为int

### Review & Comment
1. zio读取输入抽象过程太多
2. t & lookahead, luaX_next & luaX_lookahead是一个不错的设计


