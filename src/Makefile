CC      = gcc
LEX     = flex
YACC    = bison
CFLAGS  = -Wall -std=c11
LIBS    = -lfl -ly
INCLUDE = -Iinclude

SCANNER = scanner
PARSER  = parser

ASTDIR = src/AST/
AST := $(shell find $(ASTDIR) -name '*.c')

VISITORDIR = src/visitor/
VISITOR := $(shell find $(VISITORDIR) -name '*.c')

SRC := $(AST) \
       $(VISITOR)

EXEC = $(PARSER)
OBJS = $(PARSER:=.c) \
       $(SCANNER:=.c) \
       $(SRC)

# Substitution reference
DEPS := $(OBJS:%.c=%.d)
OBJS := $(OBJS:%.c=%.o)

all: $(EXEC)

# Static pattern rule
$(SCANNER).c: %.c: %.l
	$(LEX) -o $@ $<

$(PARSER).c: %.c: %.y
	$(YACC) -o $@ --defines=parser.h -v $<

%.o: %.c
	$(CC) -o $@ $(CFLAGS) $(INCLUDE) -c -MMD $<

$(EXEC): $(OBJS)
	$(CC) -o $@ $^ $(LIBS) $(INCLUDE)

clean:
	$(RM) $(DEPS) $(SCANNER:=.c) $(PARSER:=.c) $(OBJS) $(EXEC)

-include $(DEPS)
